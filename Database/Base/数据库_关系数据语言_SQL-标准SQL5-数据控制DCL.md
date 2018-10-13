- [数据库：关系数据语言 - 标准 SQL - 数据控制(DCL)](#数据库关系数据语言---标准-sql---数据控制dcl)
  - [1. 权限控制](#1-权限控制)
    - [1.1. GRANT](#11-grant)
    - [1.2. REVOKE](#12-revoke)
  - [2. 事务控制](#2-事务控制)
  - [3. Refer Links](#3-refer-links)

# 数据库：关系数据语言 - 标准 SQL - 数据控制(DCL)

## 1. 权限控制

SQL 中使用 GRANT 和 REVOKE 语句向用户授予或收回对数据的操作权限。

### 1.1. GRANT

GRANT 语句用于向用户授权：
```sql
GRANT 权限 ON 数据库 . 数据表 TO 用户名 @登录主机 INDENTIFIED BY “密码”;
```

例：
```sql
GRANT SELECT, INSERT, UPDATE, DELETE ON *.* TO jq@localhost INDENTIFIED BY “jqjqjqjq”;
```

### 1.2. REVOKE

REVOKE 语句用于收回授予用户的权限：
```sql
REVOKE 权限 ON 数据库 . 数据表 FROM 用户名 [CASCADE|RESTRICTT];
```

## 2. 事务控制

- COMMIT

- ROLLBACK

- SAVEPOINT

## 3. Refer Links