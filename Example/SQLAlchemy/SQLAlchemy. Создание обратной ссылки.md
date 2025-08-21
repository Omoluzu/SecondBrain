---
tags:
  - "#sqlalchemy"
  - "#sqlalchemy/relationship"
  - "#sqlalchemy/backref"
  - "#sqlalchemy/back_populates"
  - '#sqlalchemy/ForeignKey'
aliases:
  - Создание обратной ссылки
---

---
- [bacref](#backref)
- [back_populates](#back_populates)
- [Когда использовать каждый вариант](#когда%20использовать%20каждый%20вариант)

---

##### BACKREF
**backref** используется для создания обратное ссылки на другую сторону соотношения. Это позволяет получить доступ к связанным объектам с обеих сторон отношения.
**backref** Создает обратное отношение автоматически. Нам нужно объявить связь только с одной стороны.


```python
from sqlalchemy import Column, Integer, String, ForeignKey
from sqlalchemy.orm import relationship, backref
from sqlalchemy.ext.declarative import declarative_base

Base = declarative_base()

class User(Base):
    __tablename__ = 'users'
    id = Column(Integer, primary_key=True)
    name = Column(String(255), nullable=False)
    addresses = relationship('Address', backref=backref('user', lazy='joined'))

class Address(Base):
    __tablename__ = 'addresses'
    id = Column(Integer, primary_key=True)
    user_id = Column(Integer, ForeignKey('users.id'))
    street = Column(String(255), nullable=False)
```

**backref** может принимать дополнительные аргументы, такие как `lazy`, чтобы определить стратегию загрузки связанных объектов.  
`lazy='joined'` - Указывает на то, что связанные объекты должны загружаться в одном запросе с использованием `JOIN`

**Плюсы:**
- Уменьшение объема кода.
- Удобно для простых приложений или небольших моделей.

**Минусы:**
- Менее явное описание отношения. Связь на другой стороне становится скрытой, что может усложнить понимание кода в больших проектах.

---
##### BACK_POPULATES

Требует явное определение отношения с обеих сторон
```python
class Author(Base):
    books = relationship('Book', back_populates='author')

class Book(Base):
    author_id = Column(Integer, ForeignKey('authors.id'))
    author = relationship('Author', back_populates='books')
```

**Плюсы:**
- Более явное описание отношений, что делает код легче читаемым и поддерживаемым.
- Лучше подходит для больших проектов и сложным моделей.

**Минусы:**
- Требует больше кода

---

##### Когда использовать каждый вариант
- **backref**: Подходит для небольших проектов или когда связь очевидна.
- **back_populates**: Рекомендуется для больших проектов или кода важна ясность кода.