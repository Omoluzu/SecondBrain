
```python
from django.contrib.auth.models import User
from django.contrib.auth.hashers import make_password

User(  
    username=user,  
    password=make_password(password),  
).save()
```