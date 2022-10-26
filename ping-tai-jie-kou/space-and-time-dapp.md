---
description: 想要快速上手？ dApp 让您立即开始探索Space and Time！
---

# Space and Time dapp

dApp 为那些希望立即开始使用区块链数据的人提供了一个快速而现代的Space and Time进入点。

当您准备好时，可以在[这里找到 dApp](https://uat-dappsxt.azureedge.net/)！ 🚀

请注意，在[受控发布](../space-and-time/shou-kong-qi-dong-zhu-yi-shi-xiang.md)期间，上述 URL 将需要特殊邀请访问权限。

<figure><img src="../.gitbook/assets/image (14).png" alt=""><figcaption></figcaption></figure>

## dApp 验证

登录可以在 dApp 界面的右上角找到。 dApp 有两种身份验证方法：

### 使用钱包登录&#x20;

通过 MetaMask 连接钱包将允许您完全访问查询所有可用区块链，包括所有历史记录。

如果您已将钱包绑定到已知客户帐户，您也可以查询私有表，允许您在一个请求中加入链上和链下数据。任何计算费用都将转入您的公司帐户。

如果您以个人身份连接了您的钱包，计算成本将被确认并直接扣除（通常每次查询大约 0.01 美元到 0.02 美元），以 Eth 或 USDC 表示。

{% hint style="info" %}
在 Alpha 测试期间，请联系您的Space and Time 联系人，让您的钱包与您的公司帐户相关联。
{% endhint %}

### 匿名（无需登录）

匿名使用 dApp 可以让您轻松免费地开始使用，即使您没有 Space and Time 的客户帐户。也就是说，可用数据存在一些限制：

* 仅限以太坊链 30 天以内的数据（第 31 天到第 1 天，每天更新）
* 每个查询仅限返回 10 行

## dApp 功能

dApp 旨在使用现代范式进行模型查看和查询编辑，使导航区块链数据变得快速和容易。

### 模型导航

选择“Data Models”可以查看不同区块链的实体关系图（ERD）：

<figure><img src="../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>

要探索实际模型，请触摸/单击特定链以查看完整的 ERD：

<figure><img src="../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

如果您将鼠标悬停在 ERD 中的任何表上，模型将突出显示所有其他可以连接的表，以及使用标准 ERD 表示法突出显示可连接列和连接类型（1:1、1:M 等）。

### 查询编辑器

单击“Query Editor”会打开基于 Web 的 Space and Time dApp 查询编辑器。 ERD 将保留在右侧，而左侧提供 SQL 编辑器空间，具有现代编辑功能，包括表/列自动完成、列编辑和语法着色。

<figure><img src="../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
要运行查询，请单击“Run Query”按钮，或使用快捷键：Ctrl-Enter。
{% endhint %}

如果您有权访问多个集群，您将在页面顶部看到一个集群选择下拉菜单。系统选择对查询是“sticky”，这意味着它与查询元数据一起保存。

<figure><img src="../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

查询完成后，结果将以表格格式（默认）或使用动态创建的可视化显示在 SQL 编辑器下方。 dApp 将根据返回的数据尝试生成最佳视觉效果，但是如果您想调整视觉效果，只需单击视觉缩略图上方的“Customize Chart”按钮：

<figure><img src="../.gitbook/assets/image (19).png" alt=""><figcaption></figcaption></figure>

### 保存查询

当您有想要保存到 dApp 的查询时，您可以单击“Save”按钮（或按 ctrl-S），为其命名、添加任何适当的标签、添加描述和“Save as New Query” 。如果您想稍后更改相同的查询，您可以选择“Update”以覆盖相同的查询名称。

<figure><img src="../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
查询名称不需要是唯一的，因此建议您为所有已保存的查询提供足够的信息，以便您了解它们的用途。
{% endhint %}

### 查找已保存的查询

要查找以前保存的查询，请单击“Queries”菜单项，该菜单项将拉出您有权访问的所有查询的列表，最近保存的查询位于顶部。

最简单的方法是搜索关键字 - 搜索将返回名称或标签中的任何匹配项。

或者，您可以“Explore”查询，它会在顶部提供最常见标签的列表，并在选择标签时过滤查询。

最后，您可以查看“Query History”，它提供了按运行顺序排列的查询列表，包括 CPU 消耗、偏差、运行时和其他统计信息的摘要。

<figure><img src="../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

**如果您对 dApp 有任何反馈，请与您的 Space and Time 联系人分享 - 我们希望这对 Web3 的所有开发者都非常有用！**
