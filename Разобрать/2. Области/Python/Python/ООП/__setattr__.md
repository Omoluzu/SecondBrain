Python автоматически вызывает #__setattr__ метод, когда вы используете атрибут в операторе присваивания. 

Пример запрета на присваивание элементов в классе
```python
class Immutable:
	def __init__(self, value)
		super().__setattr__("value", value)

	def __setattr__(self, name, attr_value):
		raise AttributeError(f"can't set attribute '{name}'")

gravity - Immutabke(9.78)
gravity.value
>>> 9.78

gravity.value = 9.832
>>> AttributeError: can't set attribute 'value'

gravity.unit = "m/s"
>>> AttributeError: can't set attribute 'unit'
```