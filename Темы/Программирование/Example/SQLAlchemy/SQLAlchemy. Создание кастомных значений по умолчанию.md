---
tags:
  - "#sqlalchemy/listen_for"
aliases:
  - Создание кастомных значений по умолчанию
---


Пример создание значения position, максимального от уже заведенных в базу данных, если аргумент не был передан для создания.

```python
from sqlalchemy import Column, Integer, ForeignKey, func
from sqlalchemy.event import listens_for


class ToolArchive(Base):
    __tablename__ = 'tool_archive'
    id = Column(Integer, primary_key=True)
    position = Column(Integer, unique=True)
    tool_id = Column(Integer, ForeignKey('tool.id'))


@listens_for(ToolArchive, 'before_insert')
def before_insert(mapper, connection, target):
    from sqlalchemy.orm import Session
    session = Session(bind=connection)

    if not target.position:
        max_position = session.query(func.max(ToolArchive.position)).scalar()
        target.position = (max_position or 0) + 1
```