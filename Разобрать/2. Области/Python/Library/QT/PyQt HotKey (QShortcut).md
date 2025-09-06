

```python
from PyQt5.QtWidgets import QShortcut
from PyQt5.QtGui import QKeySequence
```

```python
self.shart = QShortcut(QKeySequence('Ctrl+Q'), self)
self.shart.activated.connect(<method>)
```

[[PyQt]]