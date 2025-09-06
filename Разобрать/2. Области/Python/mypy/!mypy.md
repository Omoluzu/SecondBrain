
[[setup.cfg]]


---
### Установка

```bash
pipenv install --dev mypy
```

---

### Запуск

```bash
pipenv run mypy script.py
```

---
###  Настройка

```ini
#setup.cfg
[mypy]
ignore_missing_imports = True
```

`ignore_missing_imports` - Игнорирование проблем аннотаций типов во всех зависимостях используемых в нашей программе.