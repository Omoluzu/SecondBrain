
# Инициализация проекта. 

- Создайте изолированное окружение с помощью `pyenv` + `pipenv` или `poetry`
- Инициализируйте проект с улучшенной структурой:
```bash
django-admin startproject config .
mkdir -p apps/{core,users} static/{css,js} config/{settings,requirements}
```

# Настройка окружения 
Разделите настройки по средам (рекомендация из [6](https://djangostars.com/blog/configuring-django-settings-best-practices/)):

```python
# config/settings/
├── __init__.py
├── base.py       # Общие настройки
├── dev.py        # Локальная разработка
├── prod.py       # Продакшен
└── ci.py         # CI/CD
```

Используйте переменные окружения через `python-dotenv`:
```python
# base.py
import os  
from dotenv import load_dotenv  
  
  
load_dotenv()  
SECRET_KEY = os.getenv('SECRET_KEY')
```

# Версионный контроль

- Инициализируйте Git-репозиторий с `.gitignore` для Django
- Настройте [[pre-commit]] хуки для линтинга:

# Настройка тестирования

Настройте pytest с покрытием:

```ini
# pytest.ini
[pytest]
DJANGO_SETTINGS_MODULE = config.settings.dev
python_files = tests.py test_*.py *_tests.py
addopts = --cov=apps --cov-report html
```

# Безопасность
- Настройте `django-environ` для секретов
- Добавьте Security Middleware:
```python
# prod.py
SECURE_HSTS_SECONDS = 31536000
SECURE_SSL_REDIRECT = True
SESSION_COOKIE_SECURE = True
CSRF_COOKIE_SECURE = True
```