
```bash
python3 it/manage.py makemigrations <app> --empty
```

Создаться файл вида: 
```python
# {service}.{app}.migrations.xxxx_auto_{data}.py

from django.db import migrations  
  
  
class Migration(migrations.Migration):  
  
	dependencies = [  
		('direction', '0009_project'),  
	]  
	  
	operations = [  
	]

```


Обновляем созданную миграцию под наши нужды.
```python
...

def combine_names(apps, schema_editor):  
	# Логика кастомной миграции.
  
  
class Migration(migrations.Migration):  
  
	dependencies = [  
		('direction', '0009_project'),  
		...  # Можем добавить зависимостей
	]  
	  
	operations = [  
		migrations.RunPython(combine_names),  
	]

```


Подключение моделей в миграцию
```python
def combine_names(apps, schema_editor):
	app_model = apps.get_model(<app>, <name_model>)
```

Подключение модели User
```python
from django.contrib.auth import get_user_model

def combine_names(apps, schema_editor):
	user_model = get_user_model()
```