Python автоматически вызывает #__delattr__ метод, когда вы пытаетесь удалить какой либо атрибут

Пример запрета удаления атрибута класса
```python
class Immutable:
	def __init__(self, value)
		super().__setattr__("value", value)

	def __delattr__(self, name):
		raise AttributeError(f"can't delete attribute '{name}'")

gravity - Immutabke(9.78)
gravity.value
>>> 9.78

del gravity.value
>>> AttributeError: can't delete attribute 'value'
```