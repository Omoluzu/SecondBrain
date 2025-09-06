Принцип работы
```python
from functools import reduce

def my_add(a, b):
	return a + b

number = [1, 2, 3, 4]
ruduce(my_add, number)
>> 10
```

Необязательным параметром функции `reduce` является `initializer`, для передачи первого аргумента в вызове функции
	
```python
reduce(my_add, number, 100)
>> 110
```

Для поиска произведения всех чисел массива рекомендуется использовать функцию `math.prod` 
```python
from math import prod
prod(number)
>> 24
```

В отличие от reduce фунция `itertools.accumulate()` возвращает итератор. Каждый элемент в этом тераторе будет накоплен результатом вычисления необязательного параметра func который принимает  2 параметра и возвращает 1. Если параметр func не указан будет произведенно сложение итерируемых элементов.
```python
from itertools import accumulate
from operator import mul

list(accumulate(number))
>> [1, 3, 6, 10]

list(accumulate(number, mul))
>> [1, 2, 6, 24]
```