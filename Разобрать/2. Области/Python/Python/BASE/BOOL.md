### BOOL

В Pyhon логический тип `bool` является подкласом `int` и может принимать значения `True` и  `False`.

```python
print(issubclass(bool, int))
>>> True

print(type(True))
>>> <class 'bool'>

print(type(False))
>>> <class 'bool'>

print(isinstance(True, int))
>>> True

print(isinstance(False, int))
>>> True

print(int(True))
>>> 1

print(int(False))
>>> 0
```
 [issubclass](https://pythonz.net/references/named/issubclass/)
 [isinstance](https://pythonz.net/references/named/isinstance/)