# SQL证明

Space and Time 工程团队正在积极构建 SQL 证明的功能：允许数据仓库生成 SQL 查询执行的 SNARK 加密证明的新颖密码学，证明查询计算已准确完成，并且查询和数据是可验证的篡改。

## 关于防篡改查询

防篡改查询需要两方：验证者和证明者。对于时空架构，网关节点扮演验证者的角色，数据库集群扮演证明者的角色。

该过程包括两种类型的交互：数据摄取和查询请求。

### 数据摄取 <a href="#data-ingestion" id="data-ingestion"></a>

当服务或客户端发送要添加到数据库的数据时，该数据将被路由到验证者，以便验证者可以创建对该数据的承诺。该承诺是数据的一个小“摘要”，但为协议的其余部分保存了足够的信息，以确保数据不被篡改。 \[注意：数据可以立即路由，但验证者仅在创建此承诺后“锁定”数据。立即路由时需要一些额外的预防措施以保持承诺与数据库正确同步]创建此承诺，验证者可以将数据路由到数据库进行存储。验证者存储此承诺以供以后使用。

<figure><img src="https://1359882050-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FzHdDPR6NVKwyccJmuzAw%2Fuploads%2FfyPZv2LRdDQr1C1xTTJm%2Ffig1.png?alt=media&#x26;token=b4feee30-57c0-4246-b5a5-b16aab67eca0" alt=""><figcaption><p>图 1：表创建协议</p></figcaption></figure>

一个重要的设计约束是承诺必须是可更新的。换句话说，假设验证者已经持有对特定表的承诺，但是要摄取新数据并将其附加到表中。为了有效地做到这一点，验证者必须能够将旧的承诺与传入的数据结合起来，为整个更新的表创建一个新的承诺。这里的关键约束是验证者必须能够在不访问旧现有数据的情况下执行此操作。

注意：在这个过程的任何阶段，证明者除了接受和存储传入数据之外不需要做任何事情。

<figure><img src="https://1359882050-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FzHdDPR6NVKwyccJmuzAw%2Fuploads%2F7msGfOHmPYSTYba5Wv28%2Ffig2.png?alt=media&#x26;token=74738f88-e277-4b9c-9f8e-6c01f0e6976e" alt=""><figcaption><p>图 2：表追加协议</p></figcaption></figure>

### 查询请求 <a href="#query-requests" id="query-requests"></a>

当服务或客户端发送查询请求时，该请求通过网关路由到证明者，即数据库。 \[因为网关扮演验证者的角色，验证者正在路由请求。但是，这并不是绝对必要的，并且可以将请求直接路由到证明者，甚至无需打开数据包。] 此时，证明者解析查询，计算正确的结果，并生成 SQL 证明。然后它将查询结果和证明发送给验证者。

一旦验证者获得了这个证明，它就可以使用承诺来对照结果检查证明，并验证证明者对查询请求产生了正确的结果。然后可以将此查询结果与防篡改成功标志一起路由回服务或客户端。但是，如果证明不通过，验证者不会路由结果，而是发送失败消息。

<figure><img src="https://1359882050-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FzHdDPR6NVKwyccJmuzAw%2Fuploads%2F6GpEPCNyxpWHz2iIOZHd%2Ffig3.png?alt=media&#x26;token=3572c15c-44c9-4d52-aeae-3610aead76d7" alt=""><figcaption><p>图 3：查询请求协议</p></figcaption></figure>

## 原生语句 <a href="#primitives" id="primitives"></a>

### 非交互随机性 <a href="#non-interactive-randomness" id="non-interactive-randomness"></a>

从概念上讲，当证明者和验证者能够拥有多轮来回协议时，更容易设计证明结果的协议。例如，每个协议中的一个基本组成部分是使用随机挑战。通常，验证者想要检查来自证明者的声明是否属实。为此，验证者向证明者发送一个随机挑战。由于证明者在发送挑战之前将声明发送给验证者，因此证明者无法提前知道，也无法针对特定挑战恶意修改声明。如果声明和质询的结构适当，每个质询具有正确响应，都会让验证者知道证明者不太可能是恶意的。更复杂的声明可能需要更多的挑战/响应交互。

<figure><img src="https://1359882050-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FzHdDPR6NVKwyccJmuzAw%2Fuploads%2FtzCiIF2roGLAu7BSGOCz%2Ffig9.png?alt=media&#x26;token=981b28e6-f2ff-4e42-942d-ad35e6f0729d" alt=""><figcaption><p>图 9：典型的交互协议</p></figcaption></figure>

两方之间的这种多轮交互会导致极端的延迟问题，尤其是对于更复杂的查询。幸运的是，Fiat-Shamir 启发式解决了这个问题，该启发式本质上允许证明者模拟整个交互。在这种情况下，证明者生成随机挑战，但通过启发式的设计，仍然无法在“发送”声明之前学习挑战。这使我们能够利用随机挑战的力量，同时仍然允许证明者只发送一条消息来响应验证者。

<figure><img src="https://1359882050-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FzHdDPR6NVKwyccJmuzAw%2Fuploads%2FgKxg0DkPy12yibpYVQB3%2Ffig10.png?alt=media&#x26;token=3bb213c6-115b-459c-a9b0-951cc36e6787" alt=""><figcaption><p>图 10：Fiat-Shamir 非交互协议</p></figcaption></figure>

### 承诺（commitment） <a href="#commitments" id="commitments"></a>

最大的计算成本围绕着承诺及其用途。因此，我们使用哪种承诺方案的决定很重要。幸运的是，我们选择的承诺方案可以被视为一个“黑匣子”，所以这个选择可以随着产品的发展而修改。

有三个主要的^\[这也许是一个概括] 经常使用的承诺方案：Pedersen 承诺、KZG 承诺和 FRI 承诺。我们最初使用的承诺方案是广义的 Pedersen 承诺。有几个原因。

* _简单_\
  Pedersen 承诺是一个简单的结构，它有助于更​​快的开发和更短的上市时间。它还具有很好的数学特性，可以简化协议的其他组件。简单介绍一下，假设 a0, a1, a2,… 是一列数据。那么，Pedersen 承诺就是 a0⋅G0⊕a1⋅G1⊕a2⋅G2⊕⋯ 其中 Gi 是“随机”的公共参数。值得注意的是，这些 Gi 是透明的参数，不需要像其他一些承诺方案所要求的可信设置阶段。
* _容易更新_\
  正如概述中提到的，一个重要的设计约束是这些承诺应该是可更新的。附加到承诺所需要做的就是为每条新数据在上述总和中添加一个额外的项。 \[其他方案是可更新的，但可能需要更复杂的架构。]
* _速度_\
  虽然 Pedersen 承诺并不是计算速度最快的承诺（FRI 承诺要快得多），但我们已经做出了一些选择，以便它们仍然足够快。许多承诺方案依赖于椭圆曲线的选择。因为我们需要快速的计算时间，我们在第一个版本中使用 Curve25519，因为它是最快的椭圆曲线之一\[使用另一条曲线的原因是如果我们想使用需要椭圆曲线配对的协议。这很有帮助，但代价是配对友好的曲线要慢得多。]，并且它在密码学中享有盛誉。此外，由于承诺的结构非常重复，计算可以很容易地并行化。因此，我们正在将这些承诺的计算推向 GPU，它的计算速度比 CPU 快 100-1000 倍。

最后应该说明我们用于承诺的开放方案。因为渐近验证器时间和证明大小对于 SQL 防篡改应用程序来说并不重要，所以我们可以使用不像大多数其他应用程序需要的那样简洁的方案。这意味着我们可以使用既简单又需要更少往返时间的方案。出于这些原因，我们选择使用 Bulletproofs 使用的承诺方案 \[我们没有使用相关的bulletproof协议，只用了它的 PCS。]。这是一个有吸引力的选择，因为它是一个经过良好测试的开源系统，我们可以快速使用。但是，与本节中关于承诺的几乎所有其他内容一样，这实际上是一个“黑匣子”，因此随着我们产品的发展，我们很可能会将此方案替换为专门针对我们的用例量身定制的承诺方案。

## 示例 <a href="#example" id="example"></a>

我们将通过一个示例，客户端创建一个表，附加到它，然后查询该表。

### 表创建 <a href="#table-creation" id="table-creation"></a>

客户端将以下数据发送给验证者以创建包含一些数据的表。

表 3: 示例员工表表

| Weekly Pay | Yearly Bonus |
| ---------- | ------------ |
| 4000       | 50000        |
| 7500       | 0            |
| 5000       | 400000       |
| 1500       | 0            |

验证者接受此数据并计算对每一列的承诺。在这种情况下，这些是：

<figure><img src="https://1359882050-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FzHdDPR6NVKwyccJmuzAw%2Fuploads%2FxnOuwvejhTesbA2NLf8N%2Fimage.png?alt=media&#x26;token=c5bec9bd-4edd-4906-bbc7-e21eba7f60fd" alt=""><figcaption></figcaption></figure>

然后验证者存储这些承诺并将数据发送给证明者，证明者在数据库中创建一个新表。

### 表格追加 <a href="#table-append" id="table-append"></a>

假设客户端随后将以下新的数据行发送给验证者。

表 4: 要附加到员工表的行

| Weekly Pay | Yearly Bonus |
| ---------- | ------------ |
| 3000       | 100000       |
| 4500       | 30000        |

验证者接受此数据并更新对每一列的承诺。在这种情况下，新的承诺是：

<figure><img src="https://1359882050-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FzHdDPR6NVKwyccJmuzAw%2Fuploads%2FeHvCG452WMpiDeO0U4XU%2Fimage2.png?alt=media&#x26;token=f7765a25-12b5-4314-a8de-96747602ca39" alt=""><figcaption></figcaption></figure>

然后验证者将新行发送给证明者，证明者将它们附加到数据库中的表中。

### SQL查询 <a href="#sql-query" id="sql-query"></a>

最后，客户决定它想知道总补偿。因此，它将以下 SQL 查询发送给验证者。

清单 2: 所有需补上的 SQL 查询

```sql
SELECT Pay*52+Bonus FROM Employees
```

验证者将查询路由到证明者，然后证明者通过获取 Pay 列、将其乘以 52 并添加 Bonus 列来执行查询。这是返回给验证者的结果。因为这个查询非常简单，所以证明者不需要向验证者发送任何额外的证明。

表 5: 员工表以及总薪酬

| Weekly Pay | Yearly Bonus | result |
| ---------- | ------------ | ------ |
| 4000       | 50000        | 258000 |
| 7500       | 0            | 390000 |
| 5000       | 400000       | 660000 |
| 1500       | 0            | 78000  |
| 3000       | 100000       | 256000 |
| 4500       | 30000        | 264000 |

验证者从证明者那里接收到这个结果列。为了验证它，验证者必须通过接受支付承诺、将其乘以 52 并添加奖励承诺来反映证明者的计算。恰好是对结果应该是什么的承诺\[大多数操作不是这么简单，对于更复杂的操作，还需要证明]

<figure><img src="https://1359882050-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FzHdDPR6NVKwyccJmuzAw%2Fuploads%2Fn2u7S3nXkNxSvK8vekAI%2Fimage3.png?alt=media&#x26;token=dbe22116-15cc-42e6-b66e-425ec25817f6" alt=""><figcaption></figcaption></figure>

然后，验证者还计算对从证明者发送的结果的承诺。如果证明者发送了正确的结果，这应该是

<figure><img src="https://1359882050-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FzHdDPR6NVKwyccJmuzAw%2Fuploads%2FWf2NefqQMEGi0oHdpa4H%2Fimage4.png?alt=media&#x26;token=34a34c66-91cd-49da-9348-9760e4b009e3" alt=""><figcaption></figcaption></figure>

如果证明者发送了错误的结果，这将是另一回事。然后，验证者检查是否

<figure><img src="https://1359882050-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FzHdDPR6NVKwyccJmuzAw%2Fuploads%2FUDWkA0XRo9CbyaQwz8Xl%2Fimage5.png?alt=media&#x26;token=fe7f17a6-8a02-477a-bd37-2a3cc9773aa7" alt=""><figcaption></figcaption></figure>

\[通过结合所有前面的等式可以看出这两个应该是相同的]。如果是，验证者将结果连同一个确认结果正确的标志一起发送给客户端。如果这两个值不相等，则验证者会告诉客户端存在错误\[验证者还应该提醒网络注意证明者可能是恶意的事实]。
