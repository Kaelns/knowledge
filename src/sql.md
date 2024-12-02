# Sql Postgres DB knowledge base

<div>
  <p align="center">
    <img src="./assets/sql.png" alt="GreatFrontEnd JavaScript Interview Questions" width="100%">
  </p>
</div>

## Resources

[Yandex Learn](https://practicum.yandex.com/trainer/sql-database-basics)
[Yandex Selecting and Filtering Cheetsheet](./assets/topic_filtering.pdf)

### Table of Contents

<!-- TOC_START -->

| No. | Theme                                           |
| --- | ----------------------------------------------- |
| 1   | [SELECT](#select-data-from-db)                  |
| 2   | [CREATE table](#create-table)                   |
| 3   | [INSERT data to tables](#insert-data-to-tables) |
| 4   | [ALTER TABLE](#alter-table)                     |
| 5   | [UPDATE](#update)                               |
| 6   | [IS NULL](#is-null)                             |
| 7   | [WHERE](#where)                                 |
| 8   | [UPDATE](#update)                               |
| 9   | [DELETE FROM](#delete-from)                     |
| 10  | [LIKE](#like)                                   |
| 11  | [CASE](#case)                                   |
| 12  | [Aggregate functions](#aggregate-functions)     |
| 13  | [GROUP BY](#group-by)                           |
| 14  | [ORDER BY](#order-by)                           |
| 15  | [JOIN ON](#join-on)                             |
| 16  | [UNION](#union)                                 |
| 17  | [WITH](#with)                                   |
| 18  | [UNION](#union)                                 |
| 19  | [Dates](#dates)                                 |

<!-- TOC_END -->

```sql
-- Get table structure
SELECT column_name, data_type, character_maximum_length
FROM INFORMATION_SCHEMA.COLUMNS
WHERE table_name = 'buyer';
```

<!-- QUESTIONS_START -->

1. #### SELECT data from DB

```sql
SELECT [DISTINCT] [ column_name / * ] AS 'Alias', films
FROM [ table name ]
LIMIT 10 OFFSET 5 -- 10 rows from 6th row. Offset like skip
```

- `AS` - renames columns or table
- `DISTINCT` - returns unique values
- `OFFSET` can work without limit. If you need to skip some amount of the rows

2. #### CREATE table

- `PRIMARY KEY` constraint can be used to uniquely identify the row.
- `UNIQUE` columns have a different value for every row.
- `NOT NULL` columns must have a value.
- `DEFAULT` assigns a default value for the column when no value is specified.

```sql
CREATE TABLE student (
  id INTEGER PRIMARY KEY,
  name TEXT UNIQUE,
  grade INTEGER NOT NULL,
  age INTEGER DEFAULT 10
);
```

**[⬆ Back to Top](#table-of-contents)**

3. #### INSERT data to tables

```sql
  -- Insert into columns in order:
INSERT INTO table_name
VALUES (value1, value2);

-- Insert into columns by name:
INSERT INTO table_name (column1, column2)
VALUES (value1, value2);
```

**[⬆ Back to Top](#table-of-contents)**

4. #### ALTER TABLE

Is used to modify the columns of an existing table

```sql
ALTER TABLE celebs
ADD COLUMN twitter_handle TEXT;
```

All rows get `NULL (∅)` after that. It represents missing or unknown data

**[⬆ Back to Top](#table-of-contents)**

5. #### UPDATE

Is used to edit `records` (rows) in a table. It includes a `SET` clause that indicates the column to edit and a `WHERE` clause for specifying the record(s).

```sql
UPDATE table_name
SET column1 = value1, column2 = value2
WHERE some_column = some_value;
```

**[⬆ Back to Top](#table-of-contents)**

6. #### IS NULL

Is condition that return true/false
**[⬆ Back to Top](#table-of-contents)**

7. #### WHERE

Clause to select rows.
`WHERE twitter_handle IS [NOT] NULL`
`WHERE id = 4 AND year > 2014;`
`WHERE NOT id = 4 OR state = 'NY';`
`WHERE age > 25 AND first_name [NOT] IN ('Виктор', 'Любовь', 'Борис', 'Станислав', 'Алина', 'Евгения', 'Ольга');`
`WHERE year BETWEEN 1990 AND 1999;` // from 1990 up to, and including 1999
`WHERE name BETWEEN 'A' AND 'J';` // Starts with A or B or C to I, but not including J
`WHERE name LIKE '%man%';`

- `WHERE` can't use pseudonyms cause `SELECT` runs after `WHERE`
- First to run is `NOT`, then `AND`, after `OR`. With the help of brackets `()` we can prioritize the logic

**[⬆ Back to Top](#table-of-contents)**

8. #### UPDATE

```sql
UPDATE celebs
SET twitter_handle = '@taylorswift13'
WHERE id = 4;
```

**[⬆ Back to Top](#table-of-contents)**

9. #### DELETE FROM

Deletes 1 or more rows where value `IS / =` `[ record ]`.
If where is omitted all `[ record ]`s will be deleted

```sql
DELETE FROM celebs
WHERE twitter_handle IS NULL;
```

**[⬆ Back to Top](#table-of-contents)**

10. #### LIKE

`LIKE` operator can be used inside of a `WHERE` clause to match a specified pattern.

```sql
SELECT name
FROM movies
WHERE name LIKE '%man%'; // -> Iron man 3
```

- `_` Wildcard matches any single unspecified character
- `%` Wildcard matches zero or more unspecified character(s)

**[⬆ Back to Top](#table-of-contents)**

11. #### CASE

`LIKE` operator can be used inside of a `WHERE` clause to match a specified pattern.

```sql
SELECT name,
  CASE
    WHEN genre = 'romance' OR genre = 'comedy' THEN 'Chill'
    WHEN rating > 8 THEN 'Bestseller' -- rewrites previous "Chill" row value
    ELSE 'Intense'
  END AS 'Mood' -- Or column will have name "Case when... "
FROM movies;
```

- `_` Wildcard matches any single unspecified character
- `%` Wildcard matches zero or more unspecified character(s)

**[⬆ Back to Top](#table-of-contents)**

12. #### Aggregate functions

- `COUNT` takes a column and counts the number of non-empty values in that column.

```sql
SELECT COUNT(*)
FROM table_name
WHERE price = 0;
```

- `SUM` takes a column and returns the sum of all the values in that column
- `MAX / MIN`
- `AVG`
- `ROUND(price, 0)` second parameter - integer, that specify decimals

```sql
SELECT ROUND(AVG(price), 2)
```

```sql
SELECT SUM(*)
FROM table_name;
```

**[⬆ Back to Top](#table-of-contents)**

13. #### GROUP BY

`GROUP BY` is a clause in SQL that is used with aggregate functions in `SELECT` statement
to arrange identical data into groups. Comes after `WHERE`, but before `ORDER BY`
`HAVING` - is like `WHERE` but for aggregate property. Add it after `GROUP BY`

```sql
SELECT price, COUNT(*)
FROM fake_apps
GROUP BY price, category; -- or use 1 , 2 , 3 to select corresponding column
HAVING COUNT(*) > 10
```

| price | COUNT(\*) |
| ----- | --------- |
| 0.0   | 73        |
| 0.99  | 43        |
| 1.99  | 42        |
| 2.99  | 21        |
| 14.99 | 12        |

```sql
SELECT category, price, AVG(downloads)
FROM fake_apps
GROUP BY 1, 2;
```

| category | price | AVG(downloads)   |
| -------- | ----- | ---------------- |
| Books    | 0.0   | 11926.5          |
| Books    | 0.99  | 27709.5          |
| Books    | 1.99  | 21770.3333333333 |
| Books    | 2.99  | 16281.0          |
| Business | 0.0   | 14744.25         |
| Business | 0.99  | 15753.0          |
| Business | 1.99  | 18155.5          |
| Business | 2.99  | 19598.5          |
| Business | 14.99 | 28488.0          |

**[⬆ Back to Top](#table-of-contents)**

14. #### ORDER BY

```sql
SELECT price
FROM fake_apps
GROUP BY price DESC; -- or use 1 , 2 , 3 to select corresponding column
```

**[⬆ Back to Top](#table-of-contents)**

15. #### JOIN ON

Ordinary or `INNER JOIN` on joins tables by id but if id doesn't match - it does't include that rows

```sql
SELECT orders.order_id,
   customers.customer_name
FROM orders
JOIN customers
  ON orders.customer_id = customers.customer_id;
```

<div>
  <p align="center">
    <img src="./assets/inner-join.webp" alt="Inner Join" width="100%">
  </p>
</div>

```sql
SELECT *
FROM table1
LEFT JOIN table2
  ON table1.c2 = table2.c2;
```

<div>
  <p align="center">
    <img src="./assets/left-join.webp" alt="Left Join" width="100%">
  </p>
</div>

`CROSS JOIN` without on adds all rows of 2nd table to every 1st table.

| shirt_color | pants_color |
| ----------- | ----------- |
| white       | light denim |
| white       | black       |
| grey        | light denim |
| grey        | black       |
| olive       | light denim |
| olive       | black       |

**[⬆ Back to Top](#table-of-contents)**

16. #### UNION

```sql
SELECT *
FROM table1
UNION
SELECT *
FROM table2;
```

- Tables must have the same number of columns.
- The columns must have the same data types in the same order as the first table.

**[⬆ Back to Top](#table-of-contents)**

17. #### WITH

```sql
WITH previous_results AS (
   SELECT ...
   ...
   ...
   ...
)
SELECT *
FROM previous_results
JOIN customers
  ON _____ = _____;
```

- The `WITH` statement allows us to perform a separate query (such as aggregating customer’s subscriptions)
- `previous_results` is the alias that we will use to reference any columns from the query inside of the WITH clause
- We can then go on to do whatever we want with this temporary table (such as join the temporary table with another table)

**[⬆ Back to Top](#table-of-contents)**

18. #### Dates

1) `DATE_TRUNC` is a feature from PostgreSQL. It returns **timestamp with time zone** type like `2023-03-01 00.00.00.` from `('month, '2023-03-04')`.

`DATE_TRUNC('[time below]', record)`

- `microseconds`;
- `milliseconds`;
- `second`;
- `minute`;
- `hour`;
- `day`;
- `week`;
- `month`;
- `quarter`;
- `year`;
- `decade`;
- `century`.

`SELECT date, DATE_TRUNC('month', date)`

| date       | date_trunc |
| ---------- | ---------- |
| 2022-02-17 | 2022-02-01 |
| 2022-02-22 | 2022-02-01 |
| 2022-03-04 | 2022-03-01 |

To filter: `WHERE DATE_TRUNC('month', date) = '2022-02-01'`

2. `EXTRACT([TIME] FROM [record])` returns specific part of a date (double precision)

- `CENTURY`
- `YEAR`
- `QUARTER`
- `MONTH`
- `WEEK`
- `DAY`
- `DOY` (day of the year) — from 1 to 365 or 366 if it's a leap year;
- `DOW` (day of the week) — from 0 to 6, where monday — 1, sunday — 0;
- `ISODOW` (day of the week and ISO 8601) — from 1 to 7, where monday — 1, sunday — 7;
- `HOUR`
- `MINUTE`
- `SECOND`
- `MILLISECOND`

```sql
SELECT date,
       EXTRACT(WEEK FROM date)
FROM hotdog
LIMIT 5;
```

To filter: `WHERE EXTRACT(WEEK FROM date) = 7`

**[⬆ Back to Top](#table-of-contents)**

16. #### UNION

```sql
SELECT DATE_TRUNC('month', date)
```

**[⬆ Back to Top](#table-of-contents)**
