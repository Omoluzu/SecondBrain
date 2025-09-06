
### SELECT

Вывести все записи таблицы `name_table`

```sql
SELECT * FROM name_table;

SELECT column1, column2 FROM name_table;
```

Вывести только уникальные значения

```sql
SELECT DISTINCT name_column FROM name_table;
```

> Для подсчета кол-ва отличительных данных можно воспользовать ключевым словом `COUNT`.
> ```sql
> SELECT COUNT(DISTINCT name_column) FROM name_table;
> ```


--- 

- `UPDATE`- обновляет данные в базе данных
- `DELETE`- удаляет данные из базы данных
- `INSERT INTO`- вставляет новые данные в базу данных
- `CREATE DATABASE`- создает новую базу данных
- `ALTER DATABASE`- изменяет базу данных
- `CREATE TABLE`- создает новую таблицу
- `ALTER TABLE`- изменяет таблицу
- `DROP TABLE`- удаляет таблицу
- `CREATE INDEX`- создает индекс (ключ поиска)
- `DROP INDEX`- удаляет индекс