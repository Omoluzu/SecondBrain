---
tags:
  - "#pyside6"
  - "#popup"
---

**Popup** - Это элемент интерфейса отображаемое по верх основного приложения и блокирующее его основную работу.

**Пример реализации popup**

```python
from PySide6.QtWidgets import (
	QApplication, QWidget, QPushButton, QLabel, QVBoxLayout)
from PySide6.QtCore import Qt
from PySide6.QtGui import QPalette, QColor
import sys


class Popup(QWidget):
    def __init__(self, parent=None):
        super().__init__(parent)

        # Устанавливаем размеры окна как у родительского виджета
        self.setGeometry(0, 0, parent.width(), parent.height())
        self.setAttribute(Qt.WA_NoSystemBackground, True)  # Убираем системный фон

        # Полупрозрачный фон для затемнения
        self.setAutoFillBackground(True)
        palette = self.palette()
		# Черный с прозрачностью
        palette.setColor(QPalette.Window, QColor(0, 0, 0, 150))  
        self.setPalette(palette)

        # Основной контейнер для содержимого поп-апа
        self.popup_content = QWidget(self)
        self.popup_content.setStyleSheet(
	        "background-color: white; border-radius: 10px;")
        self.popup_content.setGeometry(
            (self.width() - 300) // 2,
            (self.height() - 200) // 2,
            300,
            200,
        )

        # Добавляем содержимое в поп-ап
        layout = QVBoxLayout(self.popup_content)
        label = QLabel("Это всплывающее окно!", self.popup_content)
        label.setAlignment(Qt.AlignCenter)
        layout.addWidget(label)

        close_button = QPushButton("Закрыть", self.popup_content)
        close_button.clicked.connect(self.close_popup)
        layout.addWidget(close_button)

    def close_popup(self):
        self.setParent(None)  # Удаляем виджет из родителя


class MainWindow(QWidget):
    def __init__(self):
        super().__init__()
        self.setWindowTitle("Главное окно")
        self.setGeometry(100, 100, 600, 400)

        # Кнопка для открытия поп-апа
        layout = QVBoxLayout(self)
        open_popup_button = QPushButton("Открыть поп-ап")
        open_popup_button.clicked.connect(self.show_popup)
        layout.addWidget(open_popup_button)

    def show_popup(self):
        # Создаем и отображаем поп-ап как дочерний виджет
        popup = Popup(self)
        popup.show()


if __name__ == "__main__":
    app = QApplication(sys.argv)
    main_window = MainWindow()
    main_window.show()
    sys.exit(app.exec())
```