---
title: mysql常用命令
date: '2017/1/14 20:46:25'
categories: mysql
tags: mysql
description: 归纳总结了一些mysql使用过程中常用的命令
---

# mysql常用查询

## 根据b表的某字段值，更新a表的某字段状态

```sql
UPDATE ulala_room_list AS a,
 (
    SELECT
        li.id,is_star,sort
    FROM
        ulala_user_info AS i
    LEFT JOIN ulala_room_list AS li ON i.uid = li.uid
) AS b
SET a.sort = IF(is_star=1,99,0)
WHERE a.id=b.id
```

## 连表查询通过`,`分割的对应条件 如\(options\_id = 1402,1404\)

```sql
SELECT
    *
FROM
    {table_name_1} bol
LEFT JOIN {table_name_2} bmo1 ON (
    bmo1.options_id = SUBSTRING_INDEX(bol.options_id, ',', 1)
)
LEFT JOIN {table_name_2} bmo2 ON (
    bmo2.options_id = SUBSTRING_INDEX(bol.options_id, ',', - 1)
)
WHERE
    {condition}
```

## 优化分页查询

**传统查询**

```sql
SELECT * FROM articles WHERE category_id = 123 ORDER BY id LIMIT 50, 10;
```

**优化查询**

```sql
SELECT * FROM articles WHERE  id >=  
(SELECT id FROM articles  WHERE category_id = 123 ORDER BY id LIMIT 10000, 1) LIMIT 10
```

> 1.使用有索引的列或主键进行order by排序. 2.记录上次返回的主键,在下次查询时使用主键过滤.

