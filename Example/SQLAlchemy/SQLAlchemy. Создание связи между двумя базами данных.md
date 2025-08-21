---
tags:
  - "#sqlalchemy"
  - "#sqlalchemy/binds"
  - "#sqlalchemy/models/__bind_key__"
  - "#app_structure"
aliases:
  - Создание связи между двумя базами данных
---

Для работы с несколькими базами данных в SQLAlchemy используется механизм привязок (**binds**)

**Примерная структура проекта**
```text
src/
|---- app/
|       |---- __init__.py
|       |---- main.py
|---- db/
|       |---- __init__.py
|       |---- connection.py
|		|---- config.py
|       |---- models/
|       |       |---- __init__.py
|       |       |---- main.py
|       |       |---- analytics.py
|---- requirements.txt
```


**Конфигурируем проект.** 
```python
# config.py
MAIN_DB_URL = 'sqlite:///main.db'
ANALYTICS_DB_URL = 'sqlite:///analytics.db'
```

**Описываем движки для подключения к базе данных**
```python
# db/connection.py
from sqlalchemy import create_engine
from config import MAIN_DB_URL, ANALYTICS_DB_URL

main_engine = create_engine(MAIN_DB_URL)
analytics_engine = create_engine(ANALYTICS_DB_URL)
```

**Определяем модели для баз данных** 
Указываем привязку через атрибут `__bind_key__`
```python
# db/models/main_db.py
from sqlalchemy import Column, Integer, String
from sqlalchemy.ext.declarative import declarative_base

Base = declarative_base()

class User(Base):
    __tablename__ = 'users'
    __bind_key__ = 'main'
    id = Column(Integer, primary_key=True)
    name = Column(String)
    email = Column(String)
```
```python
# db/models/analytics_db.py
from sqlalchemy import Column, Integer, String
from sqlalchemy.ext.declarative import declarative_base

Base = declarative_base()

class LogEntry(Base):
    __tablename__ = 'logs'
    __bind_key__ = 'analytics'
    id = Column(Integer, primary_key=True)
    event = Column(String)
```

**Инициализируем модели и движки**[^session]
```python
# db/__init__.py
from .connection import main_engine, analytics_engine
from .models.main import Base as MainBase
from .models.analytics import Base as AnalyticsBase

# Регистрация привязок
from sqlalchemy.orm import sessionmaker

Session = sessionmaker(binds={
    MainBase: main_engine,
    AnalyticsBase: analytics_engine
})
```

[^session]: [[SQLAlchemy. Решение проблемы с множеством открытых сессий.|Решение проблемы с множеством открытых сессий.]]