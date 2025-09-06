---
tags:
  - "#alacritty/key_bindings"
---

В конфигурационном файла alacritty добавляем следующий параметр.
```yaml
key_bindings:   
	- { key: Back, mods: Control, chars: "\x17" }
```

Перезапускаем терминал
