MySQL字段管理
=============
>MySQL添加字段的方法并不复杂，下面将为您详细介绍MySQL添加字段和修改字段等操作的实现方法，希望对您学习MySQL添加字段方面会有所帮助。

##### 登录数据库
```mysql
mysql -u root -p 数据库名称
```
##### 查询所有数据表
```mysql
show tables;
```

##### 查询表的字段信息
```mysql
desc 表名称;
```
#### MySQL字段管理
##### 添加表字段
```mysql
alter table table1 add transactor varchar(10) not Null COMMENT 'null 或 0 为非会员；1 为会员';
alter table table1 add id int unsigned not Null auto_increment primary key
```
##### 修改某个表的字段类型及指定为空或非空
```mysql
alter table 表名称 change 字段名称 字段名称 字段类型 [是否允许非空] COMMENT '备注内容';
alter table 表名称 modify 字段名称 字段类型 [是否允许非空] COMMENT '备注内容';
alter table 表名称 modify 字段名称 字段类型 [是否允许非空] COMMENT '备注内容';
```
##### 修改某个表的字段名称及指定为空或非空
```mysql
alter table 表名称 change 字段原名称 字段新名称 字段类型 [是否允许非空  COMMENT '备注内容'
```
##### 如果要删除某一字段，可用命令：
```mysql
ALTER TABLE mytable DROP 字段 名;
```