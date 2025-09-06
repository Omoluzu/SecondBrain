---
tags:
  - "#sqlalchemy"
aliases:
  - Предварительное заполнение таблиц данными
---


**Вывел для себя вот такую структуру, но она пока не является панацеей**

```text
src/
|---- db/
|       |---- ...
|		|---- init_db.py
|---- ...
```

**Главное принцип заполнения**

```python
#db/init_db.py

def create_tables():
    icon.Base.metadata.create_all(
        bind=icon_engine,
        tables=[
            icon.ToolIcon.__table__,
            icon.ToolIconGroup.__table__
        ]
    )

def seed_initial_data():
	with Session(bind=icon_engine) as session:
        if session.query(icon.ToolIconGroup).count() == 0:

			# Частный пример
			milling_cutters = icon.ToolIconGroup(name="Фрезы")
			session.add_all([milling_cutters])
			session.bulk_save_objects([
                icon.ToolIcon(
                    system_name='basic_tool', name='Фрезерный инструмент',
                    group_id=milling_cutters.id
                )
			]
			
if __name__ == "__main__":
    create_tables()
    seed_initial_data()
```

**Ручной запуск (Из под VSCode)**
```bash
.\.venv\Scripts\python.exe -m db.init_db
```