
tags: #repr #str

### Пример использования `__repr__` и `__str__`

```python
class Book:
	def __init__(self, title, author):
		self.title = title
		self.author = author
		
	def __repr__(self):
		class_name = type(self).__name__
		return f"{class_name}(title={self.title!r}, author={self.author!r})"
	
	def __str__(self):
		return f'"{self.title}" by {self.author}'

```