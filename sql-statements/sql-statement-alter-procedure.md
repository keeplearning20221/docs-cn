---
title: ALTER PROCEDURE
summary: 平凯数据库中 ALTER PROCEDURE 的使用概况
---

# ALTER PROCEDURE

`ALTER PROCEDURE` 语句用于在当前所选数据库中创建存储过程，与 MySQL 中 `ALTER PROCEDURE` 语句的行为类似。

## 语法图
```ebnf+diagram
AlterProcedureStmt ::=
    'ALTER' 'PROCEDURE' ProcedureName [characteristic ...]

ProcedureName ::=
    Identifier ('.' Identifier)

characteristic ::=
    COMMENT 'string'
  | SQL SECURITY { DEFINER | INVOKER }
```

## 示例
修改一个存储过程：

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
alter procedure t1 comment 'test';
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
Query OK, 0 rows affected (0.01 sec)

mysql> delimiter ;
mysql> show procedure status;
+------+------+-----------+---------+---------------------+---------------------+---------------+---------+----------------------+----------------------+--------------------+
| Db   | Name | Type      | Definer | Modified            | Created             | Security_type | Comment | character_set_client | collation_connection | Database Collation |
+------+------+-----------+---------+---------------------+---------------------+---------------+---------+----------------------+----------------------+--------------------+
| test | t1   | PROCEDURE | root@%  | 2023-08-10 17:36:42 | 2023-08-10 17:36:42 | DEFINER       |         | utf8mb4              | utf8mb4_bin          | utf8mb4_bin        |
+------+------+-----------+---------+---------------------+---------------------+---------------+---------+----------------------+----------------------+--------------------+
1 row in set (0.00 sec)

mysql> alter procedure t1 comment 'test';
Query OK, 0 rows affected (0.00 sec)

mysql> show procedure status;
+------+------+-----------+---------+---------------------+---------------------+---------------+---------+----------------------+----------------------+--------------------+
| Db   | Name | Type      | Definer | Modified            | Created             | Security_type | Comment | character_set_client | collation_connection | Database Collation |
+------+------+-----------+---------+---------------------+---------------------+---------------+---------+----------------------+----------------------+--------------------+
| test | t1   | PROCEDURE | root@%  | 2023-08-10 17:36:42 | 2023-08-10 17:36:42 | DEFINER       | test    | utf8mb4              | utf8mb4_bin          | utf8mb4_bin        |
+------+------+-----------+---------+---------------------+---------------------+---------------+---------+----------------------+----------------------+--------------------+
1 row in set (0.00 sec)
```