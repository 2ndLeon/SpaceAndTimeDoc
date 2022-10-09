---
description: Space and Time 依靠Biscuit来支持去中心化授权。本页介绍如何在平台中使用Biscuit。
---

# Biscuit授权

## 背景

Biscuits用于授权访问资源（例如表）并与公钥/私钥对相关联。公钥在用户创建资源时提供，用于在请求授权期间验证Biscuit签名。用户在不涉及平台的情况下创建和衰减自己​​的代币（使用相应的私钥）。一个Biscuit必须至少包含一个单一的能力，指定在哪个资源上允许什么操作。

更多有关biscuites的信息，请查看官方[Biscuit 文档](https://www.biscuitsec.org/docs/getting-started/introduction/) 。

## 标准

#### 能力:

```
sxt:capability($operation, $resource);
```

* `$operation` 符合操作要求
* `$resource` 符合资源要求

#### 操作:

`$operation` 必须是字符串并匹配以下之一：

SQL 操作:

* DDL: 用于操作数据库对象和模式（例如表、视图）
  * `ddl_create` : `CREATE` 指令
  * `ddl_drop` : `DROP` 指令
  * `ddl_alter` : `ALTER` 指令
* DQL: 用于执行查询
  * `dql_select` : `SELECT` 指令
* DML: 用于执行数据操作
  * `dml_insert` : `INSERT` 指令
  * `dml_update` : `UPDATE` 指令
  * `dml_delete` : `DELETE` 指令

#### 资源:

`$resource` 必须是字符串并匹配有效的现有资源（例如表）。

例如: `ETH.TRANSACTIONS`
