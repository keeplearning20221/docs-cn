---
title: SHOW PROCEDURE
summary: 平凯数据库中 SHOW PROCEDURE 的使用概况
---

# SHOW PROCEDURE

`SHOW PROCEDURE` 语句用于在当前所选数据库中创建存储过程，与 MySQL 中 `SHOW PROCEDURE` 语句的行为类似。

## 语法图
```ebnf+diagram
ShowProcedureStatus ::= SHOW PROCEDURE STATUS
    [LIKE 'pattern' | WHERE expr]

ShowCreateProcedure ::= SHOW CREATE PROCEDURE ProcedureName

ProcedureName ::=
    Identifier ('.' Identifier)
```

## 示例
显示存储过程：

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
show create procedure t1;
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
| test | t1   | PROCEDURE | root@%  | 2023-08-10 21:25:05 | 2023-08-10 21:25:05 | DEFINER       |         | utf8mb4              | utf8mb4_bin          | utf8mb4_bin        |
+------+------+-----------+---------+---------------------+---------------------+---------------+---------+----------------------+----------------------+--------------------+
1 row in set (0.00 sec)

mysql> show create procedure t1;
+-----------+-------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------+----------------------+----------------------+--------------------+
| Procedure | sql_mode                                                                                                                                  | Create Procedure                                                | character_set_client | collation_connection | Database Collation |
+-----------+-------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------+----------------------+----------------------+--------------------+
| t1        | ONLY_FULL_GROUP_BY,STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION |  CREATE DEFINER=`root`@`%` PROCEDURE `t1`()
begin
select 1;
end | utf8mb4              | utf8mb4_bin          | utf8mb4_bin        |
+-----------+-------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------+----------------------+----------------------+--------------------+
1 row in set (0.01 sec)
```