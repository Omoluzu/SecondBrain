---
tags:
  - "#python/pip"
  - "#proxy"
---

**Ручной режим:**
```bash
python3 -m pip install --proxy http://<user>:<password>@<server>:<port> package
```

**Через конфигурационный файла**
- Создаем файл если он еще не создан `.config/pip/pip.conf`
- Заполняем следующим содержимым.
```ini
[global]
proxy = http://<user>:<password>@<server>:<port>
```