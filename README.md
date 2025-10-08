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

如需API密钥，请前往[MoreLine - 开放平台](https://more.jsoftstudio.top/open)申请，当前api接口为测试阶段，仅进行小范围邀请测试

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
    "avatar_url": "http://154.37.221.153:38871/uploads/images/default/avatar.svg"
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

# API基础URL
BASE_URL = "http://154.37.221.153:38871"
# API密钥
API_KEY = "ml_your_api_key"

# 创建会话对象
session = requests.Session()
# 设置会话级别的请求头
session.headers.update({
    "X-API-Key": API_KEY,
    "Content-Type": "application/json"
})

def test_oauth_flow_with_debug():
    """带调试信息的OAuth流程测试"""
    print("==== OAuth 2.0 授权流程 ====")
    print(f"使用的API密钥: {API_KEY}")
    
    # 步骤1: 获取state值
    print("\n步骤1: 初始化授权，获取state值...")
    url_authorize = f"{BASE_URL}/api/oauth/authorize"
    
    try:
        print(f"正在发送请求到: {url_authorize}")
        print(f"请求头: {session.headers}")
        
        response_authorize = session.get(url_authorize)
        
        print(f"\nHTTP状态码: {response_authorize.status_code}")
        print(f"响应内容: {response_authorize.text}")
        
        # 尝试解析JSON响应
        try:
            authorize_data = response_authorize.json()
            print(f"\n解析后的响应: {json.dumps(authorize_data, indent=2, ensure_ascii=False)}")
            
            if authorize_data.get('success'):
                state = authorize_data.get('state')
                print(f"✓ 获取state成功: {state}")
                
                # 步骤2: 使用用户凭证和state获取授权码
                print("\n步骤2: 使用用户凭证和state获取授权码...")
                url_login = f"{BASE_URL}/api/oauth/login"
                payload_login = {
                    "user_id": "testid",
                    "password": "Test123456",
                    "state": state
                }
                
                print(f"正在发送请求到: {url_login}")
                print(f"请求数据: {json.dumps(payload_login, indent=2, ensure_ascii=False)}")
                
                response_login = session.post(url_login, json=payload_login)
                
                print(f"\nHTTP状态码: {response_login.status_code}")
                print(f"响应内容: {response_login.text}")
                
                try:
                    login_data = response_login.json()
                    print(f"\n解析后的响应: {json.dumps(login_data, indent=2, ensure_ascii=False)}")
                    
                    if login_data.get('success'):
                        code = login_data.get('code')
                        print(f"✓ 获取授权码成功: {code}")
                        
                        # 步骤3: 使用授权码和state获取访问令牌
                        print("\n步骤3: 使用授权码和state获取访问令牌...")
                        url_token = f"{BASE_URL}/api/oauth/token"
                        payload_token = {
                            "code": code,
                            "state": state
                        }
                        
                        print(f"正在发送请求到: {url_token}")
                        print(f"请求数据: {json.dumps(payload_token, indent=2, ensure_ascii=False)}")
                        
                        response_token = session.post(url_token, json=payload_token)
                        
                        print(f"\nHTTP状态码: {response_token.status_code}")
                        print(f"响应内容: {response_token.text}")
                        
                        try:
                            token_data = response_token.json()
                            print(f"\n解析后的响应: {json.dumps(token_data, indent=2, ensure_ascii=False)}")
                            
                            if token_data.get('success'):
                                access_token = token_data.get('access_token')
                                token_type = token_data.get('token_type')
                                expires_in = token_data.get('expires_in')
                                print(f"✓ 获取访问令牌成功: {access_token}")
                                print(f"令牌类型: {token_type}")
                                print(f"过期时间: {expires_in}秒")
                            else:
                                print(f"✗ 获取访问令牌失败: {token_data.get('message')}")
                        except json.JSONDecodeError:
                            print("\n无法解析访问令牌响应的JSON")
                    else:
                        print(f"✗ 登录失败: {login_data.get('message')}")
                except json.JSONDecodeError:
                    print("\n无法解析登录响应的JSON")
            else:
                print(f"✗ 获取state失败: {authorize_data.get('message')}")
        except json.JSONDecodeError:
            print("\n无法解析授权响应的JSON")
    except requests.exceptions.RequestException as e:
        print(f"✗ 网络请求错误: {str(e)}")
    except Exception as e:
        print(f"✗ 发生错误: {str(e)}")

if __name__ == "__main__":
    print("API密钥诊断工具")
    print("="*50)
    
    # 测试OAuth流程（带调试信息）
    test_oauth_flow_with_debug()
```

#### 使用Python调用信息修改API（以修改用户名为例）

```python
import requests
import json

# API基础URL
BASE_URL = "http://154.37.221.153:38871"
# API密钥
API_KEY = "ml_your_api_key"

# 创建会话对象
session = requests.Session()
# 设置会话级别的请求头
session.headers.update({
    "X-API-Key": API_KEY,
    "Content-Type": "application/json"
})

def update_user_info():
    """修改用户信息"""
    print("==== 修改用户信息 ====")
    print(f"使用的API密钥: {API_KEY}")
    
    url = f"{BASE_URL}/api/oauth/user/update"
    payload = {
        "user_id": "testid",
        "username": "newusername"
    }
    
    try:
        print(f"正在发送请求到: {url}")
        print(f"请求头: {session.headers}")
        print(f"请求数据: {json.dumps(payload, indent=2, ensure_ascii=False)}")
        
        response = session.post(url, json=payload)
        
        print(f"\nHTTP状态码: {response.status_code}")
        print(f"响应内容: {response.text}")
        
        # 尝试解析JSON响应
        try:
            data = response.json()
            print(f"\n解析后的响应: {json.dumps(data, indent=2, ensure_ascii=False)}")
            
            if data.get('success'):
                print(f"✓ 用户信息更新成功")
                user_info = data.get('user', {})
                print(f"更新后的用户名: {user_info.get('username')}")
            else:
                print(f"✗ 用户信息更新失败: {data.get('message')}")
        except json.JSONDecodeError:
            print("\n无法解析响应的JSON")
    except requests.exceptions.RequestException as e:
        print(f"✗ 网络请求错误: {str(e)}")
    except Exception as e:
        print(f"✗ 发生错误: {str(e)}")

if __name__ == "__main__":
    print("API密钥诊断工具")
    print("="*50)
    
    # 修改用户信息
    update_user_info()
```

#### 使用Python调用获取用户信息API

```python
import requests
import json

# API基础URL
BASE_URL = "http://154.37.221.153:38871"
# API密钥
API_KEY = "ml_your_api_key"

# 创建会话对象
session = requests.Session()
# 设置会话级别的请求头
session.headers.update({
    "X-API-Key": API_KEY,
    "Content-Type": "application/json"
})

def get_user_info():
    """获取用户信息"""
    print("==== 获取用户信息 ====")
    print(f"使用的API密钥: {API_KEY}")
    
    url = f"{BASE_URL}/api/oauth/user/info"
    payload = {
        "user_id": "testid"
    }
    
    try:
        print(f"正在发送请求到: {url}")
        print(f"请求头: {session.headers}")
        print(f"请求数据: {json.dumps(payload, indent=2, ensure_ascii=False)}")
        
        response = session.post(url, json=payload)
        
        print(f"\nHTTP状态码: {response.status_code}")
        print(f"响应内容: {response.text}")
        
        # 尝试解析JSON响应
        try:
            data = response.json()
            print(f"\n解析后的响应: {json.dumps(data, indent=2, ensure_ascii=False)}")
            
            if data.get('success'):
                print(f"✓ 获取用户信息成功")
                user_info = data.get('user', {})
                print(f"用户名: {user_info.get('username')}")
                print(f"头像URL: {user_info.get('avatar_url')}")
            else:
                print(f"✗ 获取用户信息失败: {data.get('message')}")
        except json.JSONDecodeError:
            print("\n无法解析响应的JSON")
    except requests.exceptions.RequestException as e:
        print(f"✗ 网络请求错误: {str(e)}")
    except Exception as e:
        print(f"✗ 发生错误: {str(e)}")

if __name__ == "__main__":
    print("API密钥诊断工具")
    print("="*50)
    
    # 获取用户信息
    get_user_info()
```

### 使用JavaScript调用API

#### 使用JavaScript调用OAuth登录流程

```javascript
// API基础URL
const BASE_URL = "http://154.37.221.153:38871";
// API密钥
const API_KEY = "ml_your_api_key";

// 创建请求头对象
const headers = {
    "X-API-Key": API_KEY,
    "Content-Type": "application/json"
};

/**
 * 带调试信息的OAuth流程测试
 */
async function testOauthFlowWithDebug() {
    console.log("==== OAuth 2.0 授权流程 ====");
    console.log(`使用的API密钥: ${API_KEY}`);
    
    try {
        // 步骤1: 获取state值
        console.log("\n步骤1: 初始化授权，获取state值...");
        const urlAuthorize = `${BASE_URL}/api/oauth/authorize`;
        
        console.log(`正在发送请求到: ${urlAuthorize}`);
        console.log(`请求头:`, headers);
        
        const responseAuthorize = await fetch(urlAuthorize, {
            method: 'GET',
            headers: headers
        });
        
        console.log(`\nHTTP状态码: ${responseAuthorize.status}`);
        
        const authorizeText = await responseAuthorize.text();
        console.log(`响应内容: ${authorizeText}`);
        
        // 尝试解析JSON响应
        try {
            const authorizeData = JSON.parse(authorizeText);
            console.log(`\n解析后的响应:`, JSON.stringify(authorizeData, null, 2));
            
            if (authorizeData.success) {
                const state = authorizeData.state;
                console.log(`✓ 获取state成功: ${state}`);
                
                // 步骤2: 使用用户凭证和state获取授权码
                console.log("\n步骤2: 使用用户凭证和state获取授权码...");
                const urlLogin = `${BASE_URL}/api/oauth/login`;
                const payloadLogin = {
                    "user_id": "testid",
                    "password": "Test123456",
                    "state": state
                };
                
                console.log(`正在发送请求到: ${urlLogin}`);
                console.log(`请求数据:`, JSON.stringify(payloadLogin, null, 2));
                
                const responseLogin = await fetch(urlLogin, {
                    method: 'POST',
                    headers: headers,
                    body: JSON.stringify(payloadLogin)
                });
                
                console.log(`\nHTTP状态码: ${responseLogin.status}`);
                
                const loginText = await responseLogin.text();
                console.log(`响应内容: ${loginText}`);
                
                try {
                    const loginData = JSON.parse(loginText);
                    console.log(`\n解析后的响应:`, JSON.stringify(loginData, null, 2));
                    
                    if (loginData.success) {
                        const code = loginData.code;
                        console.log(`✓ 获取授权码成功: ${code}`);
                        
                        // 步骤3: 使用授权码和state获取访问令牌
                        console.log("\n步骤3: 使用授权码和state获取访问令牌...");
                        const urlToken = `${BASE_URL}/api/oauth/token`;
                        const payloadToken = {
                            "code": code,
                            "state": state
                        };
                        
                        console.log(`正在发送请求到: ${urlToken}`);
                        console.log(`请求数据:`, JSON.stringify(payloadToken, null, 2));
                        
                        const responseToken = await fetch(urlToken, {
                            method: 'POST',
                            headers: headers,
                            body: JSON.stringify(payloadToken)
                        });
                        
                        console.log(`\nHTTP状态码: ${responseToken.status}`);
                        
                        const tokenText = await responseToken.text();
                        console.log(`响应内容: ${tokenText}`);
                        
                        try {
                            const tokenData = JSON.parse(tokenText);
                            console.log(`\n解析后的响应:`, JSON.stringify(tokenData, null, 2));
                            
                            if (tokenData.success) {
                                const accessToken = tokenData.access_token;
                                const tokenType = tokenData.token_type;
                                const expiresIn = tokenData.expires_in;
                                console.log(`✓ 获取访问令牌成功: ${accessToken}`);
                                console.log(`令牌类型: ${tokenType}`);
                                console.log(`过期时间: ${expiresIn}秒`);
                            } else {
                                console.log(`✗ 获取访问令牌失败: ${tokenData.message || '未知错误'}`);
                            }
                        } catch (jsonError) {
                            console.log("\n无法解析访问令牌响应的JSON");
                            console.error(jsonError);
                        }
                    } else {
                        console.log(`✗ 登录失败: ${loginData.message || '未知错误'}`);
                    }
                } catch (jsonError) {
                    console.log("\n无法解析登录响应的JSON");
                    console.error(jsonError);
                }
            } else {
                console.log(`✗ 获取state失败: ${authorizeData.message || '未知错误'}`);
            }
        } catch (jsonError) {
            console.log("\n无法解析授权响应的JSON");
            console.error(jsonError);
        }
    } catch (error) {
        console.log(`✗ 发生错误: ${error.message}`);
        console.error(error);
    }
}

/**
 * 修改用户信息
 */
async function updateUserInfo() {
    console.log("==== 修改用户信息 ====");
    console.log(`使用的API密钥: ${API_KEY}`);
    
    const url = `${BASE_URL}/api/oauth/user/update`;
    const payload = {
        "user_id": "testid",
        "username": "newusername"
    };
    
    try {
        console.log(`正在发送请求到: ${url}`);
        console.log(`请求头:`, headers);
        console.log(`请求数据:`, JSON.stringify(payload, null, 2));
        
        const response = await fetch(url, {
            method: 'POST',
            headers: headers,
            body: JSON.stringify(payload)
        });
        
        console.log(`\nHTTP状态码: ${response.status}`);
        
        const responseText = await response.text();
        console.log(`响应内容: ${responseText}`);
        
        try {
            const data = JSON.parse(responseText);
            console.log(`\n解析后的响应:`, JSON.stringify(data, null, 2));
            
            if (data.success) {
                console.log(`✓ 用户信息更新成功`);
                const userInfo = data.user || {};
                console.log(`更新后的用户名: ${userInfo.username || '未设置'}`);
            } else {
                console.log(`✗ 用户信息更新失败: ${data.message || '未知错误'}`);
            }
        } catch (jsonError) {
            console.log("\n无法解析响应的JSON");
            console.error(jsonError);
        }
    } catch (error) {
        console.log(`✗ 发生错误: ${error.message}`);
        console.error(error);
    }
}

/**
 * 获取用户信息
 */
async function getUserInfo() {
    console.log("==== 获取用户信息 ====");
    console.log(`使用的API密钥: ${API_KEY}`);
    
    const url = `${BASE_URL}/api/oauth/user/info`;
    const payload = {
        "user_id": "testid"
    };
    
    try {
        console.log(`正在发送请求到: ${url}`);
        console.log(`请求头:`, headers);
        console.log(`请求数据:`, JSON.stringify(payload, null, 2));
        
        const response = await fetch(url, {
            method: 'POST',
            headers: headers,
            body: JSON.stringify(payload)
        });
        
        console.log(`\nHTTP状态码: ${response.status}`);
        
        const responseText = await response.text();
        console.log(`响应内容: ${responseText}`);
        
        try {
            const data = JSON.parse(responseText);
            console.log(`\n解析后的响应:`, JSON.stringify(data, null, 2));
            
            if (data.success) {
                console.log(`✓ 获取用户信息成功`);
                const userInfo = data.user || {};
                console.log(`用户名: ${userInfo.username || '未设置'}`);
                console.log(`头像URL: ${userInfo.avatar_url || '未设置'}`);
            } else {
                console.log(`✗ 获取用户信息失败: ${data.message || '未知错误'}`);
            }
        } catch (jsonError) {
            console.log("\n无法解析响应的JSON");
            console.error(jsonError);
        }
    } catch (error) {
        console.log(`✗ 发生错误: ${error.message}`);
        console.error(error);
    }
}

// 运行OAuth流程测试
console.log("API密钥诊断工具");
console.log(".".repeat(50));
testOauthFlowWithDebug();

// 如果需要运行其他功能，取消注释下面的代码
// updateUserInfo();
// getUserInfo();
```

### 使用Node.js调用API

#### 使用Node.js调用OAuth登录流程

```javascript
const axios = require('axios');

// API基础URL
const BASE_URL = "http://154.37.221.153:38871";
// API密钥
const API_KEY = "ml_your_api_key";

// 创建axios实例
const apiClient = axios.create({
    baseURL: BASE_URL,
    headers: {
        "X-API-Key": API_KEY,
        "Content-Type": "application/json"
    },
    timeout: 10000 // 设置请求超时时间
});

/**
 * 带调试信息的OAuth流程测试
 */
async function testOauthFlowWithDebug() {
    console.log("==== OAuth 2.0 授权流程 ====");
    console.log(`使用的API密钥: ${API_KEY}`);
    
    try {
        // 步骤1: 获取state值
        console.log("\n步骤1: 初始化授权，获取state值...");
        const urlAuthorize = "/api/oauth/authorize";
        
        console.log(`正在发送请求到: ${BASE_URL}${urlAuthorize}`);
        console.log(`请求头:`, apiClient.defaults.headers);
        
        const responseAuthorize = await apiClient.get(urlAuthorize);
        
        console.log(`\nHTTP状态码: ${responseAuthorize.status}`);
        console.log(`响应内容:`, responseAuthorize.data);
        
        const authorizeData = responseAuthorize.data;
        console.log(`\n解析后的响应:`, JSON.stringify(authorizeData, null, 2));
        
        if (authorizeData.success) {
            const state = authorizeData.state;
            console.log(`✓ 获取state成功: ${state}`);
            
            // 步骤2: 使用用户凭证和state获取授权码
            console.log("\n步骤2: 使用用户凭证和state获取授权码...");
            const urlLogin = "/api/oauth/login";
            const payloadLogin = {
                "user_id": "testid",
                "password": "Test123456",
                "state": state
            };
            
            console.log(`正在发送请求到: ${BASE_URL}${urlLogin}`);
            console.log(`请求数据:`, JSON.stringify(payloadLogin, null, 2));
            
            const responseLogin = await apiClient.post(urlLogin, payloadLogin);
            
            console.log(`\nHTTP状态码: ${responseLogin.status}`);
            console.log(`响应内容:`, responseLogin.data);
            
            const loginData = responseLogin.data;
            console.log(`\n解析后的响应:`, JSON.stringify(loginData, null, 2));
            
            if (loginData.success) {
                const code = loginData.code;
                console.log(`✓ 获取授权码成功: ${code}`);
                
                // 步骤3: 使用授权码和state获取访问令牌
                console.log("\n步骤3: 使用授权码和state获取访问令牌...");
                const urlToken = "/api/oauth/token";
                const payloadToken = {
                    "code": code,
                    "state": state
                };
                
                console.log(`正在发送请求到: ${BASE_URL}${urlToken}`);
                console.log(`请求数据:`, JSON.stringify(payloadToken, null, 2));
                
                const responseToken = await apiClient.post(urlToken, payloadToken);
                
                console.log(`\nHTTP状态码: ${responseToken.status}`);
                console.log(`响应内容:`, responseToken.data);
                
                const tokenData = responseToken.data;
                console.log(`\n解析后的响应:`, JSON.stringify(tokenData, null, 2));
                
                if (tokenData.success) {
                    const accessToken = tokenData.access_token;
                    const tokenType = tokenData.token_type;
                    const expiresIn = tokenData.expires_in;
                    console.log(`✓ 获取访问令牌成功: ${accessToken}`);
                    console.log(`令牌类型: ${tokenType}`);
                    console.log(`过期时间: ${expiresIn}秒`);
                } else {
                    console.log(`✗ 获取访问令牌失败: ${tokenData.message || '未知错误'}`);
                }
            } else {
                console.log(`✗ 登录失败: ${loginData.message || '未知错误'}`);
            }
        } else {
            console.log(`✗ 获取state失败: ${authorizeData.message || '未知错误'}`);
        }
    } catch (error) {
        if (error.response) {
            // 服务器返回错误状态码
            console.log(`✗ 服务器返回错误状态码: ${error.response.status}`);
            console.log(`响应内容:`, error.response.data);
        } else if (error.request) {
            // 请求已发出但没有收到响应
            console.log(`✗ 请求已发出但没有收到响应`);
            console.log(`请求详情:`, error.request);
        } else {
            // 请求配置出错
            console.log(`✗ 请求配置出错: ${error.message}`);
        }
        console.error(error);
    }
}

/**
 * 修改用户信息
 */
async function updateUserInfo() {
    console.log("==== 修改用户信息 ====");
    console.log(`使用的API密钥: ${API_KEY}`);
    
    const url = "/api/oauth/user/update";
    const payload = {
        "user_id": "testid",
        "username": "newusername"
    };
    
    try {
        console.log(`正在发送请求到: ${BASE_URL}${url}`);
        console.log(`请求头:`, apiClient.defaults.headers);
        console.log(`请求数据:`, JSON.stringify(payload, null, 2));
        
        const response = await apiClient.post(url, payload);
        
        console.log(`\nHTTP状态码: ${response.status}`);
        console.log(`响应内容:`, response.data);
        
        const data = response.data;
        console.log(`\n解析后的响应:`, JSON.stringify(data, null, 2));
        
        if (data.success) {
            console.log(`✓ 用户信息更新成功`);
            const userInfo = data.user || {};
            console.log(`更新后的用户名: ${userInfo.username || '未设置'}`);
        } else {
            console.log(`✗ 用户信息更新失败: ${data.message || '未知错误'}`);
        }
    } catch (error) {
        if (error.response) {
            // 服务器返回错误状态码
            console.log(`✗ 服务器返回错误状态码: ${error.response.status}`);
            console.log(`响应内容:`, error.response.data);
        } else if (error.request) {
            // 请求已发出但没有收到响应
            console.log(`✗ 请求已发出但没有收到响应`);
            console.log(`请求详情:`, error.request);
        } else {
            // 请求配置出错
            console.log(`✗ 请求配置出错: ${error.message}`);
        }
        console.error(error);
    }
}

/**
 * 获取用户信息
 */
async function getUserInfo() {
    console.log("==== 获取用户信息 ====");
    console.log(`使用的API密钥: ${API_KEY}`);
    
    const url = "/api/oauth/user/info";
    const payload = {
        "user_id": "testid"
    };
    
    try {
        console.log(`正在发送请求到: ${BASE_URL}${url}`);
        console.log(`请求头:`, apiClient.defaults.headers);
        console.log(`请求数据:`, JSON.stringify(payload, null, 2));
        
        const response = await apiClient.post(url, payload);
        
        console.log(`\nHTTP状态码: ${response.status}`);
        console.log(`响应内容:`, response.data);
        
        const data = response.data;
        console.log(`\n解析后的响应:`, JSON.stringify(data, null, 2));
        
        if (data.success) {
            console.log(`✓ 获取用户信息成功`);
            const userInfo = data.user || {};
            console.log(`用户名: ${userInfo.username || '未设置'}`);
            console.log(`头像URL: ${userInfo.avatar_url || '未设置'}`);
        } else {
            console.log(`✗ 获取用户信息失败: ${data.message || '未知错误'}`);
        }
    } catch (error) {
        if (error.response) {
            // 服务器返回错误状态码
            console.log(`✗ 服务器返回错误状态码: ${error.response.status}`);
            console.log(`响应内容:`, error.response.data);
        } else if (error.request) {
            // 请求已发出但没有收到响应
            console.log(`✗ 请求已发出但没有收到响应`);
            console.log(`请求详情:`, error.request);
        } else {
            // 请求配置出错
            console.log(`✗ 请求配置出错: ${error.message}`);
        }
        console.error(error);
    }
}

// 运行OAuth流程测试
console.log("API密钥诊断工具");
console.log("=" * 50);
testOauthFlowWithDebug();

// 如果需要运行其他功能，取消注释下面的代码
// updateUserInfo();
// getUserInfo();
```

## 注意事项

1. 请确保API密钥的安全性，不要在前端代码中暴露
2. 所有API调用必须使用HTTPS协议
3. 请合理处理API响应，特别是错误情况
4. state参数用于防止CSRF攻击，请确保正确传递和验证
5. 授权码有效期为10分钟，访问令牌有效期为1小时
6. 完成授权后，建议将state参数存储在安全的位置，直到整个授权流程完成
