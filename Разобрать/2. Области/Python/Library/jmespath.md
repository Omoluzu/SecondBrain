
`Jmespath` - это язык запросов, разработанный для json с плагином достыпным для Python, который работает  со словарём Python 
```bash
pip install jmespath
```

Чтобы начать работы, вызываем со сторокой запроса в качетсве первого аргумента и данными в качестве второго.
```python
import jmespath

jmespath.search("network.lines", data)
>> [
>> 	{'name.en': 'Ginza', 'name.jp': ***, 'color': 'orange', 'number': 3, 'sing': 'G'},
>> 	{'name.en': 'Marunouchi', 'name.jp': ***, 'color': 'red', 'number': 4, 'sing': 'M'}
>> ]
```

Можно использовать квадратные скобки для предоставление запроса
```python
jmespath.search("network.lines[*].number", data)
>> [3, 4]

jmespath.search("network.lines[?number=='3']", data)
>> [{'name.en': 'Ginza', 'name.jp': ***, 'color': 'orange', 'number': 3, 'sing': 'G'}]

jmespath.search("network.lines[?number=='3'].color", data)
>> ['orange']
```

Пример используемой `data` ниже
```python
data = {
	"network": {
		"lines": [
			{'name.en': 'Ginza', 'name.jp': ***, 'color': 'orange', 'number': 3, 'sing': 'G'},
			{'name.en': 'Marunouchi', 'name.jp': ***, 'color': 'red', 'number': 4, 'sing': 'M'}
		]
	}
}
```