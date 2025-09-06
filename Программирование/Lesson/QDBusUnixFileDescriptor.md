---
tags:
  - "#pyside6"
  - "#dbus"
  - "#file_descriptor"
---


[**QDBusUnixFileDescriptor**](https://doc.qt.io/qt-6/qdbusunixfiledescriptor.html) в Qt предназначен для работы с Unix-[[Дескриптор файлов]] при передаче данных через [[D-Bus]]

**Основное назначение:**
- **Передача дескрипторов через** **D-Bus**: Класс позволяет оборачивать файловые дескрипторы (Например, сокеты, открытые файлы) для отправки их между процессами через механизм **D-Bus**. Дескрипторы автоматически преобразуются в тип `h` стандарта **D-Bus**
- **Безопасное управление ресурсами**: При создании объекта **QDBusUnixFileDescriptor** используется системный вызов [[dup(2)]], который создаёт копию исходного дескриптора. Это предотвращает конфликты при закрытии файлов

**Особенности реализация:**
- **Отсутствие владения дескриптором**: Класс не становится владельцем исходного дескриптора. Пользователь должен самостоятельно управлять его жизненным циклом.
- Проверка поддержки:

```python
from PySide6.QtDBus import QDBusUnixFileDescriptor

QDBusUnixFileDescriptor.isSupported()
```

**Ограничения и рекомендации:**
- **Условия работы**: Требуется [[D-Bus]] библиотека и демон версии 1.4+ на Unix-системах

**Пример использования:**
- Создание и проверка дескриптора
```python
from PySide6.QtDBus import QDBusUnixFileDescriptor
import os

# Проверка поддержки платформой
if not QDBusUnixFileDescriptor.isSupported():
    print("Передача дескрипторов не поддерживается")
    exit()

# Открытие файла и создание дескриптора
fd = os.open("test.txt", os.O_RDWR | os.O_CREAT)
dbus_fd = QDBusUnixFileDescriptor(fd)

print(f"Валиден: {dbus_fd.isValid()}")  # Валиден: True
print(f"Значение: {dbus_fd.fileDescriptor()}")  # Выведет новый fd (не исходный)
```

- Передача через D-Bus (сервер)
```python
from PySide6.QtDBus import QDBusConnection, QDBusMessage

class DBusService:
    @staticmethod
    def get_file_descriptor():
        fd = os.open("data.bin", os.O_RDONLY)
        return QDBusUnixFileDescriptor(fd)

# Регистрация сервиса
connection = QDBusConnection.sessionBus()
connection.registerObject("/", DBusService())
connection.registerService("com.example.service")

# Проверка возможностей соединения
if not (connection.connectionCapabilities() & QDBusConnection.UnixFileDescriptorPassing):
    print("Соединение не поддерживает передачу дескрипторов")
```

- Клиентский код
```python
from PySide6.QtDBus import QDBusInterface

proxy = QDBusInterface(
    "com.example.service",
    "/",
    None,
    QDBusConnection.sessionBus()
)

reply = proxy.call("get_file_descriptor")
if reply.errorMessage():
    print("Ошибка:", reply.errorMessage())
else:
    dbus_fd = reply.arguments()[0]
    fd = dbus_fd.takeFileDescriptor()
    
    # Чтение данных из полученного дескриптора
    with os.fdopen(fd, 'rb') as f:
        data = f.read()
    print(f"Прочитано {len(data)} байт")
```

**Важные особенности:**
- Клонирование дескрипторов:
```python
# Для долгосрочного хранения
import fcntl
original_fd = dbus_fd.fileDescriptor()
new_fd = fcntl.fcntl(original_fd, fcntl.F_DUPFD_CLOEXEC)
```

- Обработка ошибок
```python
try:
    dbus_fd.setFileDescriptor(-5)  # Невалидный дескриптор
except OSError as e:
    print(f"Ошибка установки: {e.strerror}")
```

- Проверка состояния
```python
if dbus_fd.isValid():
    print("Дескриптор готов к передаче")
else:
    print("Некорректный файловый дескриптор")
```

- Дескрипторы автоматически закрываются при уничтожении объекта **QDBusUnixFileDescriptor**, если не использован `takeFileDescriptor()`