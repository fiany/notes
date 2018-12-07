### MySQL 向上递归查询树节点

```mysql
SELECT
	T2.form_id 
FROM
	(
SELECT
	@r AS _id,
	( SELECT @r := form_id FROM yy_promotion_relation_record WHERE to_id = _id ) AS form_id,
	@l := @l + 1 AS lvl 
FROM
	( SELECT @r := '2a', @l := 0 ) vars,
	yy_promotion_relation_record h 
	) T1
	JOIN yy_promotion_relation_record T2 ON T1._id = T2.to_id 
ORDER BY
	T1.lvl DESC
```

>form_id为父节点
>
>to_id为子节点

