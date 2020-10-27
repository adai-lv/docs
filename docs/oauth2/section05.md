# 访问令牌

## 颁发访问令牌

如果访问令牌请求是有效的且被授权，授权服务器如[颁发访问令牌-成功的响应][颁发访问令牌-成功的响应]节所述颁发访问令牌以及可选的刷新令牌。如果请求因客户端身份验证失败或无效，授权服务器如[颁发访问令牌-错误响应][颁发访问令牌-错误响应]节所述的返回错误响应。

#### 成功的响应

授权服务器颁发访问令牌和可选的刷新令牌，通过向HTTP响应实体正文中添加下列参数并使用200（OK）状态码构造响应：

- access_token    
  必需的。授权服务器颁发的访问令牌。

- token_type    
  必需的。如[访问令牌类型][访问令牌类型]节所述的颁发的令牌的类型。值是大小写不敏感的。

- expires_in    
  推荐的。以秒为单位的访问令牌生命周期。例如，值 `3600` 表示访问令牌将在从生成响应时的1小时后到期。如果省略，则授权服务器应该通过其他方式提供过期时间，或者记录默认值。

- refresh_token    
  可选的。刷新令牌，可以用于如下[刷新访问令牌](#刷新访问令牌)节所述使用相同的授权许可获得新的访问令牌。

- scope    
  可选的，若与客户端请求的范围相同；否则，必需的。如[访问令牌范围][访问令牌范围]节所述的访问令牌的范围。
  
这些参数使用[RFC4627][RFC4627]定义的 `application/json` 媒体类型包含在HTTP响应实体正文中。通过将每个参数添加到最高结构级别， 参数被序列化为JavaScript对象表示法（JSON）的结构。参数名称和字符串值作为JSON字符串类型包含。数值的值作为JSON数字类型包含。参数顺序无关并可以变化。

在任何包含令牌、凭据或其他敏感信息的响应中，授权服务器必须在其中包含值为“no-store”的HTTP“Cache-Control”响应头部域[RFC2616][RFC2616]，和值为“no-cache”的“Pragma”响应头部域[RFC2616][RFC2616]。例如：

```
HTTP/1.1 200 OK

Content-Type: application/json;charset=UTF-8
Cache-Control: no-store
Pragma: no-cache

{
  "access_token":"2YotnFZFEjr1zCsicMWpAA",
  "token_type":"example",
  "expires_in":3600,
  "refresh_token":"tGzv3JOkF0XG5Qx2TlKWIA",
  "example_parameter":"example_value"
}
```

客户端必须忽略响应中不能识别的值的名称。令牌和从授权服务器接收到的值的大小未定义。客户端应该避免对值的大小做假设。授权服务器应记录其发放的任何值的大小。

#### 错误响应

授权服务器使用HTTP 400（错误请求）状态码响应，在响应中包含下列参数：

- error
  必需的。下列ASCII[USASCII]错误代码之一：

  - invalid_request    
    请求缺少必需的参数、包含不支持的参数值（除了许可类型）、重复参数、包含多个凭据、采用超过一种客户端身份验证机制或其他不规范的格式。

  - invalid_client    
    客户端身份验证失败（例如，未知的客户端，不包含客户端身份验证，或不支持的身份验证方法）。授权服务器可以返回HTTP 401（未授权）状态码来指出支持的HTTP身份验证方案。如果客户端试图通过 `Authorization` 请求标头域进行身份验证，授权服务器必须响应HTTP 401（未授权）状态码，并包含与客户端使用的身份验证方案匹配的“WWW-Authenticate”响应标头字段。

  - invalid_grant    
    提供的授权许可（如授权码、资源所有者凭据）或刷新令牌无效、过期、吊销、与在授权请求使用的重定向URI不匹配或颁发给另一个客户端。

  - unauthorized_client    
    进行身份验证的客户端没有被授权使用这种授权许可类型。

  - unsupported_grant_type    
    授权许可类型不被授权服务器支持。

  - invalid_scope    
    请求的范围无效、未知的、格式不正确或超出资源所有者许可的范围。

- error_description    
  可选的。提供额外信息的人类可读的ASCII[USASCII]文本，用于协助客户端开发人员理解所发生的错误。

- error_uri    
  可选的。指向带有有关错误的信息的人类可读网页的URI，用于提供客户端开发人员关于该错误的额外信息。

> error、error_description、error_uri参数的值不能包含集合％x20-21 /％x23-5B /％x5D-7E以外的字符。
  
这些参数使用[RFC4627][RFC4627]定义的 `application/json` 媒体类型包含在HTTP响应实体正文中。通过将每个参数添加到最高结构级别，参数被序列化为JavaScript对象表示法（JSON）的结构。参数名称和字符串值作为JSON字符串类型包含。数值的值作为JSON数字类型包含。参数顺序无关并可以变化。例如：

```
HTTP/1.1 400 Bad Request

Content-Type: application/json;charset=UTF-8
Cache-Control: no-store
Pragma: no-cache

{
  "error":"invalid_request"
}
```

## 刷新访问令牌

若授权服务器给客户端颁发了刷新令牌，客户端通过使用按[附录B][附录B] `application/x-www-form-urlencoded` 格式在HTTP请求实体正文中发送下列 `UTF-8` 字符编码的参数向令牌端点发起刷新请求：
- grant_type    
  必需的。值必须设置为 `refresh_token`。

- refresh_token    
  必需的。颁发给客户端的刷新令牌。

- scope    
  可选的。如[访问令牌范围][访问令牌范围]节所述的访问请求的范围。请求的范围不能包含任何不是由资源所有者原始许可的范围，若省略，被视为与资源所有者原始许可的范围相同。
  
因为刷新令牌通常是用于请求额外的访问令牌的持久凭证，刷新令牌绑定到被它被颁发给的客户端。如果客户端类型是机密的或客户端被颁发了客户端凭据（或选定的其他身份验证要求），客户端必须如[客户端身份验证][客户端身份验证]节所述与授权服务器进行身份验证。

例如，客户端使用传输层安全发起如下HTTP请求：

```
POST /token HTTP/1.1
Host: server.example.com

Authorization: Basic czZCaGRSa3F0MzpnWDFmQmF0M2JW
Content-Type: application/x-www-form-urlencoded

grant_type=refresh_token&refresh_token=tGzv3JOkF0XG5Qx2TlKWIA
```

授权服务器必须：
- 要求机密客户端或任何被颁发了客户端凭据（或有其他身份验证要求）的客户端进行客户端身份验证，
- 若包括了客户端身份验证，验证客户端身份并确保刷新令牌是被颁发给进行身份验证的客户端的，并
- 验证刷新令牌。

如果有效且被授权，授权服务器如[颁发访问令牌-成功的响应][颁发访问令牌-成功的响应]节所述颁发访问令牌。如果请求因验证失败或无效，授权服务器[颁发访问令牌-错误响应][颁发访问令牌-错误响应]节所述返回错误响应。

授权服务器可以颁发新的刷新令牌，在这种情况下，客户端必须放弃旧的刷新令牌，替换为新的刷新令牌。在向客户端颁发新的刷新令牌后授权服务器可以撤销旧的刷新令牌。若颁发了新的刷新令牌，刷新令牌的范围必须与客户端包含在请求中的刷新令牌的范围相同。

[附录B]: oauth2/section12 "附录B"
[访问令牌范围]: oauth2/section03#访问令牌范围 "访问令牌范围"
[客户端身份验证]: oauth2/section03#客户端身份验证 "客户端身份验证"
[颁发访问令牌-成功的响应]: oauth2/section05#颁发访问令牌-成功的响应 "颁发访问令牌-成功的响应"
[颁发访问令牌-错误响应]: oauth2/section05#错误响应 "颁发访问令牌-错误响应"
[访问令牌类型]: oauth2/section06#访问令牌类型 "访问令牌类型"
[RFC4627]:http://tools.ietf.org/html/rfc4627 "JSON"
[RFC2616]:http://tools.ietf.org/html/rfc2616 "HTTP/1/1"