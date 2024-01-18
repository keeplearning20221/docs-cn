---
title: CALL
summary: 平凯数据库中 CALL 的使用概况
---

# CALL

`CALL` 语句用于在当前所选数据库中创建存储过程，与 MySQL 中 `CALL` 语句的行为类似。

## 语法图
```ebnf+diagram
CallStmt ::=
    CALL ProcedureName([parameter[,...]])

ProcedureName ::=
    Identifier ('.' Identifier)

characteristic ::=
    COMMENT 'string'
  | SQL SECURITY { DEFINER | INVOKER }

parameter ::=
     string
    |expr
    |`@`string
```

## 示例
调用存储过程：

{{< copyable "sql" >}}

```sql
use test
delimiter $$
create procedure t1()
begin
select 1;
select 2;
end $$
create procedure t2(inNum int)
begin
select inNum;
select 2;
end $$
delimiter ;
show procedure status;
call t1;
call t2(2);
```

```sql
ysql> use test
Database changed
mysql> delimiter $$
mysql> create procedure t1()
    -> begin
    -> select 1;
    -> select 2;
    -> end $$
ERROR 1304 (42000): PROCEDURE t1 already exists
mysql> create procedure t2(inNum int)
    -> begin
    -> select inNum;
    -> select 2;
    -> end $$
Query OK, 0 rows affected (0.01 sec)

mysql> delimiter ;
mysql> show procedure status;
+------+------+-----------+---------+---------------------+---------------------+---------------+---------+----------------------+----------------------+--------------------+
| Db   | Name | Type      | Definer | Modified            | Created             | Security_type | Comment | character_set_client | collation_connection | Database Collation |
+------+------+-----------+---------+---------------------+---------------------+---------------+---------+----------------------+----------------------+--------------------+
| test | t1   | PROCEDURE | root@%  | 2023-08-10 17:36:42 | 2023-08-10 17:36:42 | DEFINER       | test    | utf8mb4              | utf8mb4_bin          | utf8mb4_bin        |
| test | t2   | PROCEDURE | root@%  | 2023-08-10 17:47:13 | 2023-08-10 17:47:13 | DEFINER       |         | utf8mb4              | utf8mb4_bin          | utf8mb4_bin        |
+------+------+-----------+---------+---------------------+---------------------+---------------+---------+----------------------+----------------------+--------------------+
2 rows in set (0.00 sec)

mysql> call t1;
+---+
| 1 |
+---+
| 1 |
+---+
1 row in set (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

mysql> call t2(2);
+-------+
| inNum |
+-------+
|     2 |
+-------+
1 row in set (0.00 sec)

+---+
| 2 |
+---+
| 2 |
+---+
1 row in set (0.00 sec)

Query OK, 0 rows affected (0.00 sec)
```