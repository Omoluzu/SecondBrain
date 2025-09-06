

#### Методы для переопределения
| Наименование                        | Принимаемые аргументы | Описание                                  |
| ----------------------------------- | --------------------- | ----------------------------------------- |
| [mousePressEvent](#mousePressEvent) | [QMouseEvent](QMouseEvent.md)           | Получение событий нажатия мыши по виджету | 



##### mousePressEvent
Получение событий нажатия мыши по виджету
```python
from PyQt5.QtWidgets import QWidget
from PyQt5.QtGui import QMouseEvent

class MyWidget(QWidget):
	def mousePressEvent(self, event: QMouseEvent) -> None:
		...
```