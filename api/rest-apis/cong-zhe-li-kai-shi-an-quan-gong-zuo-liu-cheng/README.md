---
description: 要使用 REST API 与 Space and Time 连接，您必须完成以下工作流程，通过 7 个简单的步骤在平台上注册和验证自己。
---

# 从这里开始：安全工作流程

## 怎样使用此工作流程

第 1 部分（用户注册，步骤 1-5）只需完成一次，即您第一次访问 Space and Time。每次访问 Space and Time 时都需要完成第 2 部分（用户身份验证，步骤 6-7）。

> &#x20;**** :thumbsup: **** [我已经注册 - 带我去用户验证](./#di-2-bu-fen-yong-hu-shen-fen-yan-zheng)

## 第 1 部分：用户注册

用户注册是安全工作流程的第一部分。完成这些步骤以在 SxT 平台上注册您自己。您只需执行一次。

<details>

<summary>第1步：选择独一无二的 <code>userId</code></summary>

您的 `userId` 是与您在 SxT 中的所有活动和资源相关联的唯一标识符。

您的 `userId` 可以是任何东西，只要它在平台中是唯一的。例子：

```
var userId = "alice";
```

#### ✅ 要求: <a href="#requirements" id="requirements"></a>

* `userId` 必须是唯一的
  * 如果您选择 `userId` 在平台上已存在, 第3步会失败

</details>

<details>

<summary>第 2 步：生成公钥/私钥对</summary>

您的 `publicKey` 和 `privateKey` 用于在 SxT 中对您进行身份验证。你的 `privateKey` 存储在你的机器上，你的 `publicKey` 被提供给平台。

使用 ED25519 算法生成您的配对：

1. 确保你的机器上安装了 OpenSSH
2. 在命令行终端中执行以下命令：

```bash
ssh-keygen -t ed25519
```

#### :white\_check\_mark: 要求:

* `publicKey` 必须是唯一的
  * 如果您提供的 `publicKey` 在平台上已经存在（之前已经注册过）， 第3步会失败

</details>

<details>

<summary>第3步：执行 Reserve API 以保留 <code>userId</code> 和 <code>publicKey</code> </summary>

API 规范可在此处找到。成功后，您将收到一个临时的 `authCode`。

#### :pencil: 注意:

* `authCode` 只有5分钟的有效期。

</details>

<details>

<summary>第4步：计算 <code>signature</code> <em></em> </summary>

您的`signature`用于验证您的 `privateKey` 的所有权。您提供的 publicKey 将用于验证`signature`。

如何计算 `signature`:&#x20;

1\. 连接您的 `userId` 和在步骤 3 中收到的 `authCode`。示例：

```
var userId = "alice";
var authCode = "1234";
var signature = userId + authCode; // "alice1234"
```

2\. 使用第2步中生成的 `privateKey` 对连接进行签名。示例：

```
privateKey.sign(userId + authCode);
```

#### &#x20;:white\_check\_mark: 要求:

* `signature` 算法必须符合ED25519
  * 大多数主要语言都有支持 ED25519 的现有加密库

</details>

<details>

<summary>第5步：执行注册 API 以验证您的身份</summary>

API 规范可在此处找到。

#### &#x20;:white\_check\_mark: 要求:

* 提供的 `userId` 必须与第 3 步中提供的 `userId` 匹配
* 提供的 `authCode` 必须与从第 3 步收到的 `authCode` 匹配

</details>

#### 以上步骤完成了吗？您已在平台注册！ :tada: 继续第 2 部分 :arrow\_down:

## 第 2 部分：用户身份验证

用户身份验证是安全工作流程中的第二部分。完成这些步骤以在 SxT 中启动新会话（完全身份验证）或刷新当前会话（重新身份验证）。您需要为每个会话执行此操作。

### 完全身份验证：启动新会话的步骤 __&#x20;

完全身份验证启动与 SxT 的新会话，该会话最多保持活动 24 小时。

<details>

<summary>第 6 步：执行 Authentication Code API 以生成 <code>authCode</code></summary>

API 规范可在[此处](yong-hu-ren-zheng.md)找到。成功后，您将收到一个临时的 `authCode`。

#### :pencil: 注意:

* `authCode` 只有5分钟有效期。

</details>

<details>

<summary>第 7a 步：提供 <code>userId</code>、<code>authCode</code> 和 <code>signature</code> 以执行 Token Request API 并接收 <code>accessToken</code> 和 <code>refreshToken</code></summary>

API 规范可在[此处](yong-hu-ren-zheng.md)找到。成功后，您将收到一个 `accessToken` 和一个 `refreshToken`。

#### :pencil: 注意:

* `accessToken` 只有25分钟有效期
* `refreshToken` 只有30分钟有效期

</details>

#### 以上步骤完成了吗？您已启动新会话！ :tada:&#x20;

您已准备好开始发出安全的 [REST API 请求](../sql-cha-xun-api.md)。

* :pencil: 注意: 第 7 步中收到的 `accessToken` 必须在非安全 API 请求中作为 Authorization 请求中的bearer token提供。

当需要刷新会话时，继续重新验证 :arrow\_down:

### 重新认证：继续会话的步骤

重新身份验证使会话保持活动状态最多 24 小时。

<details>

<summary>第 7b 步：提供当前 <code>refreshToken</code>以完成 Token Request API 接收 <code>accessToken</code>和新 <code>refreshToken</code></summary>

API 规范可在此处找到。成功后，您将收到一个 `accessToken`和一个 `refreshToken`。

:pencil: 注意:

* &#x20;`accessToken` 只有25分钟有效期
* `refreshToken` 只有30分钟有效期

</details>

以上步骤完成了吗？那么你的会话继续保持活跃了 **** :tada:****

您可以继续发出安全的 REST API 请求。

* :pencil: 注意: 步骤 7 中收到的 `accessToken`必须在非安全 API 请求中作为 Authorization 请求头中的bearer token提供。
