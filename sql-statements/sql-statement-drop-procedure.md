---
title: DROP PROCEDURE
summary: 平凯数据库中 DROP PROCEDURE 的使用概况
---

# DROP PROCEDURE

`DROP PROCEDURE` 语句用于在当前所选数据库中创建存储过程，与 MySQL 中 `DROP PROCEDURE` 语句的行为类似。

## 语法图
```ebnf+diagram
DROPProcedureStmt ::=
    'DROP' 'PROCEDURE' IfExists ProcedureName 

IfExists ::=
    ('IF' 'EXISTS')

ProcedureName ::=
    Identifier ('.' Identifier)
```

## 示例
删除一个存储过程：

{{< copyable "sql" >}}

```sql
use test
delimiter $$
create procedure t1()
begin
select 1;
end $$
delimiter ;
show procedure status;
drop procedure t1;
show procedure status;
```

```sql
mysql> use test
Database changed
mysql> delimiter $$
mysql> create procedure t1()
    -> begin
    -> select 1;
    -> end $$
Query OK, 0 rows affected (0.00 sec)

mysql> delimiter ;
mysql> show procedure status;
+------+------+-----------+---------+---------------------+---------------------+---------------+---------+----------------------+----------------------+--------------------+
| Db   | Name | Type      | Definer | Modified            | Created             | Security_type | Comment | character_set_client | collation_connection | Database Collation |
+------+------+-----------+---------+---------------------+---------------------+---------------+---------+----------------------+----------------------+--------------------+
| test | t1   | PROCEDURE | root@%  | 2023-08-10 17:32:35 | 2023-08-10 17:32:35 | DEFINER       |         | utf8mb4              | utf8mb4_bin          | utf8mb4_bin        |
+------+------+-----------+---------+---------------------+---------------------+---------------+---------+----------------------+----------------------+--------------------+
1 row in set (0.00 sec)

mysql> drop procedure t1;
Query OK, 0 rows affected (0.00 sec)

mysql> show procedure status;
Empty set (0.00 sec)
```