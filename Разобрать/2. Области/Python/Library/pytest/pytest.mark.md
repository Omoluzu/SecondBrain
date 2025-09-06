tags: #mark #pytest

[Исходная статья](https://pytest-docs-ru.readthedocs.io/ru/latest/example/markers.html)

---


### Регистрация своих маркеров

```ini
# pytest.ini
[pytest]
markers = 
    website: mark a test as a website
```

```bash
pytest --markers
>> @pytest.mark.webtest: mark a test as a webtest.
>> ...
```

```python
import pytest

@pytest.mark.webtest
class TestClass:
    ...

class TestClass:
    pytestmark = pytest.mark.webtest

class TestClass:
    pytestmark = [pytest.mark.webtest, pytest.mark.slowtest]
```

```bash
pytest -m webtest
```

```bash
pytest -m "not webtest"
```