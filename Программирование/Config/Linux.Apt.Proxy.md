---
tags:
  - '#proxy'
---

```bash
sudo vim /etc/apt/apt.conf
```

```conf
Acquire::http::Proxy "http://{user}:{password}@{ip}:{port}";
```
