---
tags:
  - "#sqlalchemy"
  - "#sqlalchemy/relationship"
aliases:
  - Создание связи одной модели к множествам связей другой модели
---

Когда необходимо реализовать записи вида, одно модель имеет несколько аргументов которые ссылаются на другую схожую модель. Но необходимо применять следующую структуру кода.

```python
Base = declarative_base()

class CoordinateSystemAxis(Base):    
	__tablename__ = 'coordinate_system_axis'
    __bind_key__ = 'coordinate_system_axis'
    
	id = Column(Integer, primary_key=True)

class CoordinateSystem(Base):
    __tablename__ = 'coordinate_system'
    __bind_key__ = 'coordinate_system'
    
	id = Column(Integer, primary_key=True)
	
    x_id = Column(Integer, ForeignKey('coordinate_system_axis.id'))
    x = relationship(
       'CoordinateSystemAxis', foreign_keys=x_id, post_update=True)
	
	y_id = Column(Integer, ForeignKey('coordinate_system_axis.id'))
    y = relationship(
       'CoordinateSystemAxis', foreign_keys=y_id, post_update=True)
    
    z_id = Column(Integer, ForeignKey('coordinate_system_axis.id'))
    z = relationship(
       'CoordinateSystemAxis', foreign_keys=z_id, post_update=True)
  

```