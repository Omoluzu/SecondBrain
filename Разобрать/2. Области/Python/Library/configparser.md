---
tags:
  - configparser
  - ini
---



```ini
[section1]  
option1 = value1

[section2]
option2 = ${section1:option1}
```


```python
import configparser

config = configparser.ConfigParser(  
    interpolation=configparser.ExtendedInterpolation()
)

config.read(<config_path>, encoding='utf-8')

config.get('section2', 'option2')  # value1


```