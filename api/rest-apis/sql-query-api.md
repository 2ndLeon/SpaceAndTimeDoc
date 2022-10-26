# SQL 查询 API

SQL API 支持与 SxT 数据仓库的灵活 SQL 交互。使用此 API，您可以针对以下对象运行 SQL：

* 您自己的Space and Time 数据表，包含您提取的链下数据
* 包含我们从主要链索引的关系、实时区块链数据的表

在调用 SQL API 之前，请确保您已完成[安全工作流程](security-workflow/)。

<details>

<summary><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span> API 要求</summary>

#### 安全要求：

* 在授权请求头（作为`bearer` token）提供的 `accessToken` 必须和在[安全工作流程](security-workflow/)的[验证码API](security-workflow/user-authentication.md#yan-zheng-ma-api)中接收到的 `accessToken` 一致。
* Biscuit token必须在`biscuit` 请求头中提供（有关biscuits的详细信息，请参阅[此处](../../architecture/platform-security/biscuit-authorization.md)）

#### 其它要求：

* `resourceId` 必须是完整的资源路径。 例如：

对于以下的查询：&#x20;

```
SELECT * FROM ETH.TRANSACTIONS LIMIT 1000;
```

`resourceId` 是 `ETH.TRANSACTIONS`

</details>

{% tabs %}
{% tab title="DQL" %}
### :comet: 使用 DQL 端点查询 SxT 中的数据

使用DQL端点执行 `SELECT` 命令。



#### 命令示例：

```sql
SELECT column1, column2, ...
FROM table_name;
```
{% endtab %}

{% tab title="DML" %}
### :comet: 使用 DML Endpoint 修改 SxT 中的数据

用DML端点执行 `INSERT`, `UPDATE` 和 `DELETE` 命令。



#### 命令示例：

```sql
INSERT INTO table_name (column1, column2, column3, ...)
VALUES (value1, value2, value3, ...);
```

```sql
UPDATE table_name
SET column1 = value1, column2 = value2, ...
WHERE condition;
```

```sql
DELETE FROM table_name WHERE condition;
```
{% endtab %}

{% tab title="DDL" %}
### :comet: 使用 DDL 端点 在 SxT 中配置资源

使用DML端点执行 `CREATE`, `ALTER` and `DROP` 命令。



#### 命令示例：

```sql
CREATE TABLE table_name (
    column1 datatype,
    column2 datatype,
    column3 datatype,
   ....
);
```

```sql
ALTER TABLE table_name
ADD column_name datatype;
```

```sql
DROP TABLE table_name;
```
{% endtab %}
{% endtabs %}

## DQL 端点

通过将 SQL 发送到此端点来执行所有 `SELECT` 命令（查询）。

{% swagger method="post" path="" baseUrl="/sql/dql" summary="执行查询" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="header" name="bearer" type="String" required="true" %}
从身份验证工作流接收的访问token
{% endswagger-parameter %}

{% swagger-parameter in="header" name="biscuit" type="String" required="true" %}
验证token
{% endswagger-parameter %}

{% swagger-parameter in="body" name="resourceId" type="String" required="true" %}
资源标识符
{% endswagger-parameter %}

{% swagger-parameter in="body" name="sqlText" type="String" required="true" %}
要执行的原始 SQL
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="查询结果" %}
```javascript
{
    // Response
}
```
{% endswagger-response %}

{% swagger-response status="401: Unauthorized" description="无效的安全参数" %}
```javascript
{
    // Response
}
```
{% endswagger-response %}

{% swagger-response status="404: Not Found" description="无效的resourceId" %}
```javascript
{
    // Response
}
```
{% endswagger-response %}
{% endswagger %}

#### 试一试：

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl -X POST \
    "https://api.spaceandtime.io/sql/dql" \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
	-H "biscuit: <BISCUIT>" \
	-H "access_token: <ACCESS_TOKEN>" \
    -d '{"resourceId": "PUBLIC.CUSTOMER","sqlText": "select * from PUBLIC.CUSTOMER"}'
```
{% endcode %}
{% endtab %}

{% tab title="Javascript" %}
```javascript
const url = new URL(
    "'https://api.spaceandtime.io/sql/dql"
);

let headers = {
  "Content-Type": "application/json",
  "Accept": "application/json",
  "biscuit": "<BISCUIT>",
  "access_token":"<ACCESS_TOKEN>"

};

let body = {
    "resourceId": "PUBLIC.CUSTOMER",
    "sqlText": "select * from PUBLIC.CUSTOMER"
}

fetch(url, {
    method: "POST",
    headers: headers,
    body: body
})
    .then(response => response.json())
    .then(json => console.log(json));
```
{% endtab %}

{% tab title="Python" %}
```python
import requests
import json

url = 'https://api.spaceandtime.io/sql/dql'
payload = {
    "resourceId": "PUBLIC.CUSTOMER",
    "sqlText": "select * from PUBLIC.CUSTOMER"
}
headers = {
  'Content-Type': 'application/json',
  'Accept': 'application/json',
  'biscuit':'<BISCUIT>',
  'access_token':'<ACCESS_TOKEN>'
}
response = requests.request('POST', url, headers=headers, json=payload)
response.json()
```
{% endtab %}
{% endtabs %}

## DML 端点

使用此端点执行所有 `INSERT`、`UPDATE`、`DELETE` 命令。

{% swagger method="post" path="" baseUrl="/sql/dml" summary="修改数据" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="header" name="bearer" type="String" required="true" %}
从身份验证工作流接收的访问token
{% endswagger-parameter %}

{% swagger-parameter in="header" name="biscuit" type="String" required="true" %}
验证token
{% endswagger-parameter %}

{% swagger-parameter in="body" name="resourceId" type="String" required="true" %}
资源标识符
{% endswagger-parameter %}

{% swagger-parameter in="body" name="sqlText" type="String" required="true" %}
要执行的原始 SQL
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="DML 执行结果" %}
```javascript
{
    // Response
}
```
{% endswagger-response %}

{% swagger-response status="401: Unauthorized" description="无效的安全参数" %}
```javascript
{
    // Response
}
```
{% endswagger-response %}

{% swagger-response status="404: Not Found" description="无效的resourceId" %}
```javascript
{
    // Response
}
```
{% endswagger-response %}
{% endswagger %}

#### 试一试：

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl -X POST \
    "https://api.spaceandtime.io/sql/dml" \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
	-H "biscuit: <BISCUIT>" \
	-H "access_token: <ACCESS_TOKEN>" \
    -d '{"resourceId": "PUBLIC.CUSTOMER","sqlText": "INSERT INTO PUBLIC.CUSTOMER (ID,FIRSTNAME,LASTNAME,EMAIL) VALUES(0, 'Robert', 'Jr', 'robert.jr@gem.com')"}'
```
{% endcode %}
{% endtab %}

{% tab title="Javascript" %}
```javascript
const url = new URL(
    "'https://api.spaceandtime.io/sql/dml"
);

let headers = {
  "Content-Type": "application/json",
  "Accept": "application/json",
  "biscuit": "<BISCUIT>",
  "access_token":"<ACCESS_TOKEN>"

};

let body = {
    "resourceId": "PUBLIC.CUSTOMER",
    "sqlText": "INSERT INTO PUBLIC.CUSTOMER (ID,FIRSTNAME,LASTNAME,EMAIL) VALUES(0, 'Robert', 'Jr', 'robert.jr@gem.com')"
}

fetch(url, {
    method: "POST",
    headers: headers,
    body: body
})
    .then(response => response.json())
    .then(json => console.log(json));	
```
{% endtab %}

{% tab title="Python" %}
```python
import requests
import json

url = 'https://api.spaceandtime.io/sql/dml'
payload = {
  "resourceId": "PUBLIC.CUSTOMER",
  "sqlText": "INSERT INTO PUBLIC.CUSTOMER (ID,FIRSTNAME,LASTNAME,EMAIL) VALUES(0, 'Robert', 'Jr', 'robert.jr@gem.com')"
}
headers = {
  'Content-Type': 'application/json',
  'Accept': 'application/json',
  'biscuit':'<BISCUIT>',
  'access_token':'<ACCESS_TOKEN>'
}
response = requests.request('POST', url, headers=headers, json=payload)
response.json()python
```
{% endtab %}
{% endtabs %}

## DDL 端点

使用此端点执行所有 `CREATE`、`ALTER`、`DROP` 命令。

{% swagger method="post" path="" baseUrl="/sql/ddl" summary="配置资源" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="header" name="bearer" type="String" required="true" %}
从身份验证工作流接收的访问token
{% endswagger-parameter %}

{% swagger-parameter in="body" required="true" name="resourceId" type="String" %}
资源标识符
{% endswagger-parameter %}

{% swagger-parameter in="body" required="true" name="sqlText" type="String" %}
要执行的原始 SQL
{% endswagger-parameter %}

{% swagger-parameter in="header" name="biscuit" type="String" required="true" %}
验证token
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="DDL 执行结果" %}
```javascript
{
    // Response
}
```
{% endswagger-response %}

{% swagger-response status="401: Unauthorized" description="无效的安全参数" %}
```javascript
{
    // Response
}
```
{% endswagger-response %}

{% swagger-response status="404: Not Found" description="无效的resourceId" %}
```javascript
{
    // Response
}
```
{% endswagger-response %}
{% endswagger %}

#### 试一试：

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl -X POST \
    "https://api.spaceandtime.io/sql/ddl" \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
	-H "biscuit: <BISCUIT>" \
	-H "access_token: <ACCESS_TOKEN>" \
    -d '{"resourceId": "PUBLIC.CUSTOMER","sqlText": "CREATE TABLE PUBLIC.CUSTOMER (ID INTEGER,FIRSTNAME VARCHAR,LASTNAME VARCHAR,EMAIL VARCHAR,PRIMARY KEY (ID))"}'
```
{% endcode %}
{% endtab %}

{% tab title="Javascript" %}
{% code overflow="wrap" %}
```javascript
const url = new URL(
    "'https://api.spaceandtime.io/sql/ddl"
);

let headers = {
  "Content-Type": "application/json",
  "Accept": "application/json",
  "biscuit": "<BISCUIT>",
  "access_token":"<ACCESS_TOKEN>"

};

let body = {
    "resourceId": "PUBLIC.CUSTOMER",
    "sqlText": "CREATE TABLE PUBLIC.CUSTOMER (ID INTEGER,FIRSTNAME VARCHAR,LASTNAME VARCHAR,EMAIL VARCHAR,PRIMARY KEY (ID))"
}

fetch(url, {
    method: "POST",
    headers: headers,
    body: body
})
    .then(response => response.json())
    .then(json => console.log(json));
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code overflow="wrap" %}
```python
import requests
import json

url = 'https://api.spaceandtime.io/sql/ddl'
payload = {
  "resourceId": "public.CUSTOMER",
  "sqlText": "CREATE TABLE PUBLIC.CUSTOMER (ID INTEGER,FIRSTNAME VARCHAR,LASTNAME VARCHAR,EMAIL VARCHAR,PRIMARY KEY (ID))"
}
headers = {
  'Content-Type': 'application/json',
  'Accept': 'application/json',
  'biscuit':'<BISCUIT>',
  'access_token':'<ACCESS_TOKEN>'
}
response = requests.request('POST', url, headers=headers, json=payload)
response.json()
```
{% endcode %}
{% endtab %}
{% endtabs %}
