### Jsoft_Jid_moreapi

 <img src="https://www.jsoftstudio.top/css/Jsoft_logo.png" width = "100" height = "100" alt="Jsoft_logo" align=center />

###### ©2024-2025 Jsoft Studio

------

  <img src="https://img.shields.io/github/stars/kamcdev/Jsoft_Jid_moreapi.svg">
  <img src="https://img.shields.io/badge/交流QQ群-984242265-blue">
  <img src="https://img.shields.io/badge/B站-J软件官方-light">
  <img src="https://img.shields.io/badge/官网-www.jsoftstudio.top-purple">

------

# Moreid API 文档

## 目录
- [Moreid API 文档](#moreid-api-文档)
  - [目录](#目录)
  - [认证方式](#认证方式)
  - [API端点](#api端点)
    - [1. OAuth授权初始化](#1-oauth授权初始化)
    - [2. 用户登录获取授权码](#2-用户登录获取授权码)
    - [3. 使用授权码获取访问令牌](#3-使用授权码获取访问令牌)
    - [4. 用户信息修改](#4-用户信息修改)
    - [5. 获取用户信息](#5-获取用户信息)
  - [错误码说明](#错误码说明)
  - [使用示例](#使用示例)
    - [使用Python调用API](#使用python调用api)
      - [使用Python调用OAuth登录流程](#使用python调用OAuth登录流程)
      - [使用Python调用信息修改API（以修改用户名为例）](#使用python调用信息修改api以修改用户名为例)
      - [使用Python调用获取用户信息API](#使用python调用获取用户信息api)
    - [使用JavaScript调用API](#使用javascript调用api)
      - [使用JavaScript调用OAuth登录流程](#使用javascript调用OAuth登录流程)
      - [使用JavaScript调用信息修改API（以修改用户名为例）](#使用javascript调用信息修改api以修改用户名为例)
      - [使用JavaScript调用获取用户信息API](#使用javascript调用获取用户信息api)
    - [使用Node.js调用API](#使用nodejs调用api)
      - [使用Node.js调用OAuth登录流程](#使用nodejs调用OAuth登录流程)
      - [使用Node.js调用信息修改API（以修改用户名为例）](#使用nodejs调用信息修改api以修改用户名为例)
      - [使用Node.js调用获取用户信息API](#使用nodejs调用获取用户信息api)
  - [注意事项](#注意事项)

本文档描述了J账号管理平台提供的API接口，供您的站点调用账户系统功能。

## 认证方式

所有API调用都需要在请求头中提供有效的API密钥进行验证：

```
X-API-Key: ml_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

如需API密钥，请前往[MoreLine - 开放平台](https://more.jsoftstudio.top/login?next=%2Fopen)申请，当前api接口为测试阶段，仅进行小范围邀请测试

系统将验证API密钥的有效性。此外，系统还会根据API密钥配置的白名单IP进行来源验证：

- 如果API密钥设置了白名单IP，则只有来自白名单IP地址的请求才能成功调用API
- 如果未设置白名单IP，则允许任意IP地址调用API

请确保在申请API密钥时正确配置白名单IP，以保障API的安全使用。

## API端点

### 1. OAuth授权初始化

**URL**: `/api/oauth/authorize`
**方法**: `GET`
**格式**: `application/json`

**请求参数**:
- 无

**成功响应**:
```json
{
  "success": true,
  "state": "随机生成的state值",
  "message": "授权初始化成功"
}
```

**失败响应**:
```json
{
  "success": false,
  "message": "错误信息"
}
```

### 2. 用户登录获取授权码

**URL**: `/api/oauth/login`
**方法**: `POST`
**格式**: `application/json`

**请求参数**:
- `user_id`: 用户ID
- `password`: 密码
- `state`: 从授权初始化接口获取的state值

**成功响应**:
```json
{
  "success": true,
  "message": "登录成功",
  "code": "授权码",
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

### 3. 使用授权码获取访问令牌

**URL**: `/api/oauth/token`
**方法**: `POST`
**格式**: `application/json`

**请求参数**:
- `code`: 授权码
- `state`: 从授权初始化接口获取的state值

**成功响应**:
```json
{
  "success": true,
  "message": "获取访问令牌成功",
  "access_token": "token_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
  "token_type": "Bearer",
  "expires_in": 3600
}
```

**失败响应**:
```json
{
  "success": false,
  "message": "错误信息"
}
```

### 4. 用户信息修改

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

### 5. 获取用户信息

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
- `403`: 请求来源不匹配已注册的白名单IP
- `404`: 请求的资源不存在
- `408`: state值过期或无效
- `409`: 授权码无效或已过期
- `500`: 服务器内部错误

## 使用示例

### 使用Python调用API

#### 使用Python调用OAuth登录流程

```python
import requests
import json
import time

# 步骤1: 获取state值
url_authorize = "https://more.jsoftstudio.top/api/oauth/authorize"
headers = {
    "X-API-Key": "ml_your_api_key",
    "Content-Type": "application/json"
}

response_authorize = requests.get(url_authorize, headers=headers)
authorize_data = response_authorize.json()

if authorize_data.get('success'):
    state = authorize_data.get('state')
    print(f"获取state成功: {state}")
    
    # 步骤2: 使用用户凭证和state获取授权码
    url_login = "https://more.jsoftstudio.top/api/oauth/login"
    payload_login = {
        "user_id": "testid",
        "password": "Test123456",
        "state": state
    }
    
    response_login = requests.post(url_login, headers=headers, data=json.dumps(payload_login))
    login_data = response_login.json()
    
    if login_data.get('success'):
        code = login_data.get('code')
        print(f"获取授权码成功: {code}")
        
        # 步骤3: 使用授权码和state获取访问令牌
        url_token = "https://more.jsoftstudio.top/api/oauth/token"
        payload_token = {
            "code": code,
            "state": state
        }
        
        response_token = requests.post(url_token, headers=headers, data=json.dumps(payload_token))
        token_data = response_token.json()
        
        if token_data.get('success'):
            access_token = token_data.get('access_token')
            token_type = token_data.get('token_type')
            expires_in = token_data.get('expires_in')
            print(f"获取访问令牌成功: {access_token}")
            print(f"令牌类型: {token_type}")
            print(f"过期时间: {expires_in}秒")
        else:
            print(f"获取访问令牌失败: {token_data.get('message')}")
    else:
        print(f"登录失败: {login_data.get('message')}")
else:
    print(f"获取state失败: {authorize_data.get('message')}")
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

#### 使用JavaScript调用OAuth登录流程

```javascript
// 步骤1: 获取state值
fetch('https://more.jsoftstudio.top/api/oauth/authorize', {
  method: 'GET',
  headers: {
    'X-API-Key': 'ml_your_api_key',
    'Content-Type': 'application/json'
  }
})
.then(response => response.json())
.then(data => {
  if (data.success) {
    const state = data.state;
    console.log(`获取state成功: ${state}`);
    
    // 步骤2: 使用用户凭证和state获取授权码
    return fetch('https://more.jsoftstudio.top/api/oauth/login', {
      method: 'POST',
      headers: {
        'X-API-Key': 'ml_your_api_key',
        'Content-Type': 'application/json'
      },
      body: JSON.stringify({
        user_id: 'testid',
        password: 'Test123456',
        state: state
      })
    });
  } else {
    throw new Error(`获取state失败: ${data.message}`);
  }
})
.then(response => response.json())
.then(data => {
  if (data.success) {
    const code = data.code;
    const state = data.state; // 假设这里的state是从步骤1保存的
    console.log(`获取授权码成功: ${code}`);
    
    // 步骤3: 使用授权码和state获取访问令牌
    return fetch('https://more.jsoftstudio.top/api/oauth/token', {
      method: 'POST',
      headers: {
        'X-API-Key': 'ml_your_api_key',
        'Content-Type': 'application/json'
      },
      body: JSON.stringify({
        code: code,
        state: state
      })
    });
  } else {
    throw new Error(`登录失败: ${data.message}`);
  }
})
.then(response => response.json())
.then(data => {
  if (data.success) {
    const access_token = data.access_token;
    const token_type = data.token_type;
    const expires_in = data.expires_in;
    console.log(`获取访问令牌成功: ${access_token}`);
    console.log(`令牌类型: ${token_type}`);
    console.log(`过期时间: ${expires_in}秒`);
  } else {
    throw new Error(`获取访问令牌失败: ${data.message}`);
  }
})
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

#### 使用Node.js调用OAuth登录流程

```javascript
const axios = require('axios');

async function oauthFlow() {
  try {
    const headers = {
      'X-API-Key': 'ml_your_api_key',
      'Content-Type': 'application/json'
    };
    
    // 步骤1: 获取state值
    const authorizeResponse = await axios.get('https://more.jsoftstudio.top/api/oauth/authorize', { headers });
    
    if (!authorizeResponse.data.success) {
      throw new Error(`获取state失败: ${authorizeResponse.data.message}`);
    }
    
    const state = authorizeResponse.data.state;
    console.log(`获取state成功: ${state}`);
    
    // 步骤2: 使用用户凭证和state获取授权码
    const loginResponse = await axios.post('https://more.jsoftstudio.top/api/oauth/login', {
      user_id: 'testid',
      password: 'Test123456',
      state: state
    }, { headers });
    
    if (!loginResponse.data.success) {
      throw new Error(`登录失败: ${loginResponse.data.message}`);
    }
    
    const code = loginResponse.data.code;
    console.log(`获取授权码成功: ${code}`);
    
    // 步骤3: 使用授权码和state获取访问令牌
    const tokenResponse = await axios.post('https://more.jsoftstudio.top/api/oauth/token', {
      code: code,
      state: state
    }, { headers });
    
    if (!tokenResponse.data.success) {
      throw new Error(`获取访问令牌失败: ${tokenResponse.data.message}`);
    }
    
    const { access_token, token_type, expires_in } = tokenResponse.data;
    console.log(`获取访问令牌成功: ${access_token}`);
    console.log(`令牌类型: ${token_type}`);
    console.log(`过期时间: ${expires_in}秒`);
    
  } catch (error) {
    console.error('Error:', error.response ? error.response.data : error.message);
  }
}

oauthFlow();
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
3. 请合理处理API响应，特别是错误情况
4. state参数用于防止CSRF攻击，请确保正确传递和验证
5. 授权码有效期为10分钟，访问令牌有效期为1小时
6. 完成授权后，建议将state参数存储在安全的位置，直到整个授权流程完成