---
description: 无需加载数据来编写查询 - 立即探索区块链数据！
---

# 执行你的第一条查询

#### 🪐  在继续前进之前请确保已连接到Space and Time。🪐

## Space and Time 的查询

现在您已经连接到平台，是时候执行您的第一条查询了！作为我们客户的增值服务，Space and Time 提供对标准化区块链索引数据的默认访问 - 这意味着您可以直接进入创作查询！​​​🚀

## 示例 SQL&#x20;

下面是一些关于区块链表的示例查询，可帮助您入门。您可以使用 [Space and Time dApp](broken-reference) 或您最喜欢的兼容 JDBC 的 SQL 编辑器运行这些查询。

#### 哪个以太坊交易所的交易数量最多？

```sql
select EXCHANGE_NAME, count(*) 
from ETH.DEX_TRADE
Group by EXCHANGE_NAME
order by 2 desc
```

![](<../../.gitbook/assets/image (5).png>)

你执行了你的第一条查询！ :tada:   \
\
让我们来试试另一个：

#### 给定钱包的内容是什么？

```sql
select Type as TokenType, count(*) 
from eth.WALLET
join eth.TOKEN_TYPE
  on WALLET.TOKEN_TYPE_ID = TOKEN_TYPE.ID
where WALLET_ADDRESS = '0x024BCbCAad82E67F721799E259ca60bc7d363419'
group by TokenType
order by 2 desc 
```

![](<../../.gitbook/assets/image (20).png>)

尝试用你的钱包地址替换上面的 WALLET\_ADDRESS，看看会发生什么！

## 探索 Space and Time

在运行其区块链索引时，Space and Time将规范化为所有区块链数据的关系结构（又叫做表）。这使得对数据元素（又叫做列）的理解变得简单而直观。

您可以使用 dApp 来探索可用的不同区块链和表格，方法是 跳转到数据模型选项卡并选择要探索的链：

<figure><img src="../../.gitbook/assets/image (12).png" alt=""><figcaption></figcaption></figure>

这将显示具有标准化区块链数据的所有表的模型，包括键和性能连接路径（又名实体关系图或 ERD）：

<figure><img src="../../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

您还可以在右侧的“Query Editor”中找到此 ERD 模型。dApp 查询编辑器将自动完成表/列名称。当您将表添加到查询中时，右侧的模型将只关注那个表和相关/可连接表：

<figure><img src="../../.gitbook/assets/image (11).png" alt=""><figcaption></figcaption></figure>

当然，Space and Time 是一个数据仓库——这意味着其他 ERD 程序和 SQL 开发环境也应该可以工作。
