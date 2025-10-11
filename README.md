### Jsoft_Jid_moreapi

 <img src="https://www.jsoftstudio.top/css/Jsoft_logo.png" width = "100" height = "100" alt="Jsoft_logo" align=center />

###### ©2024-2025 Jsoft Studio

------

  <img src="https://img.shields.io/github/stars/kamcdev/Jsoft_Jid_moreapi.svg">
  <img src="https://img.shields.io/badge/交流QQ群-984242265-blue">
  <img src="https://img.shields.io/badge/B站-J软件官方-light">
  <img src="https://img.shields.io/badge/官网-www.jsoftstudio.top-purple">

------

Moreid API 文档

目录

- [认证方式](#认证方式)
- [API端点](#api端点)
  - [1. OAuth授权初始化](#1-oauth授权初始化)
  - [2. 用户登录获取授权码](#2-用户登录获取授权码)
  - [3. 使用授权码获取访问令牌](#3-使用授权码获取访问令牌)
  - [4. 用户信息修改](#4-用户信息修改)
  - [5. 获取用户信息](#5-获取用户信息)
  - [6. 获取一次性确认码](#6-获取一次性确认码)
  - [7. 用户确认页面](#7-用户确认页面)
  - [8. 生成登录码](#8-生成登录码)
  - [9. 安全登录接口](#9-安全登录接口)
  - [错误码说明](#错误码说明)
- [使用示例](#使用示例)
  - [使用Python调用API](#使用python调用api)
    - [使用Python调用OAuth登录流程](#使用python调用oauth登录流程)
    - [使用Python调用OAuth安全登录流程](#使用python调用oauth安全登录流程)
    - [使用Python调用信息修改API（以修改用户名为例）](#使用python调用信息修改api以修改用户名为例)
    - [使用Python调用获取用户信息API](#使用python调用获取用户信息api)
  - [使用JavaScript调用API](#使用javascript调用api)
    - [使用JavaScript调用OAuth登录流程](#使用javascript调用oauth登录流程)
    - [使用JavaScript调用OAuth安全登录流程](#使用javascript调用oauth安全登录流程)
    - [使用JavaScript调用信息修改API（以修改用户名为例）](#使用javascript调用信息修改api以修改用户名为例)
    - [使用JavaScript调用获取用户信息API](#使用javascript调用获取用户信息api)
- [注意事项](#注意事项)

## <a id="认证方式"></a>认证方式

所有API调用都需要在请求头中提供有效的API密钥进行验证：

```
X-API-Key: ml_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

如需API密钥，请前往MoreLine - 开放平台申请，当前api接口为测试阶段，仅进行小范围邀请测试

系统将验证API密钥的有效性。此外，系统还会根据API密钥配置的白名单IP进行来源验证：

· 如果API密钥设置了白名单IP，则只有来自白名单IP地址的请求才能成功调用API
· 如果未设置白名单IP，则允许任意IP地址调用API

请确保在申请API密钥时正确配置白名单IP，以保障API的安全使用。

## <a id="api端点"></a>API端点

## <a id="1-oauth授权初始化"></a>1. OAuth授权初始化

URL: /api/oauth/authorize
方法:GET
格式:application/json

请求参数:

· 无

成功响应:

```json
{
  "success": true,
  "state": "随机生成的state值",
  "message": "授权初始化成功"
}
```

失败响应:

```json
{
  "success": false,
  "message": "错误信息"
}
```

## <a id="2-用户登录获取授权码"></a>2. 用户登录获取授权码

URL: /api/oauth/login
方法:POST
格式:application/json

请求参数:

· user_id: 用户ID
· password: 密码
· state: 从授权初始化接口获取的state值

成功响应:

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

失败响应:

```json
{
  "success": false,
  "message": "错误信息"
}
```

## <a id="3-使用授权码获取访问令牌"></a>3. 使用授权码获取访问令牌

URL: /api/oauth/token
方法:POST
格式:application/json

请求参数:

· code: 授权码
· state: 从授权初始化接口获取的state值

成功响应:

```json
{
  "success": true,
  "message": "获取访问令牌成功",
  "access_token": "token_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
  "token_type": "Bearer",
  "expires_in": 3600
}
```

失败响应:

```json
{
  "success": false,
  "message": "错误信息"
}
```

## <a id="4-用户信息修改"></a>4. 用户信息修改

URL: /api/oauth/user/update
方法:POST
格式:application/json

请求参数:

· user_id: 用户ID（必填）
· state: 从授权初始化接口获取的state值（必填）
· cookie: 目标用户的cookie（必填）
· username: 新用户名（可选）
· bio: 个人简介（可选）

成功响应:

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

失败响应:

```json
{
  "success": false,
  "message": "错误信息"
}
```

## <a id="5-获取用户信息"></a>5. 获取用户信息

URL: /api/oauth/user/info
方法:POST
格式:application/json

请求参数:

· user_id: 用户ID（必填）

成功响应:

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

失败响应:

```json
{
  "success": false,
  "message": "错误信息"
}
```

## <a id="6-获取一次性确认码"></a>6. 获取一次性确认码

URL: /api/oauth/ucaoncecode
方法:POST
格式:application/json

请求参数:

· state: 从授权初始化接口获取的state值

成功响应:

```json
{
  "success": true,
  "message": "获取一次性确认码成功",
  "ucaoncecode": "uca_xxxxxxxxxxxxxxxx"
}
```

失败响应:

```json
{
  "success": false,
  "message": "错误信息"
}
```

## <a id="7-用户确认页面"></a>7. 用户确认页面

URL: /api/oauth/user_confirmation
方法:GET

请求参数:

· uca: 一次性确认码

## <a id="8-生成登录码"></a>8. 生成登录码

URL: /api/oauth/generate_login_code
方法:POST
格式:application/json

请求参数:

· uca: 一次性确认码

成功响应:

```json
{
  "success": true,
  "message": "生成登录码成功",
  "login_code": "abc123def4"
}
```

失败响应:

```json
{
  "success": false,
  "message": "错误信息"
}
```

## <a id="9-安全登录接口"></a>9. 安全登录接口

URL: /api/oauth/login_safe
方法:POST
格式:application/json

请求参数:

· state: 从授权初始化接口获取的state值
· login_code: 10位随机登录码

成功响应:

```json
{
  "success": true,
  "message": "登录成功，授权码已生成",
  "code": "code_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
  "user": {
    "id": 1,
    "username": "测试用户",
    "user_id": "testuser123",
    "email": "test@example.com"
  }
}
```

失败响应:

```json
{
  "success": false,
  "message": "错误信息"
}
```

错误码说明

· 400: 请求参数错误
· 401: 缺少API密钥或API密钥无效
· 403: 请求来源不匹配已注册的白名单IP
· 404: 请求的资源不存在
· 408: state值过期或无效
· 409: 授权码无效或已过期
· 500: 服务器内部错误

## <a id="错误码说明"></a>错误码说明

## <a id="使用示例"></a>使用示例

## <a id="使用python调用api"></a>使用Python调用API

## <a id="使用python调用oauth登录流程"></a>使用Python调用OAuth登录流程

```python
import requests
import json

# API基础URL
BASE_URL = "https://more.jsoftstudio.top"
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

## <a id="使用python调用oauth安全登录流程"></a>使用Python调用OAuth安全登录流程

```python
import requests
import json
import webbrowser
from getpass import getpass

# API基础URL
BASE_URL = "https://more.jsoftstudio.top"
# API密钥
API_KEY = "ml_your_api_key"

# 创建会话和请求头
session = requests.Session()
headers = {
    "X-API-Key": API_KEY,
    "Content-Type": "application/json"
}

def test_oauth_safe_login_flow():
    """测试OAuth安全登录流程"""
    print("开始OAuth安全登录流程测试...")
    print("="*60)
    
    try:
        # 1. 获取授权state
        print("✓ 步骤1: 获取授权state")
        auth_url = f"{BASE_URL}/api/oauth/authorize"
        print(f"  请求URL: {auth_url}")
        response = session.get(auth_url, headers=headers)
        print(f"  响应状态码: {response.status_code}")
        
        if response.status_code != 200:
            print(f"✗ 获取state失败: {response.text}")
            return None
        
        state_data = response.json()
        print(f"  响应内容: {json.dumps(state_data, ensure_ascii=False)}")
        state = state_data['state']
        print(f"✓ 获取state成功: {state}")
        print("-"*60)
        
        # 2. 获取一次性确认码
        print("✓ 步骤2: 获取一次性确认码")
        uca_url = f"{BASE_URL}/api/oauth/ucaoncecode"
        print(f"  请求URL: {uca_url}")
        payload = {"state": state}
        print(f"  请求数据: {json.dumps(payload, ensure_ascii=False)}")
        response = session.post(uca_url, headers=headers, json=payload)
        print(f"  响应状态码: {response.status_code}")
        
        if response.status_code != 200:
            print(f"✗ 获取确认码失败: {response.text}")
            return None
        
        uca_data = response.json()
        print(f"  响应内容: {json.dumps(uca_data, ensure_ascii=False)}")
        ucaoncecode = uca_data['ucaoncecode']
        print(f"✓ 获取确认码成功: {ucaoncecode}")
        print("-"*60)
        
        # 3. 打开用户确认页面
        print("✓ 步骤3: 打开用户确认页面")
        confirmation_url = f"{BASE_URL}/api/oauth/user_confirmation?uca={ucaoncecode}"
        print(f"  确认页面URL: {confirmation_url}")
        print("  正在打开浏览器...")
        webbrowser.open(confirmation_url)
        print("  请在浏览器中登录并确认授权")
        print("  确认后，浏览器会显示10位登录码")
        print("-"*60)
        
        # 4. 获取用户输入的登录码
        print("✓ 步骤4: 输入登录码")
        login_code = getpass("  请输入浏览器中显示的10位登录码: ").strip()
        if len(login_code) != 10:
            print(f"✗ 登录码格式不正确，应为10位")
            return None
        print(f"  已输入登录码: {login_code}")
        print("-"*60)
        
        # 5. 使用安全登录API
        print("✓ 步骤5: 使用安全登录API")
        login_safe_url = f"{BASE_URL}/api/oauth/login_safe"
        print(f"  请求URL: {login_safe_url}")
        payload = {
            "state": state,
            "login_code": login_code
        }
        print(f"  请求数据: {json.dumps(payload, ensure_ascii=False)}")
        response = session.post(login_safe_url, headers=headers, json=payload)
        print(f"  响应状态码: {response.status_code}")
        
        if response.status_code != 200:
            print(f"✗ 安全登录失败: {response.text}")
            return None
        
        login_data = response.json()
        print(f"  响应内容: {json.dumps(login_data, ensure_ascii=False)}")
        authorization_code = login_data['code']
        print(f"✓ 安全登录成功，获取授权码: {authorization_code}")
        print("-"*60)
        
        # 6. 使用授权码换取访问令牌
        print("✓ 步骤6: 使用授权码换取访问令牌")
        token_url = f"{BASE_URL}/api/oauth/token"
        print(f"  请求URL: {token_url}")
        payload = {
            "code": authorization_code,
            "state": state
        }
        print(f"  请求数据: {json.dumps(payload, ensure_ascii=False)}")
        response = session.post(token_url, headers=headers, json=payload)
        print(f"  响应状态码: {response.status_code}")
        
        if response.status_code != 200:
            print(f"✗ 获取访问令牌失败: {response.text}")
            return None
        
        token_data = response.json()
        print(f"  响应内容: {json.dumps(token_data, ensure_ascii=False)}")
        access_token = token_data['access_token']
        print(f"✓ 获取访问令牌成功: {access_token}")
        print("="*60)
        print("🎉 OAuth安全登录流程测试成功完成！")
        
        return {
            'access_token': access_token,
            'user_info': login_data.get('user', {})
        }
        
    except json.JSONDecodeError as e:
        print(f"✗ JSON解析错误: {str(e)}")
    except requests.exceptions.RequestException as e:
        print(f"✗ 网络请求错误: {str(e)}")
    except Exception as e:
        print(f"✗ 未知错误: {str(e)}")
    
    return None

# 执行测试
if __name__ == "__main__":
    result = test_oauth_safe_login_flow()
    if result:
        print("\n登录结果:")
        print(f"访问令牌: {result['access_token']}")
        print(f"用户信息: {json.dumps(result['user_info'], ensure_ascii=False)}")
    else:
        print("\n登录流程失败")
```

## <a id="使用python调用信息修改api以修改用户名为例"></a>使用Python调用信息修改API（以修改用户名为例）

```python
import requests
import json

# API基础URL
BASE_URL = "https://more.jsoftstudio.top"
# API密钥
API_KEY = "ml_your_api_key"

# 创建会话对象
session = requests.Session()
# 设置会话级别的请求头
session.headers.update({
    "X-API-Key": API_KEY,
    "Content-Type": "application/json"
})

def get_state():
    """获取授权state值"""
    print("获取授权state...")
    url = f"{BASE_URL}/api/oauth/authorize"
    try:
        response = session.get(url)
        data = response.json()
        if data.get('success'):
            return data.get('state')
        else:
            print(f"获取state失败: {data.get('message')}")
            return None
    except Exception as e:
        print(f"获取state时发生错误: {str(e)}")
        return None

def update_user_info():
    """修改用户信息"""
    print("==== 修改用户信息 ====")
    print(f"使用的API密钥: {API_KEY}")
    
    # 步骤1: 获取state值
    state = get_state()
    if not state:
        print("获取state失败，无法继续操作")
        return
    print(f"✓ 获取state成功: {state}")
    
    # 假设已经获取到用户的cookie
    # 注意：实际使用时，需要通过适当的方式获取用户cookie
    user_cookie = "your_user_cookie_here"  # 例如: "session=.eJ..."
    
    url = f"{BASE_URL}/api/oauth/user/update"
    payload = {
        "user_id": "testid",
        "username": "newusername",
        "state": state,  # 添加state参数
        "cookie": user_cookie  # 添加cookie参数
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

## <a id="使用python调用获取用户信息api"></a>使用Python调用获取用户信息API

```python
import requests
import json

# API基础URL
BASE_URL = "https://more.jsoftstudio.top"
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

## <a id="使用javascript调用api"></a>使用JavaScript调用API

## <a id="使用javascript调用oauth登录流程"></a>使用JavaScript调用OAuth登录流程

```javascript
// API基础URL
const BASE_URL = "https://more.jsoftstudio.top";
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
 * 获取授权state值
 */
async function getState() {
    console.log("获取授权state...");
    const url = `${BASE_URL}/api/oauth/authorize`;
    try {
        const response = await fetch(url, {
            method: 'GET',
            headers: headers
        });
        const data = await response.json();
        if (data.success) {
            return data.state;
        } else {
            console.log(`获取state失败: ${data.message || '未知错误'}`);
            return null;
        }
    } catch (error) {
        console.log(`获取state时发生错误: ${error.message}`);
        return null;
    }
}
```

## <a id="使用javascript调用oauth安全登录流程"></a>使用JavaScript调用OAuth安全登录流程

```javascript
/**
 * 使用JavaScript调用OAuth安全登录流程
 */
async function testOAuthSafeLoginFlow() {
    console.log("==== OAuth 2.0 安全登录流程 ====");
    console.log(`使用的API密钥: ${API_KEY}`);
    console.log("=".repeat(60));
    
    try {
        // 步骤1: 获取授权state
        console.log("\n✓ 步骤1: 获取授权state");
        const state = await getState();
        if (!state) {
            console.log("✗ 获取state失败，无法继续操作");
            return;
        }
        console.log(`✓ 获取state成功: ${state}`);
        console.log("-".repeat(60));
        
        // 步骤2: 获取一次性确认码
        console.log("\n✓ 步骤2: 获取一次性确认码");
        const ucaUrl = `${BASE_URL}/api/oauth/ucaoncecode`;
        console.log(`正在发送请求到: ${ucaUrl}`);
        console.log(`请求数据:`, JSON.stringify({state: state}, null, 2));
        
        const ucaResponse = await fetch(ucaUrl, {
            method: 'POST',
            headers: headers,
            body: JSON.stringify({state: state})
        });
        
        console.log(`\nHTTP状态码: ${ucaResponse.status}`);
        const ucaText = await ucaResponse.text();
        console.log(`响应内容: ${ucaText}`);
        
        try {
            const ucaData = JSON.parse(ucaText);
            console.log(`\n解析后的响应:`, JSON.stringify(ucaData, null, 2));
            
            if (ucaData.success) {
                const ucaoncecode = ucaData.ucaoncecode;
                console.log(`✓ 获取确认码成功: ${ucaoncecode}`);
                console.log("-".repeat(60));
                
                // 步骤3: 打开用户确认页面
                console.log("\n✓ 步骤3: 打开用户确认页面");
                const confirmationUrl = `${BASE_URL}/api/oauth/user_confirmation?uca=${ucaoncecode}`;
                console.log(`确认页面URL: ${confirmationUrl}`);
                
                // 打开新窗口
                if (typeof window !== 'undefined') {
                    window.open(confirmationUrl, '_blank');
                } else {
                    console.log('在Node.js环境中，请手动打开上述URL');
                }
                console.log("请在浏览器中登录并确认授权");
                console.log("确认后，浏览器会显示10位登录码");
                console.log("-".repeat(60));
                
                // 步骤4: 获取用户输入的登录码
                console.log("\n✓ 步骤4: 输入登录码");
                let loginCode;
                
                // 浏览器环境
                if (typeof window !== 'undefined') {
                    loginCode = prompt("请输入浏览器中显示的10位登录码: ");
                } else {
                    // Node.js环境中需要使用其他方式获取输入
                    loginCode = "1234567890"; // 示例值
                }
                
                if (!loginCode || loginCode.length !== 10) {
                    console.log("✗ 登录码格式不正确，应为10位");
                    return;
                }
                console.log(`✓ 已输入登录码: ${loginCode}`);
                console.log("-".repeat(60));
                
                // 步骤5: 使用安全登录API
                console.log("\n✓ 步骤5: 使用安全登录API");
                const loginSafeUrl = `${BASE_URL}/api/oauth/login_safe`;
                const payload = {
                    "state": state,
                    "login_code": loginCode
                };
                console.log(`正在发送请求到: ${loginSafeUrl}`);
                console.log(`请求数据:`, JSON.stringify(payload, null, 2));
                
                const loginResponse = await fetch(loginSafeUrl, {
                    method: 'POST',
                    headers: headers,
                    body: JSON.stringify(payload)
                });
                
                console.log(`\nHTTP状态码: ${loginResponse.status}`);
                const loginText = await loginResponse.text();
                console.log(`响应内容: ${loginText}`);
                
                try {
                    const loginData = JSON.parse(loginText);
                    console.log(`\n解析后的响应:`, JSON.stringify(loginData, null, 2));
                    
                    if (loginData.success) {
                        const code = loginData.code;
                        console.log(`✓ 安全登录成功，获取授权码: ${code}`);
                        console.log("-".repeat(60));
                        
                        // 步骤6: 使用授权码获取访问令牌
                        console.log("\n✓ 步骤6: 使用授权码获取访问令牌");
                        const tokenUrl = `${BASE_URL}/api/oauth/token`;
                        const tokenPayload = {
                            "code": code,
                            "state": state
                        };
                        console.log(`正在发送请求到: ${tokenUrl}`);
                        console.log(`请求数据:`, JSON.stringify(tokenPayload, null, 2));
                        
                        const tokenResponse = await fetch(tokenUrl, {
                            method: 'POST',
                            headers: headers,
                            body: JSON.stringify(tokenPayload)
                        });
                        
                        console.log(`\nHTTP状态码: ${tokenResponse.status}`);
                        const tokenText = await tokenResponse.text();
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
                                console.log("=".repeat(60));
                                console.log("🎉 OAuth安全登录流程测试成功完成！");
                            } else {
                                console.log(`✗ 获取访问令牌失败: ${tokenData.message || '未知错误'}`);
                            }
                        } catch (jsonError) {
                            console.log("\n无法解析访问令牌响应的JSON");
                            console.error(jsonError);
                        }
                    } else {
                        console.log(`✗ 安全登录失败: ${loginData.message || '未知错误'}`);
                    }
                } catch (jsonError) {
                    console.log("\n无法解析登录响应的JSON");
                    console.error(jsonError);
                }
            } else {
                console.log(`✗ 获取确认码失败: ${ucaData.message || '未知错误'}`);
            }
        } catch (jsonError) {
            console.log("\n无法解析确认码响应的JSON");
            console.error(jsonError);
        }
    } catch (error) {
        console.log(`✗ 发生错误: ${error.message}`);
        console.error(error);
    }
}
```

## <a id="使用javascript调用信息修改api以修改用户名为例"></a>使用JavaScript调用信息修改API（以修改用户名为例）

```javascript
/**
 * 修改用户信息
 */
async function updateUserInfo() {
    console.log("==== 修改用户信息 ====");
    console.log(`使用的API密钥: ${API_KEY}`);
    
    // 步骤1: 获取state值
    const state = await getState();
    if (!state) {
        console.log("获取state失败，无法继续操作");
        return;
    }
    console.log(`✓ 获取state成功: ${state}`);
    
    // 假设已经获取到用户的cookie
    // 注意：实际使用时，需要通过适当的方式获取用户cookie
    const userCookie = "your_user_cookie_here";  // 例如: "session=.eJ..."
    
    const url = `${BASE_URL}/api/oauth/user/update`;
    const payload = {
        "user_id": "testid",
        "username": "newusername",
        "state": state,  // 添加state参数
        "cookie": userCookie  // 添加cookie参数
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
```

## <a id="使用javascript调用获取用户信息api"></a>使用JavaScript调用获取用户信息API

```javascript
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
console.log("==== OAuth 流程测试 ====");

// 如果需要运行其他功能，取消注释下面的代码
// testOAuthSafeLoginFlow();
// updateUserInfo();
// getUserInfo();
```

## <a id="注意事项"></a>注意事项

1. 请确保API密钥的安全性，不要在前端代码中暴露
2. 所有API调用必须使用HTTPS协议
3. 请合理处理API响应，特别是错误情况
4. state参数用于防止CSRF攻击，请确保正确传递和验证
5. 授权码有效期为10分钟，访问令牌有效期为1小时
6. 完成授权后，建议将state参数存储在安全的位置，直到整个授权流程完成
