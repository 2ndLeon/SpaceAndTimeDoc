# 概览

Space and Time由两个互操作的方面组成：数据进入系统的网关和存储和分析数据的数据仓库。 SxT 平台的成功取决于网关和数据仓库的无缝交互，以方便用户轻松访问企业级数据存储、闪电般快速的分析以及轻松连接链上和链下数据。

## 网关/数据仓库交互

下图演示了网关组件和服务如何与数据仓库交互：

<figure><img src="https://1359882050-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FzHdDPR6NVKwyccJmuzAw%2Fuploads%2FymKXlxgZmXtkheuGU54D%2Fgateway%20interfaces.png?alt=media&#x26;token=64490662-ca4b-425c-abc0-10ca8ef3898c" alt=""><figcaption><p>SxT 网关和数据仓库交互性</p></figcaption></figure>

### 路由

路由支持与去中心化数据仓库网络的事务和查询交互。

* 将核心 API 连接到 OLTP 引擎
* 为用户提供执行 SQL 的能力
* 读写交互

Core API 支持用户通过 REST API、GraphQL 或 JDBC 连接与数据平台进行 SQL 交互。

### 流

流作为大容量客户流（事件驱动）工作负载的接收器。

* 连接 Kafka 和数据摄取
* 为用户提供将数据流式传输到平台的能力
* 只写交互（从 Kafka 读取数据）

用户可以将他们的应用程序连接到 Gateway Kafka，作为其数据管道的接收器。数据仓库将通过灵活和动态的开源协议从 Kafka 中提取用户生成的数据。

### 查询证明

查询证明向平台提供 SQL 证明。

* 连接验证者和 SQL 引擎证明
* 为用户提供执行防篡改查询的能力
* 只读交互

验证者与数据仓库交互以发送入站请求的安全参数并查询响应的结果证明。

### 智能

智能支持平台治理和运营管理/编排。

* 连接 Orchestrator 和网络管理
* 提供数据仓库编排功能
* 没有直接的用户交互

Orchestrator 指示数据仓库的网络管理端点配置输入/输出操作（例如 WAN CDC 和数据摄取）。网络管理还发送回集群检测信号，以便根据需要从主集群快速故障转移到辅助集群。
