---
description: 用于将您最喜欢的 IDE、SQL 编辑器或 BI 工具连接到 Space and Time
---

# JDBC驱动

对于 IDE、商业智能 (BI) 工具等本地安装应用程序，通过 JDBC 连接通常是标准做法，也是 Space and Time 支持的东西。这允许数据科学家和分析师在探索区块链数据时使用他们最喜欢的工具立即提高工作效率。

### 要求

* [Java v11](https://www.codejava.net/java-se/download-and-install-java-11-openjdk-and-oracle-jdk) 或更高版本
* 最新的 [SxT JDBC 驱动](https://media.istockphoto.com/vectors/under-construction-site-banner-sign-vector-black-and-yellow-diagonal-vector-id1192837450?k=20\&m=1192837450\&s=612x612\&w=0\&h=MpHAjpQ7v\_zZmH\_2FjcbmVMonTOkjs156B1egVrFViw=)&#x20;
* 属性配置如下

### 安全

JDBC 在用户名/密码 范式上运行，而 Space and Time 在更安全的用户 + 公钥/私钥 范式上运行。为了弥合这些差异，JDBC 配置需要一些非标准属性。

### 属性

* **user** - 应该为在新用户注册过程中提供的“userid”值。
* **password** - （可选）要么为空（空白），要么为用户的私钥。如果密码为空，则必须填写“privatekey”属性。
* **privateKey** - （可选）如果密码为空，则此属性必须包含用户的私钥。
* **publicKey** - 上面的privateKey对应的公钥。
* **biscuit\_\<name>** - （可选） 绑定到许可表的 [biscuits](broken-reference) 列表。

{% hint style="info" %}
无论使用“密码”还是“私钥”，用户的私钥**永远不会脱离** JDBC 驱动程序/用户的计算机。相反，私钥用于生成加密签名，允许Space and Time服务器验证密钥所有权，而无需传输密钥本身。
{% endhint %}

### **Biscuits:**

Space and Time 使用 [biscuits](broken-reference) 来管理去中心化网络上的用户和表之间的安全授权。在 cookie 和用户之间建立这种关系确实需要 JDBC 驱动程序的一个额外步骤。

有关创建饼干的详细信息，请参阅[饼干授权](../architecture/platform-security/biscuit-authorization.md)页面

**受控版本/Alpha** - 对于初始版本，Space and Time要求将每个私有表添加一个biscuit到 JDBC 属性中。例如，如果您想从 JDBC 连接访问 3 个私有表“TableA”、“TableB”和“TableC”，则需要 3 个新的 JDBC 属性：

* biscuit\_TableA: \<authorizing biscuit for TableA>
* biscuit\_TableB: \<authorizing biscuit for TableB>
* biscuit\_TableC: \<authorizing biscuit for TableC>

虽然是有一点小麻烦，但它保证了在去中心化网络中的正确授权。 &#x20;

**Testnet / Beta & 以外** - 正在努力使这个过程对用户来说更加动态，允许 JDBC 驱动程序为一组表提供一个biscuit，或者为用户自动生成biscuit列表。随着我们的进展，请定期回来查看。

{% hint style="info" %}
Space and Time 提供的区块链数据是“公开的”，这意味着它可以默认访问，并且不需要biscuit。如果你所做的只是在链上，你可以完全跳过biscuit干设置过程。
{% endhint %}
