[[СHANGELOG 6.8.2]]

[LINK](https://bugreports.qt.io/browse/PYSIDE-2936)

**Описание**
A crash using [[QDBusUnixFileDescriptor]] in [[QDBusAbstractInterface]].call() has been fixed.

Исправлен сбой при использовании [[QDBusUnixFileDescriptor]] в [[QDBusAbstractInterface]].call().

**Пример моей реализация для работы с данными классами:**

```python
from PySide6.QtDBus import (
	QDBusUnixFileDescriptor, 
	QDBusAbstractInterface, 
	QDBusConnection
)

if not QDBusUnixFileDescriptor.isSupported():
    print("Передача файловых дескрипторов недоступна")
    exit()

# Пример создания с файловым дескриптором (например, из открытого файла)
fd = open("/path/to/file", "r").fileno()
unix_fd = QDBusUnixFileDescriptor(fd)

interface = QDBusAbstractInterface(
    "org.example.Service",
    "/org/example/Object",
    "org.example.Interface",
    QDBusConnection.sessionBus()
)

# Вызов метода с передачей дескриптора
reply = interface.call("MethodName", unix_fd)
```

**Примечание:** После передачи через `call()` **Qt** автоматически управляет жизненным циклом дескриптора. Не закрывайте [файловый дескриптор](Дескриптор%20файлов.md) вручную после передачи.