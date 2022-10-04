---
description: 安全工作流程第 2 部分的 API。
---

# 用户认证

## **验证码 API**

为提供的 `userId` 生成一个临时 `authCode`（5 分钟有效期后过期）。

****

{% swagger method="post" path="" baseUrl="/api/v1/auth/code" summary="发起认证 - 请求认证码" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="body" name="userId" required="true" %}
唯一用户标识
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="为提供的 userId 生成临时 authCode" %}
```javascript
string
```
{% endswagger-response %}

{% swagger-response status="404: Not Found" description="userId在平台中未找到" %}
```javascript
string
```
{% endswagger-response %}
{% endswagger %}

#### 试一试：

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://api.spaceandtime.io/api/v1/auth/code' \
--header 'Content-Type: application/json' \
--header 'Accept: */*' \
--data-raw '{
 "userId": "<USERID>"
}'
```
{% endtab %}

{% tab title="Javascript" %}
```javascript
var myHeaders = new Headers();
myHeaders.append("Content-Type", "application/json");
myHeaders.append("Accept", "*/*");

var raw = JSON.stringify({
  "userId": "<USERID>"
});

var requestOptions = {
  method: 'POST',
  headers: myHeaders,
  body: raw,
  redirect: 'follow'
};

fetch("https://api.spaceandtime.io/api/v1/auth/code", requestOptions)
  .then(response => response.text())
  .then(result => console.log(result))
  .catch(error => console.log('error', error));
```
{% endtab %}

{% tab title="Python" %}
```python
import requests
import json

url = "https://api.spaceandtime.io/api/v1/auth/code"

payload = json.dumps({
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

## **Token请求**

生成 `accessToken` 和 `refreshToken`。对于完全的身份验证，请提供：`userId`、`authCode`、`signature`。对于重新认证，请提供：`refreshToken`。

{% swagger method="post" path="" baseUrl="/api/v1/auth/token" summary="完成身份验证 - 请求访问/刷新token" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="body" name="userId" type="String" %}
唯一用户标识
{% endswagger-parameter %}

{% swagger-parameter in="body" name="authCode" type="" %}
从验证码API接收
{% endswagger-parameter %}

{% swagger-parameter in="body" name="signature" %}
根据 userId、authCode 和私钥计算
{% endswagger-parameter %}

{% swagger-parameter in="body" name="refreshToken" %}
从之前的token请求 API 接收
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="已成功提供访问和刷新令牌" %}
```javascript
{
  "accessToken": "string", // 基于token的身份验证中的访问token，用来允许应用程序访问 API
  "refreshToken": "string", // 访问token过期后，应用程序可以使用刷新token重新生成新的访问token
  "accessTokenExpires": 0, // 访问token过期时间
  "refreshTokenExpires": 0 // 刷新token过期时间
}
```
{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="authCode 或签名无效或过期" %}
```javascript
{
  "accessToken": "string", // 基于token的身份验证中的访问token，用来允许应用程序访问 API
  "refreshToken": "string", // 访问token过期后，应用程序可以使用刷新token重新生成新的访问token
  "accessTokenExpires": 0, // 访问token过期时间
  "refreshTokenExpires": 0 // Refresh token expiry time
}
```
{% endswagger-response %}

{% swagger-response status="404: Not Found" description="userId在平台中未找到" %}
```javascript
{
  "accessToken": "string", // 基于token的身份验证中的访问token，用来允许应用程序访问 API
  "refreshToken": "string", // 访问token过期后，应用程序可以使用刷新token重新生成新的访问token
  "accessTokenExpires": 0, // 访问token过期时间
  "refreshTokenExpires": 0 // 刷新token过期时间
}
```
{% endswagger-response %}
{% endswagger %}

#### 试一试：

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://api.spaceandtime.io/api/v1/auth/token' \
--header 'Content-Type: application/json' \
--header 'Accept: */*' \
--data-raw '{
 "userId": "<USERID>",
 "authCode": "<AUTHCODE>",
 "signature": "<SIGNATURE>"
}'
```
{% endtab %}

{% tab title="Javascript" %}
```javascript
var myHeaders = new Headers();
myHeaders.append("Content-Type", "application/json");
myHeaders.append("Accept", "*/*");
 
var raw = JSON.stringify({
  "userId": "<USERID>",
  "authCode": "<AUTHCODE>",
  "signature": "<SIGNATURE>"
});
 
var requestOptions = {
  method: 'POST',
  headers: myHeaders,
  body: raw,
  redirect: 'follow'
};
 
fetch("https://api.spaceandtime.io/api/v1/auth/token", requestOptions)
  .then(response => response.text())
  .then(result => console.log(result))
  .catch(error => console.log('error', error));
```
{% endtab %}

{% tab title="Python" %}
```python
import requests
import json
 
url = "https://api.spaceandtime.io/api/v1/auth/token"
 
payload = json.dumps({
  "userId": "<USERID>",
  "authCode": "<AUTHCODE>",
  "signature": "<SIGNATURE>"
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

