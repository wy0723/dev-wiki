\[TOC\]

# 1. 授权回调地址

为确保验证授权过程的安全,开发者必须在开发者中心预先注册应用所在的域名或 URL,用以 OAuth2.0 检验授权请求中的“redirect\_uri”参数。以便保证 OAuth2.0 在回调过程中,会回调到 安全域名。 

请进入如下页面对授权回调地址进行配置:进入开发者中心应用管理页,点击需要配置回调地 址的工程,进入工程基本信息页,即可在左侧导航找到“安全设置”的入口。   \(授权回调页请填写 完整 url，例如:[http://www.example.com/oauth\_redirect](http://www.example.com/oauth_redirect%29。) ）。![](/assets/anquan.png)

# 2. display参数说明

请求用户授权时百度提供了一个在 OAuth2.0 协议中没有提到的参数:display。它是用来标识 不同形式的客户端所对应的不同展现形式的授权页面,其值定义如下: 

* page:全屏形式的授权页面\(默认\),适用于web应用。 
* popup: 弹框形式的授权页面,适用于桌面软件应用和web应用。 
* dialog:浮层形式的授权页面,只能用于站内web应用。 
* mobile:IPhone/Android等智能移动终端上用的授权页面,适用于IPhone/Android等智能移动终端上的应用。 
* pad: IPad/Android等平板上使用的授权页面,适用于IPad/Android等智能移动终端上的应用。 
* tv:电视等超大显示屏使用的授权页面。 

# 3. OAuth2.0错误码列表

| **错误码** | **错误信息** | **详细描述** |
| :--- | :--- | :--- |
| invalid\_request | invalid refresh token | 请求缺少某个必需参数，包含一个不支持的参数或参数值，或者格式不正确。 |
| invalid\_client | unknown client id | “client\_id”、“client\_secret”参数无效。 |
| invalid\_grant | The provided authorization grant is revoked | 提供的Access Grant是无效的、过期的或已撤销的，例如，Authorization Code无效\(一个授权码只能使用一次\)、Refresh Token无效、redirect\_uri与获取Authorization Code时提供的不一致、Devie Code无效\(一个设备授权码只能使用一次\)等。 |
| unauthorized\_client | The client is not authorized to use this authorization grant type | 应用没有被授权，无法使用所指定的grant\_type。 |
| unsupported\_grant\_type | The authorization grant type is not supported | “grant\_type”百度OAuth2.0服务不支持该参数。 |
| invalid\_scope | The requested scope is exceeds the scope granted by the resource owner | 请求的“scope”参数是无效的、未知的、格式不正确的、或所请求的权限范围超过了数据拥有者所授予的权限范围。 |
| expired\_token | refresh token has been used | 提供的Refresh Token已过期 |
| redirect\_uri\_mismatch | Invalid redirect uri | “redirect\_uri”所在的根域与开发者注册应用时所填写的根域名不匹配。 |
| unsupported\_response\_type | The response type is not supported | “response\_type”参数值不为百度OAuth2.0服务所支持，或者应用已经主动禁用了对应的授权模式 |
| slow\_down | The device is polling too frequently | Device Flow中，设备通过Device Code换取Access Token的接口过于频繁，两次尝试的间隔应大于5秒。 |
| authorization\_pending | User has not yet completed the authorization | Device Flow中，用户还没有对Device Code完成授权操作。 |
| authorization\_declined | User has declined the authorization | Device Flow中，用户拒绝了对Device Code的授权操作。 |
| invalid\_referer | Invalid Referer | Implicit Grant模式中，浏览器请求的Referer与根域名绑定不匹配 |

# 4. OpenAPI错误码列表

| **错误码** | **错误信息** | **详细描述** |
| :--- | :--- | :--- |
| 1 | Unknown error | 未知错误，如果频繁发生此错误，请联系developer\_support@baidu.com |
| 2 | Service temporarily unavailable | 服务暂时不可用 |
| 3 | Unsupported openapi method | 访问URL错误，该接口不能访问 |
| 4 | Open api request limit reached | 该APP访问该接口的QPS达到上限 |
| 5 | Unauthorized client IP address | 访问的客户端IP不在白名单内 |
| 6 | No permission to access data | 该APP没有访问该接口的权限 |
| 17 | Open api daily request limit reached | 该APP访问该接口超过每天的访问限额 |
| 18 | Open api qps request limit reached | 该APP访问该接口超过QPS限额 |
| 19 | Open api total request limit reached | 该APP访问该接口超过总量限额 |
| 100 | Invalid parameter | 没有获取到token参数 |
| 110 | Access token invalid or no longer valid | token不合法 |
| 111 | Access token expired | token已过期 |
| 213 | No permission to access user mobile | 没有权限获取用户手机号 |



