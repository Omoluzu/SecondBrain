
# Установка
```bash
pip install pre-commit
```

# Создание конфигурационного файла
Создайте файл `.pre-commit-config.yaml` в корне проекта с содержимым:

```yaml
repos:
- repo: https://github.com/pre-commit/pre-commit-hooks
  rev: v4.5.0
  hooks:
    - id: check-yaml
    - id: end-of-file-fixer
    - id: trailing-whitespace
    - id: check-added-large-files

- repo: https://github.com/psf/black
  rev: 24.3.0
  hooks:
    - id: black
      language_version: python3.11

- repo: https://github.com/PyCQA/flake8
  rev: 7.0.0
  hooks:
    - id: flake8

- repo: https://github.com/PyCQA/isort
  rev: 5.13.2
  hooks:
    - id: isort
      name: isort (python)
      args: ["--profile", "black"]

```

# Активация хуков
```bash
# Установите хуки в ваш git репозиторий
pre-commit install

# Для принудительного запуска на всех файлов
pre-commit run --all-files
```

# Использование в разработке
После активации при каждом коммите будут выполняться проверки:

```bash
git add .
git commit -m "Ваше сообщение"

# Пример вывода:
[INFO] Initializing environment for https://github.com/psf/black...
[INFO] Installing environment for https://github.com/psf/black...
[INFO] Once installed this environment will be reused.
black....................................................(no files to check)Skipped
isort (python)...........................................(no files to check)Skipped
flake8...............................................(no files to check)Skipped
```

# Дополнительные команды
## Пропуск проверки
```bash
git commit --no-verify -m "Сообщение"
```
## Обновление версии хуков
```bash
pre-commit autoupdate
```
## Запуск для конкретного файла
```bash
pre-commit run --files path/to/file.py
```

## Настройка исключений
Для кастомизации добавьте в файл `.flake8`:
```ini
[flake8]
exclude = .git,__pycache__,venv,migrations
max-line-length = 88
ignore = E203, W503
```

## Интеграция с IDE
Для автоматического форматирования в VS Code добавьте в `settings.json`:
```json
{
  "editor.formatOnSave": true,
  "python.formatting.provider": "black",
  "python.sortImports.args": ["--profile", "black"]
}
```


## Важные особенности:

1. **Автофикс**: Некоторые хуки (например, black/isort) автоматически исправят ошибки
2. **Кэширование**: Хуки устанавливаются один раз и кэшируются в `.cache/pre-commit`
3. **Производительность**: Проверяются только измененные файлы
4. **Безопасность**: Все репозитории хуков проверяются по SHA-хешам

С этой настройкой вы получаете:
- Автоматическое форматирование кода (Black)
- Сортировку импортов (isort)
- Проверку стиля (Flake8)
- Проверку YAML файлов
- Удаление лишних пробелов