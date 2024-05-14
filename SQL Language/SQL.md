# SQL文法と注意点

## 基礎

### 比較演算子

```sql
=
<
>
<=
>=
```

### LIKE

```sql
SELECT * FROM t 
WHERE col LIKE '%ABC%';
```

前后的`%`代表向前，向后匹配，可以单独用，也可一起用

### NOT/NULL

```sql
SELECT * FROM t WHERE NOT col = 'ABC';

SELECT * FROM t WHERE col IS NULL;
SELECT * FROM t WHERE col IS NOT NULL;
```

`NOT`通常用在`WHERE`后，而不是比较符的前面。当判定涉及`NULL`时，需要用`IS NOT`。
`NULL`的判定用`IS`而不是`=`比较符

### AND/OR

```sql
AND
OR
```

### ORDER BY：昇順・降順

```sql
-- ASC：昇順
-- DESC：降順
SELECT * FROM t ORDER BY col DESC;
```

### LIMIT：限定取得的数据数目

```sql
SELECT * FROM t LIMIT 5;
```

## 高級

### DISTINCT・去重・重複除く

要用括号包裹，注意！

```sql
SELECT DISTINCT(col) FROM t;
```

### 四則演算・加减乘除

```sql
-- +、-、*、/
SELECT col * 1000 FROM t;
```

### 集計関数・统计函数（SUM、AVG、COUNT、MAX、MIN）

```sql
-- SUM
SELECT SUM(col) FROM t;

-- AVG
SELECT AVG(col) FROM t;

-- COUNT
SELECT COUNT(col) FROM t;

-- MAX・MIN
SELECT MAX(col) FROM t;
SELECT MIN(col) FROM t;
```

### GROUP BY：グループ化と集計・获取分组后的统计结果

`GROUP BY`を用いる場合、`SELECT`で使えるのは、`GROUP BY`に指定しているカラム名と、**集計関数**のみです。

也就是说当使用`GROUP BY`时，`SELECT`只能获取被`GROUP BY`指定的列名，以及统计函数（内部可包裹其他列名）。

可以理解为，通过获得分组后的目标列，以及对应的统计值，来展示分组统计的效果，所以一般都是采用统计函数和分组值的SELECT内容。虽然使用不分组值也不会报错，但是从目的上来看，这么做是没有意义的。因为不能同时展示每个分组下所有的非分组值，只会显示一个，所以统计函数是必要的。

```sql
⭕️ SELECT SUM(col1) FROM t GROUP BY col1;
⭕️ SELECT SUM(col2), col1 FROM t GROUP BY col1;
❌ SELECT col1 FROM t GROUP BY col1; -- 无统计函数
❌ SELECT SUM(col1), col2 FROM t GROUP BY col1; -- 获得的col2无意义
```

也可以使用复数个列名来分组，用`,`分隔，例子如下。

```sql
SELECT SUM(col1), co1, col2 FROM t GROUP BY col1, col2;
```

和`WHERE`的并用，应该写在`WHERE`之后

```sql
SELECT SUM(col1), co1, col2 FROM t 
WHERE t = 1 
GROUP BY col1, col2;
```

### HAVING：仅针对GROUP的筛选

`HAVING`用于`GROUP BY`后，用于筛选已经经过分组，计算后的数据的再次筛选。

和`WHERE`不同，`WHERE`生效对象是`FROM`所指定的表格，而`HAVING`生效对象是分组后的结果，两者生效的顺序和范围有所不同。

所以会先计算`WHERE`，然后计算`GROUP BY`，然后计算`SELECT`中的统计函数，最终得到的结果，再由`HAVING`来筛选，所以`HAVING`仅可使用`SELECT`筛选结果中存在的列名。

```
SELECT SUM(col1), co1, col2 FROM t
WHERE t = 1
GROUP BY col1, col2
HAVING SUM(col1) > 100;
```

### AS：为取得数据赋名

```sql
SELECT name AS '名前' FROM t;
```

### JOIN ON：結合・结合

```sql
SELECT * FROM t
JOIN t1
ON t.col = t1.col1;
```

### LEFT JOIN：

如同字面意思，向左JOIN，即保留左侧（FROM侧）的表格的整体结构，将右侧表格（JOIN侧）向左合并。不会出现当左侧表格中特定行不存在外键数据时，该行会在普通的JOIN中被略过的问题。

```sql
-- t表的数据和结构将全部保留，即使存在不包含t1表的外键的数据，也不会被省略。
SELECT * FROM t
LEFT JOIN t1
ON t.col = t1.col1;
```

### JOINを複数回使用

```sql
SELECT * FROM t
JOIN t1
ON t.col = t1.col1
LEFT JOIN t2
ON t.col = t2.col2;
```

## CRUD

### INSERT

AUTO INCREMENT：即ID自增，无需手动在insert句中填入

```sql
INSERT INTO students (name, course) 
VALUES('Kate', 'Java');
```

### UPDATE

WHERE不指定的话就会更新所有的数据行

```sql
UPDATE students SET name='',course='' WHERE id = 1
```

### DELETE

```sql
DELETE FROM students WHERE id = 1;
```

## 実行順番

上から下へ

FROM

ON・JOIN

WHERE

GROUP BY

関数（COUNT・SUM・AVG・MAX・MIN）

HAVING

SELECT・DISTINCT

ORDER BY

LIMIT
