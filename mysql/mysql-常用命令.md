### 1.根据b表的某字段值，更新a表的某字段状态
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