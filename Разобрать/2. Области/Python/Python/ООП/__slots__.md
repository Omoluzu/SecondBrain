#class, #__slots__, #управлениеПамятью #__dict__

**Слоты в классах**

По умолчанию в Python в классах используется словарь `__dict__` для хранения атрибутов, который создается по умолчанию при создании экземпляра класса. Данная особенность позволяет динамически в ран тайме добавлять атрибуты, но от сюда появляться соответствующие проблемы с производительностью.

В случаях, когда мы сразу же точно знаем все атрибуты, используемые в классе, мы можем воспользоваться атрибутом `__slots__`, который позволяет задать ограниченный список аргументов для класса. В этом случае словарь `__dict__` не будет создаваться, что позволит сэкономить память и поднять производительность.

```python
class Student:
	__slots__ = ("name", "mark",)  # recomented used typle()
	def __init__(self, name, mark)
		self.name = name
		self.mark = mark

stud = Student("Alex", 3)
print(stud.name, stud.mark, sep=": ")
>>> Alex: 3

print(stud.__slots__)
>>> ['name', 'mark']

print(stud,__dict__)
>>> AttributeError: 'Student' object has no attribyte '__dict__'

stud.surname = "Volkov"
>>> AttributeError: 'Student' object has no attribute 'surname'
```

> __slots__ не мешает динамически добавлять новые атрибуты класса.
> Это можно обойти с помощью [[__setattr__]] 
```python
Student.surname = "Example"
dir(Student)
>>> [
>>> 	...,
>>> 	surname,
>>> ]
```

> __slots__ не запрещает удалять атрибуты класса
> Это можно обойти с помощью [[__delattr__]] 
```python
del stud.name
print(stud.name)
>>> AttributeError: 'Student' object has no attribute 'name'
```