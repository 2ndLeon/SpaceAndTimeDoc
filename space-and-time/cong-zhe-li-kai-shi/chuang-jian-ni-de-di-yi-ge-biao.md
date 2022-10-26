---
description: 自定义表
---

# 创建你的第一个表

#### 🪐  在继续前进之前请确保已连接到Space and Time。🪐

## 在Space and Time中创建表

将区块链索引数据与您自己的数据相结合是 Space and Time 的基石。显然，这需要您将自己的数据带入Space and Time。幸运的是，创建表非常简单。



## 示例 SQL&#x20;

让我们创建一个简单的表格来为您的用户收集钱包地址。这很简单：

```sql
Create table My_User_Wallets
( User_Wallet_Address  VARCHAR  Primary Key,
  User_Subscription    VARCHAR
)
```

搞定！让我们插入几行，让这更有趣……在这种情况下，前 100 个以 0x123 开头的钱包将成为“标准”用户：

```sql
insert into My_User_Wallets
  select distinct WALLET_ADDRESS, 'standard'
  from eth.WALLET
  where substr(WALLET_ADDRESS,1,5) = '0x123'
  limit 100
```

一条快捷的Select语句将验证数据：

```sql
select * from My_User_Wallets
```

![](<../../.gitbook/assets/image (17).png>)

### 加入链上和链下数据

这是 Space and Time 真正闪耀的地方 - 在单个请求中将您的特定用例数据加入到区块链数据中。

例如，一个 Web3 游戏可能有一个代表队列中玩家的钱包列表，并且需要提取 NFT 所有权以通过设备加载 (NFT) 适当地匹配玩家以进行下一轮游戏：

```sql
Select * 
from eth.WALLET -- on-chain 
join TOKEN_TYPE -- on-chain 
  on WALLET.TOKEN_TYPE_ID = TOKEN_TYPE.ID
join My_User_Wallets  -- off-chain, aka your app data
on WALLET.WALLET_ADDRESS = My_User_Wallets.User_Wallet_Address
where type in ('ERC721') -- only NFTs
```

<figure><img src="../../.gitbook/assets/image (15).png" alt=""><figcaption></figcaption></figure>
