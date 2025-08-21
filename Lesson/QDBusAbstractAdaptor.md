---
tags:
  - "#pyside6"
  - "#dbus"
---

[QDBusAbstractAdaptor](https://doc.qt.io/qt-6/qdbusabstractadaptor.html) - это базовый класс в Qt [[D-Bus]], используемый для создания адаптеров, которые позволяют экспонировать интерфейсы [[QObject]] через [[D-Bus]]. Он служит для того, чтобы предоставлять доступ к методам и сигналам объектов через шину [[D-Bus]]

**Основные функции:**
- **Экспонирование интерфейсов:** Классы, наследующие [[QDBusAbstractAdaptor]], могут определять интерфейсы [[D-Bus]] с помощью макроса `Q_CLASSINFO`, что позволяет экспонировать их методы и свойства через [[D-Bus]]
- **Релай сигналов и вызов методов:** Адаптеры могут автоматически ретранслировать сигналы от родительского объекта в вызывать методы, определенные в адаптере, через [[D-Bus]]
- **Легковесность**: Адаптеры предназначены для того, чтобы быть легковесными обертками, которые просто передают вызовы в реальный объект и сигналы от него.

**Применение:**
- **Гибкость**: Позволяет добавлять поддержку [[D-Bus]] к существующим классам без изменения их кода.
- **Множественные интерфейсы:** Можно создавать несколько адаптеров для одного объекта, чтобы экспонировать разные интерфейсы.

**Пример использования:**
```python
from PySide6.QtCore import QObject, Slot, Signal, Q_CLASSINFO
from PySide6.QtDBus import QDBusAbstractAdaptor, QDBusConnection


# Родительский класс с бизнес-логикой
class ServiceObject(QObject):
    # Сигнал, который будет ретранслирован через D-Bus
    dataChanged = Signal(str)

    def __init__(self):
        super().__init__()
        self._value = ""

    # Внутренний метод для изменения данных
    def update_data(self, new_value):
        self._value = new_value
        self.dataChanged.emit(new_value)


# Адаптер D-Bus
class ServiceAdaptor(QDBusAbstractAdaptor):
    Q_CLASSINFO("D-Bus Interface", "org.example.Service")

    def __init__(self, parent):
        super().__init__(parent)
        self.setAutoRelaySignals(True)  # Включаем автоматическую ретрансляцию сигналов

    # Слот, доступный через D-Bus
    @Slot(str, result=bool)
    def setValue(self, new_value):
        if len(new_value) > 0:
            self.parent().update_data(new_value)
            return True
        return False

    # Свойство, доступное через D-Bus
    @property
    def currentValue(self) -> str:
        return self.parent()._value


# Инициализация сервиса
if __name__ == "__main__":
    import sys
    from PySide6.QtCore import QCoreApplication

    app = QCoreApplication(sys.argv)

    # Создаем объект и адаптер
    service = ServiceObject()
    adaptor = ServiceAdaptor(service)  # Адаптер становится дочерним объектом

    # Регистрируем сервис в сессионной шине D-Bus
    connection = QDBusConnection.sessionBus()
    connection.registerObject("/org/example/Service", service)
    connection.registerService("org.example.Service")

    sys.exit(app.exec())
```

**Ключевые элементы:**
- `Q_CLASSINFO` определяет имя [[D-Bus]] интерфейса.
- `@Slot` декоратор делает метод доступным через [[D-Bus]]
- `setAutoRelaySignals(True)` автоматически ретранслирует сигналы родителя
- `registerObject()` и `reqisterService()` публикуют сервис в шине.

**Проверка работы:**
```bash
# Вызов метода через командную строку
dbus-send --session --dest=org.example.Service \
          --type=method_call \
          /org/example/Service \
          org.example.Service.setValue string:"Hello DBus"
```

**Особенности реализации:**
- Адаптер должен быть дочерним объектом сервиса.
- Сигнал родителя автоматически экспортируются при `setAutoRelaySignals=True`
- Свойства требуют использования декоратора `@property`
