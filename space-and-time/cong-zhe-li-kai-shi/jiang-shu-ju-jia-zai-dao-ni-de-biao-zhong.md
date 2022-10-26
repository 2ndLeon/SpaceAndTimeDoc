---
description: 将数据放入Space and Time
---

# 将数据加载到你的表中

#### 🪐  在继续前进之前请确保已连接到Space and Time。🪐

## 将数据加载到Space and Time中

有几种方法可以将数据加载到Space and Time中，具体取决于体积、速度和频率。

下面的示例将加载我们在上一节创建的示例表，创建您的第一个表，即：My\_User\_Wallets。如果您想一起体验，请务必先查看该部分。

<details>

<summary>DML - Insert 语句</summary>

如果您的数据很小，那么老式的 INSERT 语句可能是一个简单的解决方案。例如，您的 App 或 dApp 可以发出 API 请求，以根据用户活动简单地添加（或更新或删除）少量数据。

让我们在 My\_User\_Wallets 表中添加一个“Premium”用户：

```sql
insert into My_User_Wallets 
(User_Wallet_Address, User_Subscription)
values ('0x456008396BFdd64159998cE362b8D650FFd6F28b', 'premium')
```

我们来确认一下我们现在有一个Premium用户：



看起来不错！当然，这只是众多可能的 DML 语句之一（delete, update, merge等），但它展示了Space and Time的事务速度……提交时间为 148 毫秒，足以驱动实时应用程序。

</details>

<details>

<summary>Bulk Load - Space and Time 批量加载器</summary>

如果您要加载超过几千行，则需要将它们捆绑到批量加载中以达到最佳性能。这对于初始数据加载特别方便，如果您拥有只需要加载一次的大型数据文件。

要使用批量加载器，您需要 2 个文件：

### 数据文件

要上传的数据文件，采用 JSON 或 CSV 格式（即将推出更多格式）。为了继续我们之前的示例，让我们创建一个名为 Premium\_Users.json 的示例数据文件：

```json
[
["0x45619a075cf19df2506285c376cf5d2ae7d13391","premium"]
["0x4560b99c904aad03027b5178cca81584744ac01f","premium"]
["0x4565aaa0fbdf08bf710f7bc98132ee5d9c01dd0d","premium"]
["0x45692A7bBf52D7913d5a17397A91a8FB253c65A3","premium"]
["0x45656f9ec4341a1c199993e8744a5f2002989705","premium"]
]
```

### 控制文件

需要的第二个文件是控制文件，它为批量加载程序提供加载参数。为我们上面的例子创建一个新的控制文件：

```json
{
  "job_name":      "sample user load",
  "source_file":   "./Premium_Users.json",
  "dest_table":    "My_User_Wallets",
  "chunk_records": 1000,
  "error_count"    10 
}
```

* job\_name - 用于标识加载任务的任意标题
* source\_file - 数据文件的位置
* dest\_table - 要加载的预先存在的表的名称
* chunk\_records - 每个加载块添加的记录数（默认为 1000） - 目前可选性能约为每块 1MB
* error\_count - 加载停止前允许的错误数\
  &#x20;\- 值为 10 将允许 10 个错误，第 11 个错误将停止加载\
  &#x20;\- 值为 0 将在任何错误发生时停止加载\
  &#x20;\- 值为负数 (-1) 将禁用错误跟踪，允许任意数量的错误

加载以记录块的形式完成，允许引擎轻松处理任何大小的文件 - 从 MB 到 PB。每次加载都会生成一个带时间戳的日志文件，其中包含所有活动的记录以及任何包含错误的记录。

您可能会注意到缺少的一些项目：

* 没有认证 - 在受控发布期间，Space and Time 将手动确认（并安排，如果需要）每个客户的所有批量上传。未来的版本将需要额外的安全步骤来防止未经授权的加载。
* 没有列的定义 - 列应在计数、位置和数据类型上与目标表匹配。偏差只会引发错误。未来的版本将允许在控制文件中对列进行可选定义，从而在批量加载程序中提供新的智能选项，比如：\
  &#x20; \- 文件预验证，在发送到数据库之前\
  &#x20; \- 按名称加载，而不是按位置加载\
  &#x20; \- 如果缺少，可以选择创建表

### 文件已创建，接下来呢？

为确保在受控发射期间顺利运行，请联系您的Space and Time联系人 - 他们可以为您提供上传位置并讨论时间安排。

</details>

<details>

<summary>Streaming - Kafka 客户</summary>

Space and Time 数据摄取的核心是 Kafka，它在幕后管理着大部分数据加载。如果您已经有数据生产者，那么将数据发送给Space and Time客户自然适合自动摄取。

集成细节仍在最终确定中，请之后再回来查看！

</details>

<details>

<summary>以上所有：3TL</summary>

3TL 是 Space and Time 的合作伙伴，它运行您现有的 python 脚本以从任何 Web2 或 Web3 源（链上或链下，中心化或去中心化）加载数据，对数据应用任何转换或计算，达成共识输出，并将该输出发送到任何 Web3 目标。

如果您对 3TL 感兴趣，请联系 Space and Time 来获取介绍。

</details>
