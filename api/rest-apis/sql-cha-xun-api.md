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

{% tab title="Second Tab" %}

{% endtab %}

{% tab title="DDL" %}

{% endtab %}
{% endtabs %}
