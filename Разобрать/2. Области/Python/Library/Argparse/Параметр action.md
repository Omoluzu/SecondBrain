[[Argparse]]

```python
self.register('action', None, _StoreAction)  
self.register('action', 'store', _StoreAction)  
self.register('action', 'store_const', _StoreConstAction)  
self.register('action', 'store_true', _StoreTrueAction)  
self.register('action', 'store_false', _StoreFalseAction)  
self.register('action', 'append', _AppendAction)  
self.register('action', 'append_const', _AppendConstAction)  
self.register('action', 'count', _CountAction)  
self.register('action', 'help', _HelpAction)  
self.register('action', 'version', _VersionAction)  
self.register('action', 'parsers', _SubParsersAction)  
self.register('action', 'extend', _ExtendAction)
```



`store` - Просто сохраняет значение аргумента

`store_const` - Сохранение значение, указанное аргументом ключевого слова const. Чаше всего используется с необязательным аргументами, указывающий какой то флаг

`store_true` and `store_false` - Используется для хранения значений True и False. Кроме того, они создают значения по умолчанию.

`append` - Сохраняет список и добавляет в него значение каждого аргумента.  Это полезно для того, чтобы параметр можно было указывать несколько раз.

`append_const`  - Это сохраняет список и добавляет значение, указанное аргументом ключевого слова const в список

`count` - Подсчитывает, сколько раз встречается аргумент ключевого слова. Например, это полезно для повышения уровня детализации

`version` - ожидает аргумент version = keyworf в вызове add_argument(), выводит информацию о версии и завершает работу при вызове

`extend` - сохраняет список и расширяет каждое значение аргумента до списка. 