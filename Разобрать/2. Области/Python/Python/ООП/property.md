Управляемые атрибуты класса.

```python
class Circle:
	def __init__(self, radius)
		self.__radius = radius

	@property
	def radius(self):
		"""The radius property"""
		return self.__radius

	@radius.setter
	def radius(self, value):
		self.__radius = value

	@raduis.deleter
	def radius(self)
		def self.__radius
```

Создание кэшированых свойств `functools.cached_property()`. Это декоратор, позволяет преобразовать метод в кэшированное свойство. Свойство вычисляет свое значение только один раз и кэширует его как обычные атрибут в течении всего времени существования экземпляра.
```python
from functools import cached_property

class Circle:
	def __init__(self, raduis):
		self.__radius = radius 

	@cached_property
	def diameter(self):
		return self.__radius * 2
```
> Данный способ позволяет напрямую изменять свойство атрибута `diameter`.

Если необходимо его не изменять, то необходимо создать кэшированное свойство. которое же допускает модификации, рекомендуеться использовать `property` и `functools.cache` вместе.
```python
from functools import cache

class Circle:
	def __init__(self, radius):
		self.radius = radius

	@property
	@cache
	def diameter(self):
		return self.radius * 2
```
```
