### Jsoft_Jid_moreapi

 <img src="https://www.jsoftstudio.top/css/Jsoft_logo.png" width = "100" height = "100" alt="Jsoft_logo" align=center />

###### ©2024-2025 Jsoft Studio

------

  <img src="https://img.shields.io/github/stars/kamcdev/Jsoft_Jid_moreapi.svg">
  <img src="https://img.shields.io/badge/交流QQ群-984242265-blue">
  <img src="https://img.shields.io/badge/B站-J软件官方-light">
  <img src="https://img.shields.io/badge/官网-www.jsoftstudio.top-yellow">

------

## 目录
Moreid API 文档
  - [认证方式](#认证方式)
  - [API端点](#api端点)
    - [1. 用户登录](#1-用户登录)
    - [2. 用户信息修改](#2-用户信息修改)
    - [3. 获取用户信息](#3-获取用户信息)
  - [错误码说明](#错误码说明)
  - [使用示例](#使用示例)
    - [使用Python调用API](#使用python调用api)
      - [使用Python调用登录API](#使用python调用登录api)
      - [使用Python调用信息修改API（以修改用户名为例）](#使用python调用信息修改api以修改用户名为例)
      - [使用Python调用获取用户信息API](#使用python调用获取用户信息api)
    - [使用JavaScript调用API](#使用javascript调用api)
      - [使用JavaScript调用登录API](#使用javascript调用登录api)
      - [使用JavaScript调用信息修改API（以修改用户名为例）](#使用javascript调用信息修改api以修改用户名为例)
      - [使用JavaScript调用获取用户信息API](#使用javascript调用获取用户信息api)
    - [使用Node.js调用API](#使用nodejs调用api)
      - [使用Node.js调用登录API](#使用nodejs调用登录api)
      - [使用Node.js调用信息修改API（以修改用户名为例）](#使用nodejs调用信息修改api以修改用户名为例)
      - [使用Node.js调用获取用户信息API](#使用nodejs调用获取用户信息api)
  - [注意事项](#注意事项)

本文档描述了J账号管理平台提供的API接口，供您的站点调用账户系统功能。

## 认证方式

所有API调用都需要在请求头中提供有效的API密钥进行验证：

```
X-API-Key: ml_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```
申请API密钥可前往：[MoreLine - 开放平台](https://more.jsoftstudio.top/open)


同时，系统会验证请求来源URL，要求其与在平台上注册的回调地址匹配。

## API端点

### 1. 用户登录

**URL**: `/api/oauth/login`
**方法**: `POST`
**格式**: `application/json`

**请求参数**:
- `user_id`: 用户ID
- `password`: 密码

**成功响应**:
```json
{
  "success": true,
  "message": "登录成功",
  "access_token": "token_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
  "user": {
    "id": 1,
    "username": "用户名",
    "user_id": "userid",
    "email": "user@example.com"
  }
}
```

**失败响应**:
```json
{
  "success": false,
  "message": "错误信息"
}
```

### 2. 用户信息修改

**URL**: `/api/oauth/user/update`
**方法**: `POST`
**格式**: `application/json`

**请求参数**:
- `user_id`: 用户ID（必填）
- `username`: 新用户名（可选）
- `bio`: 个人简介（可选）

**成功响应**:
```json
{
  "success": true,
  "message": "用户信息更新成功",
  "user": {
    "id": 1,
    "username": "新用户名",
    "user_id": "userid",
    "email": "user@example.com",
    "bio": "个人简介"
  }
}
```

**失败响应**:
```json
{
  "success": false,
  "message": "错误信息"
}
```

### 3. 获取用户信息

**URL**: `/api/oauth/user/info`
**方法**: `POST`
**格式**: `application/json`

**请求参数**:
- `user_id`: 用户ID（必填）

**成功响应**:
```json
{
  "success": true,
  "message": "获取用户信息成功",
  "user": {
    "username": "用户名",
    "avatar_url": "https://more.jsoftstudio.top/uploads/images/default/avatar.svg"
  }
}
```

**失败响应**:
```json
{
  "success": false,
  "message": "错误信息"
}
```

## 错误码说明

- `400`: 请求参数错误
- `401`: 缺少API密钥或API密钥无效
- `403`: 请求来源不匹配已注册的回调地址
- `404`: 请求的资源不存在
- `500`: 服务器内部错误

## 使用示例

### 使用Python调用API

#### 使用Python调用登录API

```python
import requests
import json

url = "https://more.jsoftstudio.top/api/oauth/login"
headers = {
    "X-API-Key": "ml_your_api_key",
    "Content-Type": "application/json"
}

payload = {
    "user_id": "testid",
    "password": "Test123456"
}

response = requests.post(url, headers=headers, data=json.dumps(payload))
print(response.json())
```

#### 使用Python调用信息修改API（以修改用户名为例）

```python
import requests
import json

url = "https://more.jsoftstudio.top/api/oauth/user/update"
headers = {
    "X-API-Key": "ml_your_api_key",
    "Content-Type": "application/json"
}

payload = {
    "user_id": "testid",
    "username": "newusername"
}

response = requests.post(url, headers=headers, data=json.dumps(payload))
print(response.json())
```

#### 使用Python调用获取用户信息API

```python
import requests
import json

url = "https://more.jsoftstudio.top/api/oauth/user/info"
headers = {
    "X-API-Key": "ml_your_api_key",
    "Content-Type": "application/json"
}

payload = {
    "user_id": "testid"
}

response = requests.post(url, headers=headers, data=json.dumps(payload))
print(response.json())
```

### 使用JavaScript调用API

#### 使用JavaScript调用登录API

```javascript
fetch('https://more.jsoftstudio.top/api/oauth/login', {
  method: 'POST',
  headers: {
    'X-API-Key': 'ml_your_api_key',
    'Content-Type': 'application/json'
  },
  body: JSON.stringify({
    username: 'testuser',
    password: 'testpassword'
  })
})
.then(response => response.json())
.then(data => console.log(data))
.catch(error => console.error('Error:', error));
```

#### 使用JavaScript调用信息修改API（以修改用户名为例）

```javascript
fetch('https://more.jsoftstudio.top/api/oauth/user/update', {
  method: 'POST',
  headers: {
    'X-API-Key': 'ml_your_api_key',
    'Content-Type': 'application/json'
  },
  body: JSON.stringify({
    user_id: 'testid',
    username: 'newusername'
  })
})
.then(response => response.json())
.then(data => console.log(data))
.catch(error => console.error('Error:', error));
```

#### 使用JavaScript调用获取用户信息API

```javascript
fetch('https://more.jsoftstudio.top/api/oauth/user/info', {
  method: 'POST',
  headers: {
    'X-API-Key': 'ml_your_api_key',
    'Content-Type': 'application/json'
  },
  body: JSON.stringify({
    user_id: 'testid'
  })
})
.then(response => response.json())
.then(data => console.log(data))
.catch(error => console.error('Error:', error));
```

### 使用Node.js调用API

#### 使用Node.js调用登录API

```javascript
const axios = require('axios');

async function login() {
  try {
    const response = await axios.post('https://more.jsoftstudio.top/api/oauth/login', {
      username: 'testuser',
      password: 'testpassword'
    }, {
      headers: {
        'X-API-Key': 'ml_your_api_key',
        'Content-Type': 'application/json'
      }
    });
    console.log(response.data);
  } catch (error) {
    console.error('Error:', error.response ? error.response.data : error.message);
  }
}

login();
```

#### 使用Node.js调用信息修改API（以修改用户名为例）

```javascript
const axios = require('axios');

async function updateUser() {
  try {
    const response = await axios.post('https://more.jsoftstudio.top/api/oauth/user/update', {
      user_id: 'testid',
      username: 'newusername'
    }, {
      headers: {
        'X-API-Key': 'ml_your_api_key',
        'Content-Type': 'application/json'
      }
    });
    console.log(response.data);
  } catch (error) {
    console.error('Error:', error.response ? error.response.data : error.message);
  }
}

updateUser();
```

#### 使用Node.js调用获取用户信息API

```javascript
const axios = require('axios');

async function getUserInfo() {
  try {
    const response = await axios.post('https://more.jsoftstudio.top/api/oauth/user/info', {
      user_id: 'testid'
    }, {
      headers: {
        'X-API-Key': 'ml_your_api_key',
        'Content-Type': 'application/json'
      }
    });
    console.log(response.data);
  } catch (error) {
    console.error('Error:', error.response ? error.response.data : error.message);
  }
}

getUserInfo();
```

## 注意事项

1. 请确保API密钥的安全性，不要在前端代码中暴露
2. 所有API调用必须使用HTTPS协议
3. 请合理处理API响应
4. 请确保请求来源url与申请密钥时设置的回调地址相同，若更换url，请及时申请新密钥并联系站长（B站：J软件官方，QQ：2450069268）废除旧密钥