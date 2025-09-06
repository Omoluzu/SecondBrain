[[Python]]

#### Создать проекта
```bash
poetry new <name_new_project>
```

```bash
poetry new --src <name_new_project>
```

#### Установка зависимостей.
```bash
poetry install
```

#### Установка необходимых пакетов
```bash
paetry add <package>
```
> Данный пакет так же будет записан в `pyproject.toml`

#### Обновление зависимостей
```bash
poetry update
```

#### Удаление зависимостей
```bash
poetry remove <package>
```