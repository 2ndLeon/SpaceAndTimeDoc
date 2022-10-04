---
description: 安全工作流程第 1 部分的 API。
---

# 用户注册 API

用户注册通过两部分工作流程完成。在开始注册工作流程之前，用户必须通过 ED25519 算法生成公钥/私钥对。

## 预留 API

将检查提供的 `userId` 和 `publicKey` 以确保它们是唯一的，此时它们将被临时保留用于生成的 `authCode`的生命周期（5 分钟）。

{% swagger method="post" path="" baseUrl="/api/v1/auth/reserve" summary="开始注册 - 预留 uesrId, publicKey" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="body" name="userId" type="String" required="true" %}
用户唯一标识符
{% endswagger-parameter %}

{% swagger-parameter in="body" name="publicKey" type="String" required="true" %}
ED25519算法生成的公钥
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="已生成临时 authCode" %}
```javascript
{
    // Response
}
```
{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="该userId或者publicKey在平台中已存在" %}
```javascript
{
    // Response
}
```
{% endswagger-response %}
{% endswagger %}

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```
curl --location --request POST 'https://api.spaceandtime.io/api/v1/auth/reserve' \
--header 'Content-Type: application/json' \
--header 'Accept: */*' \
--data-raw '{
 "publicKey": "<PUBLIC_KEY>",
 "userId": "<USERID>"
}'
```
{% endcode %}
{% endtab %}

{% tab title="Second Tab" %}
```javascript
var myHeaders = new Headers();
myHeaders.append("Content-Type", "application/json");
myHeaders.append("Accept", "*/*");

var raw = JSON.stringify({
  "publicKey": "<PUBLIC_KEY>",
  "userId": "<USERID>"
});

var requestOptions = {
  method: 'POST',
  headers: myHeaders,
  body: raw,
  redirect: 'follow'
};

fetch("https://api.spaceandtime.io/api/v1/auth/reserve", requestOptions)
  .then(response => response.text())
  .then(result => console.log(result))
  .catch(error => console.log('error', error));
```
{% endtab %}

{% tab title="Python" %}
```python
import requests
import json
 
url = "https://api.spaceandtime.io/api/v1/auth/reserve"
 
payload = json.dumps({
  "publicKey": "<PUBLIC_KEY>",
  "userId": "<USERID>"
})
headers = {
  'Content-Type': 'application/json',
  'Accept': '*/*'
}
 
response = requests.request("POST", url, headers=headers, data=payload)
print(response.text)
```
{% endtab %}
{% endtabs %}

### [🔙 返回安全工作流程 ](./)

## 注册

提供的 `userId`、`authCode` 和 `signature` 将根据预留的 `userId`和 `publicKey`进行验证。成功后，用户在平台上正式注册。

{% swagger method="post" path="" baseUrl="/api/v1/auth/register" summary="完成注册 - 标识符验证" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="body" name="userId" type="String" required="true" %}
唯一用户标识
{% endswagger-parameter %}

{% swagger-parameter in="body" name="authCode" type="String" required="true" %}
通过预留API接收
{% endswagger-parameter %}

{% swagger-parameter in="body" name="signature" type="String" required="true" %}
根据 userId、authCode 和私钥计算
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="用户注册成功" %}

{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="签名（signature）无效" %}
authCode or signature is invalid or expired OR userId or publicKey already exist in the platform
{% endswagger-response %}
{% endswagger %}

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```
curl --location --request POST 'https://api.spaceandtime.io/api/v1/auth/register' \
--header 'Content-Type: application/json' \
--data-raw '{
 "authCode": "<AUTHCODE>”,
 "signature": "<SIGNATURE>",
 "userId": "<USERID>"
}'
```
{% endcode %}
{% endtab %}

{% tab title="Second Tab" %}
```javascript
var myHeaders = new Headers();
myHeaders.append("Content-Type", "application/json");
 
var raw = JSON.stringify({
  "authCode": "<AUTH_CODE>",
  "signature": "<SIGNATURE>",
  "userId": "<USERID>"
});
 
var requestOptions = {
  method: 'POST',
  headers: myHeaders,
  body: raw,
  redirect: 'follow'
};
 
fetch("https://api.spaceandtime.io/api/v1/auth/register", requestOptions)
  .then(response => response.text())
  .then(result => console.log(result))
  .catch(error => console.log('error', error));
```
{% endtab %}

{% tab title="Python" %}
```python
import requests
import json
 
url = "https://api.spaceandtime.io/api/v1/auth/register"
 
payload = json.dumps({
 "authCode": "<AUTH_CODE>",
 "signature": "<SIGNATURE>",
 "userId": "<USERID>"
})
headers = {
 'Content-Type': 'application/json'
}
 
response = requests.request("POST", url, headers=headers, data=payload)
print(response.text)
```
{% endtab %}
{% endtabs %}

### [🔙 返回安全工作流程 ](./)



