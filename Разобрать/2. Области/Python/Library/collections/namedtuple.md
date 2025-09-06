
#collection #namedtuple #tuple 

Создание кортежа с именованными полями, которые можно использовать для доступа к элементам данных

```python
from collections import namedtuple

Point = namedtuple("Point", "x y")
point = Point(21, 42)
point
>>> Point(x=21, y=42)

point.x
>>> 21
point.y
>>> 42
```

Создание своих методов для именованных кортежей, через наследование.
```python
import math
from collections import namedtuple

class Point(namedtuple("Point", "x y")):
    __slots__ = ()

    def distance(self, other: "Point") -> float:
        return math.dist((self.x, self.y), (other.x, other.y))
```

Подсказка типов
```python
import math
from typing import NamedTuple

class Point(NamedTuple):
    x: float
    y: float

    def distance(self, other: "Point") -> float:
        return math.dist((self.x, self.y), (other.x, other.y))
```