# 1. 概述

如果百度用户访问第三方应用网页,则第三方应用可以通过网页授权机制,来获取百度用户基 本信息,进而实现自身业务功能。 

具体而言,百度帐号网页授权流程分为四步: 

1. 引导用户进入授权页面同意授权,获取 code; 

2. 通过 code 换取网页授权 access\_token; 

3. 如果需要,开发者可以刷新网页授权 access\_token,避免过期; 

# 2. 引导用户完成授权获取code

开发时,需要将用户浏览器重定向到如下 URL 地址。 

接口调用请求说明 

```
GET https://openapi.baidu.com/oauth/2.0/authorize?response_type=CODE&client_id=API_KEY&redirect_uri=REDIRECT_URI&scope=SCOPE&state=STATE
```

参数说明

| 参数名 | 类型 | 是否必须 | 描述 |
| :--- | :--- | :--- | :--- |
| response\_type | string  | 是  | 固定为 code。 |
| client\_id | string | 是 | 注册应用时获得的API Key。 |
|  | string | 是 | 授权后要回调的URI，即接收Authorization Code的URI。如果用户在授权过程中取消授权，会回调该URI，并在URI末尾附上error=access\_denied参数。对于无Web Server的应用，其值可以是“oob”，此时用户同意授权后，授权服务会将Authorization Code直接显示在响应页面的页面中及页面title中。非“oob”值的redirect\_uri按照如下规则进行匹配：（1）如果开发者在“授权安全设置”中配置了“授权回调地址”，则redirect\_uri必须与“授权回调地址”中的某一个相匹配； （2）如果未配置“授权回调地址”，redirect\_uri所在域名必须与开发者注册应用时所提供的网站根域名列表或应用的站点地址（如果根域名列表没填写）的域名相匹配。授权回调地址配置请参考附录5.1。 |
| scope | string | 否 | 以空格分隔的权限列表，若不传递此参数，代表请求用户的默认权限。可填basic或mobile。 |
| display | string | 否 | 登录和授权页面的展现样式，默认为“page”，具体参数定义请参考附录5.2。 |
| state | string | 否 | 重定向后会带上state参数。建议开发者利用state参数来防止CSRF攻击。 |
| force\_login | int | 否 | 如传递“force\_login=1”，则加载登录页时强制用户输入用户名和口令，不会从cookie中读取百度用户的登陆状态。 |
| confirm\_login | int | 否 | 如传递“confirm\_login=1”且百度用户已处于登陆状态，会提示是否使用已当前登陆用户对应用授权。 |
| login\_type | string | 否 | 如传递“login\_type=sms”，授权页面会默认使用短信动态密码注册登陆方式。 |

下图为登录授权页面: 

无 scope 权限或 redirect\_uri 不合法时,会展示错误页面,并提示出错原因,如下图示:   

用户同意授权后:页面将跳转至 redirect\_uri/?code=CODE&state=STATE。 

code 说明:code 作为换取 access\_token 的票据,每次用户授权带上的 code 将不一样,code 只 能使用一次,10 分钟未被使用自动过期。

# 3. 获取网页授权access\_token

redirect\_uri 指定的开发者服务器地址,在获取到授权 code 参数后,从服务端向百度开放平台发起

如下 HTTP 请求,通过 code 换取网页授权 access\_token。 

注意:access\_token 长度保留 256 字符。 

接口调用请求说明 

```
GET   https://openapi.baidu.com/oauth/2.0/token?grant_type=authorization_code&code=CODE&client_id=AP I_KEY&client_secret=SECRET_KEY&redirect_uri=REDIRECT_URI    
```

参数说明

| 参数名  | 类型  | 是否必 须  | 描述  |
| :--- | :--- | :--- | :--- |
| grant\_type  | string  | 是  | 固定为 authorization\_code  |
| code  | string  | 是  | 用户授权后得到 code  |
| client\_id  | string  | 是  | 应用的 API  Key |
| client\_secret  | string  | 是  | 应用的 Secret  Key |
| redirect\_uri  | string  | 是  | 该值必须与获取 Authorization  Code 时传递的“redirect\_uri”保 持一致。 |

返回值说明

| 字段名  | 类型  | 描述  |
| :--- | :--- | :--- |
| access\_token  | string  | 获取到的网页授权接口调用凭证  |
| expires\_in  | int  | Access  Token 的有效期,以秒为单位 |
| refresh\_token  | string  | 用于刷新 Access   Token   的   Refresh   Token,所有应用都会返回该参数;\(1 0 年 的 有 效期\) |
| scope  | string  | Access  Token 最终的访问范围,即用户实际授予的权限列表\(用户在授权页面时,有 可能会取消掉某些请求的权限\) |
| session\_key  | string  | 基于 http 调用 Open  API 时所需要的 Session  Key,其有效期与 Access  Token 一致 |
| session\_secret | string  | 基于 http 调用 Open  API 时计算参数签名用的签名密钥。 |

错误情况下

Microsoft Word - 百度帐号接入指南V1.1.docx

| 字段名  | 类型  | 描述  |
| :--- | :--- | :--- |
| error  | string  | 错误码;关于错误码的详细信息请参考附录 5.3 |
| error\_description  | string  | 错误描述信息,用来帮助理解和解决发生的错误  |

返回值示例

```
{  
     "access_token":  "1.a6b7dbd428f731035f771b8d15063f61.86400.1292922000-2346678-124328",  
     "expires_in":  86400,  
     "refresh_token":  "2.385d55f8615fdfd9edb7c4b5ebdc3e39.604800.1293440400-2346678-124328",               
     "scope":  "basic  email",  
     "session_key":  "ANXxSNjwQDugf8615OnqeikRMu2bKaXCdlLxn",  
     "session_secret":  "248APxvxjCZ0VEC43EYrvxqaK4oZExMB"  
} 
```

出错时返回

```
{  
     "error":  "invalid_grant",  
     "error_description":  "Invalid  authorization  code:  ANXxSNjwQDugOnqeikRMu2bKaXCdlLxn"   
} 
```

# 4. 按需刷新access\_token

当 access\_token 过期后,可以使用 refresh\_token 进行刷新。refresh\_token 有效期为十年。 

接口调用请求说明 

```
GET  
https://openapi.baidu.com/oauth/2.0/token?grant_type=refresh_token&refresh_token=REFRESH_TOKEN &client_id=API_KEY&client_secret=SECRET_KEY 
```

参数说明

| **参数名** | **类型** | **是否必须** | **描述** |
| :--- | :--- | :--- | :--- |
| grant\_type | string | 是 | 固定为refresh\_token |
| refresh\_token | string | 是 | 通过access\_token获取到的refresh\_token参数 |
| client\_id | string | 是 | 应用的API Key |
| client\_secret | string | 是 | 应用的Secret Key |

返回值说明

| 字段名  | 类型  | 描述  |
| :--- | :--- | :--- |
| access\_token  | string  | 获取到的网页授权接口调用凭证  |
| expires\_in  | int  | Access  Token 的有效期,以秒为单位 |
| refresh\_token  | string  | 用于刷新 Access   Token   的   Refresh   Token,所有应用都会返回该参数;\(1 0 年 的 有 效期\) |
| scope  | string  | Access  Token 最终的访问范围,即用户实际授予的权限列表\(用户在授权页面时,有 可能会取消掉某些请求的权限\) |
| session\_key  | string  | 基于 http 调用 Open  API 时所需要的 Session  Key,其有效期与 Access  Token 一致 |
| session\_secret  | string  | 基于 http 调用 Open  API 时计算参数签名用的签名密钥。 |

错误情况下 

| 字段名  | 类型  | 描述  |
| :--- | :--- | :--- |
| error  | string  | 错误码;关于错误码的详细信息请参考附录 5.3 |
| error\_description  | string  | 错误描述信息,用来帮助理解和解决发生的错误  |

返回值示例

```
{  
     "access_token":  "1.a6b7dbd428f731035f771b8d15063f61.86400.1292922000-2346678-124328",               
     "expires_in":  86400,  
     "refresh_token":  "2.af3d55f8615fdfd9edb7c4b5ebdc3e32.604800.1293440400-2346678-124328",               
     "scope":  "basic  email",  
     "session_key":  "ANXxSNjwQDugf8615OnqeikRMu2bKaXCdlLxn",  
     "session_secret":  "248APxvxjCZ0VEC43EYrvxqaK4oZExMB"  
} 
```

出错时返回

```
{
     "error": "expired_token",
     "error_description": "refresh token has been used"
}
```

# 5. 获取授权用户信息

获取 access\_token 之后,开发者可以通过 access\_token 拉取用户信息。 

接口调用请求说明 

```
GET  https://openapi.baidu.com/rest/2.0/passport/users/getInfo?access_token=access_token 
```

参数说明

| 参数名  | 类型  | 是否必须  | 描述  |
| :--- | :--- | :--- | :--- |
| access\_token  | string  | 是  | 由上述步骤获取的 openapi 接口调用凭证  |

返回参数

| **参数名称** | **参数类型** | **是否必需** | **示例值** | **描述** |
| :--- | :--- | :--- | :--- | :--- |
| userid | uint | 是 | 67411167 | 当前登录用户的数字ID |
| securemobile | uint | 否 | 188888888 | 当前用户绑定手机号（需要向开放平台申请权限） |
| username | string | 否 | testname | 当前登录用户的用户名，值可能为空**。** |
| realname | string | 否 | 阳光 | 用户真实姓名，可能为空。 |
| portrait | string | 否 | e2c1776c31393837313031319605 | 当前登录用户的头像 |
| userdetail | string | 否 | 喜欢自由 | 自我简介，可能为空。 |
| birthday | string | 否 | 1987-01-010000-00-00为未知 | 生日，以yyyy-mm-dd格式显示。 |
| marriage | string | 否 | 0:未知,1:单身,2:已婚3:恋爱4:离异 | 婚姻状况 |
| sex | string | 否 | 0:未知,1:男,2:女 | 性别 |
| blood | string | 否 | 0:未知,1:A,2:B,3:O,4:AB,5:其他 | 血型 |
| figure | string | 否 | 0:未知,1:苗条,2:丰满,3:中等身材,4:高大,5:小巧,6:运动型,7:健美 | 体型 |
| constellation | string | 否 | 0:未知,1:水瓶,2:双鱼,3:白羊,4:金牛,5:双子,6:巨蟹,7:狮子,8:处女,9:天秤,10:天蝎,11:射手,12:魔羯 | 星座 |
| education | string | 否 | 0:未知,1:初中,2:高中,3:大学,4:硕士,5:小学,6:中专/技校,7:大专,8:博士,9其他 | 学历 |
| trade | string | 否 | 0:未知,1:广告/营销/公关,2:航天,3:农业/化工/林业产品,4:汽车,5:计算机/电子产品,6:建筑,7:教育\(包括学生\),8:能源/采矿,9:金融/保险/房地产,10:政府/军事/公共服务,11:招待,12:传媒/出版/娱乐,13:医疗/保健服务,14:制药,15:零售,16:服务,17:电信/网络,18:旅游/交通,19:其他 | 当前职业 |
| job | string | 否 | 0:未知,1:学生,2:普通职员,3:工程师,4:总经理、董事长、CXO,5:市场部经理,6:销售部经理,7:行政主管,8:人事主管,9:财务主管,10:技术主管,11:退休,12:其他 | 职位 |
| is\_bind\_mobile | uint | 否 | 0:未绑定,1:已绑定 | 是否绑定手机号 |
| is\_realname | uint | 否 | 0：未实名制，1：已实名制 | 是否实名制 |

错误情况下

| 字段名  | 类型  | 描述  |
| :--- | :--- | :--- |
| error\_code  | int  | 错误码  |
| error\_msg  | string  | 错误描述信息,用来帮助理解和解决发生的错误  |

关于错误码的详细信息请参考附录 5.4 

返回值示例 

```
{                                                                                        
     "userid":  "2097322476",  
     "username":  "wl19871011",         
     "realname":  "阳光",  
     "userdetail":  "喜欢自由",  
     "birthday":  "1987-01-01",  
     "marriage":  "0",  
     "sex":  "1",  
     "blood":  "3",  
     "constellation":  "2",  
     "figure":  "2",  
     "education":  "0",  
     "trade":  "0",  
     "job":  "0"  
 }
```

出错时返回

```
{  
     "error_code":  "100",  
     "error_msg":  "Invalid  parameter"   
} 
```



