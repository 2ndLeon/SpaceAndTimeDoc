---
description: 安全工作流程第 1 部分的 API。
---

# 用户注册 API

用户注册通过两部分工作流程完成。在开始注册工作流程之前，用户必须通过 ED25519 算法生成公钥/私钥对。

## 预留 API

将检查提供的 `userId` 和 `publicKey` 以确保它们是唯一的，此时它们将被临时保留用于生成的 `authCode`的生命周期（5 分钟）。

<details>

<summary><em><strong>POST</strong></em> /api/v1/auth/reserve</summary>



</details>
