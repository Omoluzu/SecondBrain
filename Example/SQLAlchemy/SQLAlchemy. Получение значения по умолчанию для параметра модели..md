---
tags:
  - "#sqlalchemy"
aliases:
  - Получение значения по умолчанию для параметра модели
---

```python
default_value = YourModel.__table__.c.your_column.default.arg
```