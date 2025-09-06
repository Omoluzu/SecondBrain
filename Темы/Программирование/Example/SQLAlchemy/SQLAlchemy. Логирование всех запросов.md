---
tags:
  - "#sqlalchemy"
  - "#logging/config"
aliases:
  - Логирование всех запросов
---


```python
import logging
import logging.config

... # Настройки логирования

logging.config.dictConfig({
	...
	'loggers': {
		...
        'sqlalchemy.engine': {
            'level': 'INFO'
        },
        ...
})
```