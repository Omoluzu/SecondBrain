---
tags:
  - "#PySide6"
---


**QtRemoteObjects** (QtRO) представляет собой модуль межпроцессного взаимодействия, разработанный для Qt. Его ключевая особенность — возможность обмена информацией между процессами или компьютерами с помощью понятной Qt-разработчикам парадигмы.

Фундаментальная идея **QtRemoteObjects** заключается в полном дублировании Qt-объекта между процессами. Это означает, что:
- Все изменения свойств (properties) в объекте-источнике синхронизируются с объектами-репликами.   
- Сигналы, эмитируемые объектом-источником, также эмитируются в каждой связанной реплике.    
- Взаимодействие между объектами происходит так, будто они находятся в одном процессе


В архитектуре QtRemoteObjects важно понимать разницу между Source (источник) и Replica (реплика):
- **Source** — оригинальный объект, который мы хотим сделать доступным через сеть или между процессами
- **Replica** — легковесный прокси для исходного объекта, который позволяет работать с ним так же, как с любым другим **QObject**




## Реализация Динамической реплики

```python
#source.py

import sys
from PySide6.QtCore import QObject, Property, Signal, Slot, QUrl
from PySide6.QtRemoteObjects import QRemoteObjectHost
from PySide6.QtWidgets import QApplication


class MySource(QObject):
    # Сигнал для уведомления об изменениях
    messageChanged = Signal(str)

    def __init__(self):
        super().__init__()
        self._message = "Начальное сообщение"

    @Property(str, notify=messageChanged)
    def message(self):
        return self._message

    @message.setter
    def message(self, value):
        if self._message != value:
            self._message = value
            self.messageChanged.emit(value)

    @Slot(str)
    def processMessage(self, msg):
        print(f"Сервер обработал: {msg}")


# Создаем приложение
app = QApplication(sys.argv)

# Создаем источник
source = MySource()

# Настраиваем хост
# Пробуем TCP вместо local
host = QRemoteObjectHost(QUrl("tcp://0.0.0.0:12345"))
host.enableRemoting(source, "my_service")

print("Сервер запущен на tcp://0.0.0.0:12345")
print("Объект доступен как 'my_service'")

sys.exit(app.exec())
```

```python
#replica.py
import sys
from PySide6.QtCore import QObject, Slot, QUrl, QMetaObject, Q_ARG, Q_RETURN_ARG
from PySide6.QtRemoteObjects import QRemoteObjectNode
from PySide6.QtWidgets import QApplication

# Создаем приложение
app = QApplication(sys.argv)

# Настраиваем подключение к узлу
node = QRemoteObjectNode()
node.connectToNode(QUrl("tcp://127.0.0.1:12345"))

# Получаем динамическую реплику.
replica = node.acquireDynamic("my_service")

# Ждем подключение в течении 5 секунд.
if replica.waitForSource(5000):
    # Получаем текущее значение из атрибута message
    print(f"Текущее сообщение: {replica.property('message')}")

	# Находим индекс нашего метода в метаданных объекта реплики.
	# Указываем наименование метода и тип передаваемых данных.
	# Тип данных обязательно указываем в типизации Qt
    method_index = replica.metaObject().indexOfMethod("processMessage(QString)")

	# Если индекс равен -1 - метод не найден в реплике
    if method_index == -1:
        print("Ошибка: метод не найден!")
        sys.exit(1)

	# Получем метод из реплики по его индексу
    method = replica.metaObject().method(method_index)
	# Вызываем метод на выполнение.
    result: bool = method.invoke(
	    replica, 
		Q_ARG(str, "Тестовое сообщение")
	)
    print(f"Результат вызова: {result}")

	# Второй вариант делающий то же самое но другим способом
	result: bool = QMetaObject.invokeMethod(
		replica,
		'processMessage',
		Qt.ConnectionType.AutoConnection,
		Q_ARG(str, 'Тестовое сообщение')
	)

else:
    print("Не удалось подключиться к источнику!")

sys.exit(app.exec())
```

В любом случае `result` получит логическое значение которое информирует о успешности вызова метода и нету способов получить ответа от вызываемой функции.


## Реализация статической реплики

На текущий момент я не нашел информации о том как реализовать статическую реплику в PySide6. Дело в том что для статической реплики нужен файл `.rep` для которого в PySide6 нету модуля для корректной его конвертации.

> Говорится что в версии PySide 6.9.0 - это исправили



## Возможная проблема на Виртуальной машине

При использовании `local`
```python
QUrl("local:my_service")
```

В контексте виртуальной машины здесь может возникнуть несколько проблем:
- **Локальный транспорт и права доступа**: Протокол "local:" использует Unix-сокеты, которые могут требовать определенных прав доступа в системе Debian
- **Изоляция виртуальной машины**: Виртуальная машина может изолировать некоторые системные ресурсы, что затрудняет использование локальных сокетов

## Отладка соединения

Для диагностики проблем соединения рекомендуется включить отладочные сообщения:

```python
import os 

os.environ["QT_REMOTE_OBJECTS_DEBUG"] = "1"
```

Это позволит получить больше информации о том, что происходит при попытке соединения.