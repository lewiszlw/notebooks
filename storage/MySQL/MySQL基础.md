## BigInt(20) 和 Int(20) 辨别

括号里的数字只用于填充0（ZEROFILL），而跟能存储数字的大小没有区别，所以基本没用，因为一般不会打开ZEROFILL选项。

BigInt存储8字节有符号整数，Int存储4字节有符号整数。

https://stackoverflow.com/questions/3135804/types-in-mysql-bigint20-vs-int20

# SQL优化
1.limit当offset很大时性能低下，改用子查询的分页方式或者JOIN分页方式（使用字段带有索引）。原理：子查询是在索引上完成的，而普通的查询时在数据文件上完成的，通常来说，索引文件要比数据文件小得多，所以操作起来也会更有效率。
```sql
-- 传统limit，文件扫描
[SQL]SELECT * FROM tableName ORDER BY id LIMIT 500000,2;
受影响的行: 0
时间: 5.371s

-- 子查询方式，索引扫描
[SQL]
SELECT * FROM tableName
WHERE id >= (SELECT id FROM tableName ORDER BY id LIMIT 500000 , 1)
LIMIT 2;
受影响的行: 0
时间: 0.274s

-- JOIN分页方式
[SQL]
SELECT *
FROM tableName AS t1
JOIN (SELECT id FROM tableName ORDER BY id desc LIMIT 500000, 1) AS t2
WHERE t1.id <= t2.id ORDER BY t1.id desc LIMIT 2;
受影响的行: 0
时间: 0.278s
```