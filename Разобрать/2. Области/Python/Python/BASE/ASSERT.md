Оператор `assert` позволяет писать проверки работоспособности кода.

Синтаксис:
```python
assert expression[, assertion_message]
```

Пример использования
```python 
number = 42

assert number > 0
assert number < 0
>> Traceback (most recent call last)
>> ...
>> AssertionError

assert number < 0, f"number greater that 0 expexted, got: {number}"
>> AssertionError: number greater that 0 expexted, got: 42

assert 3>2 and 5<10
assert isinstance(number, int)
assert all([True, True, True])
```

Пример использования assert в коде
```python
def get_response(server, ports=(443, 80)):
	assert len(ports) > 0, f"ports expected a non-empty tuple got: {ports}"
```

Использование assert в описаном примере, является эффективным и действенным способом документирования намерений и предотвращения трудно обнаруживаемых ошибок из-за случайных ошибок или злонамерных действий.



