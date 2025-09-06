
```python
import subprocess

subprocess.run(['python', 'main.py'])  >> CompletedProcess
```

Таким образом мы не можем запустить больше одного процесса за раз

Args:
- `capture_output(bool)` - нужно ли потоки [[stdout]] и [[stderr]] возвращается в объекте `CompletedProcess` а не в консоли.
- `stderr` -  куда перенаправить поток [[stderr]] например `stderr=subprocess.STDOUT`
- `input(str)` - передача аргументов программе, если в ней есть вызов метода `input()`, передает до знака переноса строки. `input='some text\ntext'`