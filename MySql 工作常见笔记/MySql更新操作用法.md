Update 数据的用法
-----------------
>更新一条数据：
```mysql
UPDATE live_number_fun_auth SET state = 2
WHERE circle_id = 'ef3e7d15de9113fd4-7ffe' ORDER BY id DESC LIMIT 1;
```
>JOIN 更新数据：
```mysql
UPDATE orders LEFT JOIN  freports ON freports.order_id = orders.id
SET orders.finish_at = freports.created_at
WHERE orders.id IN (1,2,3)
```