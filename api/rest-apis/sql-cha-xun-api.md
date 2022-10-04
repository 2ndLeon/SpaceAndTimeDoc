# SQL 查询 API

SQL API 支持与 SxT 数据仓库的灵活 SQL 交互。使用此 API，您可以针对以下对象运行 SQL：

* 您自己的Space and Time 数据表，包含您提取的链下数据
* 包含我们从主要链索引的关系、实时区块链数据的表

在调用 SQL API 之前，请确保您已完成[安全工作流程](cong-zhe-li-kai-shi-an-quan-gong-zuo-liu-cheng/)。

<details>

<summary><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span> API 要求</summary>

#### 安全要求：

* 在授权请求头（作为`bearer` token）提供的 `accessToken` 必须和在[安全工作流程](cong-zhe-li-kai-shi-an-quan-gong-zuo-liu-cheng/)的[验证码API](cong-zhe-li-kai-shi-an-quan-gong-zuo-liu-cheng/yong-hu-ren-zheng.md#yan-zheng-ma-api)中接收到的 `accessToken` 一致。
* Biscuit token必须在`biscuit` 请求头中提供（有关biscuits的详细信息，请参阅[此处](../../zheng-ti-jia-gou/ping-tai-an-quan/biscuit-shou-quan.md)）

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
```
```
{% endtab %}

{% tab title="Javascript" %}
```
```
{% endtab %}

{% tab title="Python" %}
```
curl -X POST \
    "https://api.spaceandtime.io/sql/dql" \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
	-H "biscuit: <BISCUIT>" \
	-H "access_token: <ACCESS_TOKEN>" \
    -d '{"resourceId": "PUBLIC.CUSTOMER","sqlText": "select * from PUBLIC.CUSTOMER"}'curl -X POST \
    "https://api.spaceandtime.io/sql/dql" \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
	-H "biscuit: <BISCUIT>" \
	-H "access_token: <ACCESS_TOKEN>" \
    -d '{"resourceId": "PUBLIC.CUSTOMER","sqlText": "select * from PUBLIC.CUSTOMER"}'curl -X POST \
    "https://api.spaceandtime.io/sql/dql" \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
	-H "biscuit: <BISCUIT>" \
	-H "access_token: <ACCESS_TOKEN>" \
    -d '{"resourceId": "PUBLIC.CUSTOMER","sqlText": "select * from PUBLIC.CUSTOMER"}'vfde
```
{% endtab %}
{% endtabs %}
