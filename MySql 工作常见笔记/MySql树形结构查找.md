MySql树形结构查找
-----------------
>树形表结构：
```mysql
create table products(
  id int,
  name varchar(100),
  parent_id int
);
```

>树形数据
```mysql
insert into products values 
(15, 'category15', 0), -- not a descendent of 19
(16, 'category16', 15), -- not a descendent of 19
(19, 'category19', 0),
(20, 'category20', 19), -- level 1
(21, 'category21', 20), -- level 2
(22, 'category22', 21), -- level 3
(23, 'category23', 19), -- level 1
(24, 'category24', 21), -- level 3
(25, 'category25', 22), -- level 4
(26, 'category26', 22), -- level 4
(27, 'category26', 25); -- level 5
```

>树形结构数据，查找某节点下的所有子节点：
```mysql
select id,
        name,
        parent_id 
from    (select id, name, parent_id from products
         order by parent_id, id) products_sorted,
        (select @pv := '19') tmp
where   find_in_set(parent_id, @pv)
and     @pv := concat(@pv, ',', id);
```