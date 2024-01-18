---
title: CREATE PROCEDURE
summary: 平凯数据库中 CREATE PROCEDURE 的使用概况
---

# CREATE PROCEDURE

`CREATE PROCEDURE` 语句用于在当前所选数据库中创建存储过程，与 MySQL 中 `CREATE PROCEDURE` 语句的行为类似。

## 语法图
```ebnf+diagram
CreateProcedureStmt ::=
    'CREATE' [DEFINER = user] 'PROCEDURE' IfNotExists ProcedureName ([proc_parameter[,...]]) 
    [characteristic ...] routineBody

proc_parameter ::=
   [ IN | OUT | INOUT ] param_name type

type ::=
    Any valid MySQL data type (exception json)

ProcedureName ::=
    Identifier ('.' Identifier)

IfNotExists ::=
    ('IF' 'NOT' 'EXISTS')

characteristic ::=
    COMMENT 'string'
  | SQL SECURITY { DEFINER | INVOKER }

routineBody ::=
    statement

BlockStmt ::=
[begin_label:] BEGIN
    [statement_list]
    END [end_label]

CaseStmt ::=
     CASE
     WHEN search_condition THEN statement_list
        [[WHEN search_condition THEN statement_list] ...]
        [ELSE statement_list]
     END CASE 
    |CASE case_value
        WHEN when_value THEN statement_list
        [[WHEN when_value THEN statement_list] ...]
        [ELSE statement_list]
     END CASE

IfStmt ::=
    IF search_condition THEN statement_list
        [[ELSEIF search_condition THEN statement_list] ...]
        [ELSE statement_list]
    END IF

LoopStmt ::=
    [begin_label:] LOOP
        statement_list
    END LOOP [end_label]

RepeatStmt ::=
    [begin_label:] REPEAT
        statement_list
        UNTIL search_condition
    END REPEAT [end_label]

WhileStmt ::=
    [begin_label:] WHILE search_condition DO
        statement_list
    END WHILE [end_label]


CloseCur ::=
    CLOSE cursor_name

DeclareVar ::=
     DECLARE var_name [, var_name] ... type [DEFAULT value]
    |DECLARE handler_action HANDLER
     FOR condition_value [, condition_value] ...
     statement
    |DECLARE cursor_name CURSOR FOR select_statement

FetchInto ::=
    FETCH [[NEXT] FROM] cursor_name INTO var_name [[, var_name] ...]

OpenCur ::=
    OPEN cursor_name

ProcedureSQL ::=
     SelectStmt
    |UpdateStmt
    |InsertStmt
    |DeleteStmt
    |ShowStmt
    |ExplainStmt

statement_list ::= 
    statement_list [; statement]

statement ::=
     DeclareVar
    |FetchInto
    |OpenCur
    |CloseCur
    |WhileStmt
    |RepeatStmt
    |LoopStmt
    |IfStmt
    |CaseStmt
    |BlockStmt
    |ProcedureSQL

handler_action: {
    CONTINUE
  | EXIT
  | UNDO
}

condition_value: {
    mysql_error_code
  | SQLSTATE [VALUE] sqlstate_value
  | SQLWARNING
  | NOT FOUND
  | SQLEXCEPTION
}
```
TiDB 不支持以下 `characteristic`:`LANGUAGE SQL`,`[NOT] DETERMINISTIC`,`{ CONTAINS SQL | NO SQL | READS SQL DATA | MODIFIES SQL DATA }`。

| 参数                              |含义                                           |举例                               |
|----------------------------------|-----------------------------------------------|-----------------------------------|
|`tidb_enable_procedure`           |开启或者关闭 tidb 存储过程                        |`tidb_enable_procedure` = ON|
|`tidb_enable_sp_param_substitute` |是否开启存储过程内部变量查询下推                    |`tidb_enable_sp_param_substitute`=ON|
|`max_sp_recursion_depth`          |存储过程最多支持多少次递归                         |`max_sp_recursion_depth` = 125|
|`stored_program_cache`            |最多缓存多少个存储过程执行计划                      |`stored_program_cache` = 256|

> **注意：**
>
> 在 TiDB 存储过程中不支持调用 DDL、CALL 语句，在存储过程中使用 prepare 和 execute 处于测试状态，不建议使用。

## 示例
创建一个存储过程：

{{< copyable "sql" >}}

```sql
use test
delimiter $$
create procedure t1()
begin
select 1;
end $$
delimiter ;
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
```