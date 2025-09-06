---
tags:
  - "#sqlalchemy"
  - "#pattern/singleton"
aliases:
  - Решение проблемы с множеством открытых сессий.
---

В большом проекте может возникнуть ситуация когда открывается множество открытых сессий к БД и ситуация когда модель открылась в одной сессии и пытается обрабатываться в другой сессии. 
Для решения данной проблемы я решил использовать паттер Singleton. И вот пример моей реализации

```python
class Session:
    _instance = None
    __session = None

    def __new__(cls):
        if not cls._instance:
            cls._instance = super(Session, cls).__new__(cls)
            cls.__session = sessionmaker(binds={
                IconBase: icon_engine,
                ToolBase: tool_engine
            })()

        return cls._instance

    def __getattr__(self, name):
        return getattr(self.__session, name)
```