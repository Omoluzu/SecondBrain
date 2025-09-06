Позволяет временно подавлять определенные исключения в блоке кода. Это альтернатива [[try-except]], если нам не требуется обрабатывать ошибку а нужно лишь ей игнорировать.

```python
with contextlib.suppress(FileNotFoundError):
	with open('no_exist_file.txt') as file:
		print(file.read())  # Ошибка FileNotFoundError будет проигнорированна
```