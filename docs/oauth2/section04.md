# 获得授权

为了请求访问令牌，客户端从资源所有者获得授权。授权表现为授权许可的形式，客户端用它请求访问令牌。OAuth定义了四种许可类型：授权码、隐式许可、资源所有者密码凭据和客户端凭据。它还提供了扩展机制定义其他许可类型。

## 授权码许可

授权码许可类型用于获得访问令牌和刷新令牌并且为受信任的客户端进行了优化。由于这是一个基于重定向的流程，客户端必须能够与资源所有者的用户代理（通常是Web浏览器）进行交互并能够接收来自授权服务器的传入请求（通过重定向）。

     +----------+
     | Resource |
     |   Owner  |
     |          |
     +----------+
          ^
          |
         (B)
     +----|-----+          Client Identifier      +---------------+
     |         -+----(A)-- & Redirection URI ---->|               |
     |  User-   |                                 | Authorization |
     |  Agent  -+----(B)-- User authenticates --->|     Server    |
     |          |                                 |               |
     |         -+----(C)-- Authorization Code ---<|               |
     +-|----|---+                                 +---------------+
       |    |                                         ^      v
      (A)  (C)                                        |      |
       |    |                                         |      |
       ^    v                                         |      |
     +---------+                                      |      |
     |         |>---(D)-- Authorization Code ---------'      |
     |  Client |          & Redirection URI                  |
     |         |                                             |
     |         |<---(E)----- Access Token -------------------'
     +---------+       (w/ Optional Refresh Token)
注：说明步骤（A）、（B）和（C）的直线因为通过用户代理而被分为两部分。    
图3：授权码流程

在图3中所示的流程包括以下步骤：
- （A）客户端通过向授权端点引导资源所有者的用户代理开始流程。客户端包括它的客户端标识、请求范围、本地状态和重定向URI，一旦访问被许可（或拒绝）授权服务器将传送用户代理回到该URI。
- （B）授权服务器验证资源拥有者的身份（通过用户代理），并确定资源所有者是否授予或拒绝客户端的访问请求。
- （C）假设资源所有者许可访问，授权服务器使用之前（在请求时或客户端注册时）提供的重定向URI重定向用户代理回到客户端。重定向URI包括授权码和之前客户端提供的任何本地状态。
- （D）客户端通过包含上一步中收到的授权码从授权服务器的令牌端点请求访问令牌。当发起请求时，客户端与授权服务器进行身份验证。客户端包含用于获得授权码的重定向URI来用于验证。
- （E）授权服务器对客户端进行身份验证，验证授权代码，并确保接收的重定向URI与在步骤（C）中用于重定向（资源所有者的用户代理）到客户端的URI相匹配。如果通过，授权服务器响应返回访问令牌与可选的刷新令牌。

#### 授权请求

客户端通过按[附录B](../AppendixB/b.md)使用“application/x-www-form-urlencoded”格式向授权端点URI的查询部分添加下列参数构造请求URI：
- response_type    
  必需的。值必须被设置为“code”。
- client_id    
  必需的。如[2.2](../Section02/2.2.md)节所述的客户端标识。
- redirect_uri    
  可选的。如[3.1.2](../Section03/3.1.2.md)节所述。
- scope    
  可选的。如3.3节所述的访问请求的范围。
- state    
  推荐的。客户度用于维护请求和回调之间的状态的不透明的值。当重定向用户代理回到客户端时，授权服务器包含此值。该参数应该用于防止如[10.12](../Section10/10.12.md)所述的跨站点请求伪造。
  
客户端使用HTTP重定向响应向构造的URI定向资源所有者，或者通过经由用户代理至该URI的其他可用方法。
例如，客户端使用TLS定向用户代理发起下述HTTP请求（额外的换行仅用于显示目的）：

    GET /authorize?response_type=code&client_id=s6BhdRkqt3&state=xyz&redirect_uri=https%3A%2F%2Fclient%2Eexample%2Ecom%2Fcb HTTP/1.1
    Host: server.example.com
  
授权服务器验证该请求，确保所有需要的参数已提交且有效。如果请求是有效的，授权服务器对资源所有者进行身份验证并获得授权决定（通过询问资源所有者或通过经由其他方式确定批准）。

当确定决定后，授权服务器使用HTTP重定向响应向提供的客户端重定向URI定向用户代理，或者通过经由用户代理至该URI的其他可行方法。

#### 授权响应

如果资源所有者许可访问请求，授权服务器颁发授权码，通过按[附录B](../AppendixB/b.md)使用“application/x-www-form-urlencoded”格式向重定向URI的查询部分添加下列参数传递授权码至客户端：
- code    
  必需的。授权服务器生成的授权码。授权码必须在颁发后很快过期以减小泄露风险。推荐的最长的授权码生命周期是10分钟。客户端不能使用授权码超过一次。如果一个授权码被使用一次以上，授权服务器必须拒绝该请求并应该撤销（如可能）先前发出的基于该授权码的所有令牌。授权码与客户端标识和重定向URI绑定。
- state    
  必需的，若“state”参数在客户端授权请求中提交。从客户端接收的精确值。
  
例如，授权服务器通过发送以下HTTP响应重定向用户代理：

    HTTP/1.1 302 Found
    Location: https://client.example.com/cb?code=SplxlOBeZQQYbYS6WxSbIA&state=xyz
  
客户端必须忽略无法识别的响应参数。本规范未定义授权码字符串大小。客户端应该避免假设代码值的长度。授权服务器应记录其发放的任何值的大小。

###### 错误响应

如果由于缺失、无效或不匹配的重定向URI而请求失败，或者如果客户端表示缺失或无效，授权服务器应该通知资源所有者该错误且不能向无效的重定向URI自动重定向用户代理。

如果资源所有者拒绝访问请求，或者如果请求因为其他非缺失或无效重定向URI原因而失败，授权服务器通过按[附录B](../AppendixB/b.md)使用“application/x-www-form-urlencoded”格式向重定向URI的查询部分添加下列参数通知客户端：
- error    
  必需的。下列ASCII[USASCII]错误代码之一：
  - invalid_request    
  请求缺少必需的参数、包含无效的参数值、包含一个参数超过一次或其他不良格式。
  - unauthorized_client    
  客户端未被授权使用此方法请求授权码。
  - access_denied    
  资源所有者或授权服务器拒绝该请求。
  - unsupported_response_type    
  授权服务器不支持使用此方法获得授权码。
  - invalid_scope    
  请求的范围无效，未知的或格式不正确。
  - server_error    
  授权服务器遇到意外情况导致其无法执行该请求。（此错误代码是必要的，因为500内部服务器错误HTTP状态代码不能由HTTP重定向返回给客户端）。
  - temporarily_unavailable    
  授权服务器由于暂时超载或服务器维护目前无法处理请求。（此错误代码是必要的，因为503服务不可用HTTP状态代码不可以由HTTP重定向返回给客户端）。
  
  “error”参数的值不能包含集合％x20-21 /％x23-5B /％x5D-7E以外的字符。
- error_description    
  可选的。提供额外信息的人类可读的ASCII[USASCII]文本，用于协助客户端开发人员理解所发生的错误。    
  “error_description”参数的值不能包含集合％x20-21 /％x23-5B /％x5D-7E以外的字符。
- error_uri    
  可选的。指向带有有关错误的信息的人类可读网页的URI，用于提供客户端开发人员关于该错误的额外信息。    
  “error_uri”参数值必须符合URI参考语法，因此不能包含集合％x21/%x23-5B /％x5D-7E以外的字符。
- state    
  必需的，若“state”参数在客户端授权请求中提交。从客户端接收的精确值。

例如，授权服务器通过发送以下HTTP响应重定向用户代理：

    HTTP/1.1 302 Found
    Location: https://client.example.com/cb?error=access_denied&state=xyz

#### 访问令牌请求

客户端通过使用按[附录B](../AppendixB/b.md)“application/x-www-form-urlencoded”格式在HTTP请求实体正文中发送下列UTF-8字符编码的参数向令牌端点发起请求：
- grant_type    
  必需的。值必须被设置为“authorization_code”。
- code    
  从授权服务器收到的授权码。
- redirect_uri    
  必需的，若“redirect_uri”参数如[4.1.1](../Section04/4.1.1.md)节所述包含在授权请求中，且他们的值必须相同。
- client_id    
  必需的，如果客户端没有如[3.2.1](../Section03/3.2.1.md)节所述与授权服务器进行身份认证。
  
如果客户端类型是机密的或客户端被颁发了客户端凭据（或选定的其他身份验证要求），客户端必须如
  必需的，如果客户端没有如[3.2.1](../Section03/3.2.1.md)节所述与授权服务器进行身份验证。

例如，客户端使用TLS发起如下的HTTP请求（额外的换行符仅用于显示目的）：

    POST /token HTTP/1.1
    Host: server.example.com
    Authorization: Basic czZCaGRSa3F0MzpnWDFmQmF0M2JW
    Content-Type: application/x-www-form-urlencoded
    grant_type=authorization_code&code=SplxlOBeZQQYbYS6WxSbIA&redirect_uri=https%3A%2F%2Fclient%2Eexample%2Ecom%2Fcb

授权服务器必须：
- 要求机密客户端或任何被颁发了客户端凭据（或有其他身份验证要求）的客户端进行客户端身份验证，
- 若包括了客户端身份验证，验证客户端身份，
- 确保授权码颁发给了通过身份验证的机密客户端，或者如果客户端是公开的，确保代码颁发给了请求中的“client_id”，
- 验证授权码是有效的，并
- 确保给出了“redirect_uri”参数，若“redirect_uri”参数如[4.1.1](../Section04/4.1.1.md)所述包含在初始授权请求中，且若包含，确保它们的值是相同的。

#### 访问令牌响应

如果访问令牌请求是有效的且被授权，授权服务器如5.1节所述颁发访问令牌以及可选的刷新令牌。如果请求客户端身份验证失败或无效，授权服务器如5.2节所述的返回错误响应。

一个样例成功响应：

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

## 隐式许可

隐式授权类型被用于获取访问令牌（它不支持发行刷新令牌），并对知道操作具体重定向URI的公共客户端进行优化。这些客户端通常在浏览器中使用诸如JavaScript的脚本语言实现。

由于这是一个基于重定向的流程，客户端必须能够与资源所有者的用户代理（通常是Web浏览器）进行交互并能够接收来自授权服务器的传入请求（通过重定向）。

不同于客户端分别请求授权和访问令牌的授权码许可类型，客户端收到访问令牌作为授权请求的结果。

隐式许可类型不包含客户端身份验证而依赖于资源所有者在场和重定向URI的注册。因为访问令牌被编码到重定向URI中，它可能会暴露给资源所有者和其他驻留在相同设备上的应用。

     +----------+
     | Resource |
     |  Owner   |
     |          |
     +----------+
          ^
          |
         (B)
     +----|-----+          Client Identifier     +---------------+
     |         -+----(A)-- & Redirection URI --->|               |
     |  User-   |                                | Authorization |
     |  Agent  -|----(B)-- User authenticates -->|     Server    |
     |          |                                |               |
     |          |<---(C)--- Redirection URI ----<|               |
     |          |          with Access Token     +---------------+
     |          |            in Fragment
     |          |                                +---------------+
     |          |----(D)--- Redirection URI ---->|   Web-Hosted  |
     |          |          without Fragment      |     Client    |
     |          |                                |    Resource   |
     |     (F)  |<---(E)------- Script ---------<|               |
     |          |                                +---------------+
     +-|--------+
       |    |
      (A)  (G) Access Token
       |    |
       ^    v
     +---------+
     |         |
     |  Client |
     |         |
     +---------+
注：说明步骤（A）和（B）的直线因为通过用户代理而被分为两部分。

图4：隐式许可流程

图4中的所示流程包含以下步骤：
- （A）客户端通过向授权端点引导资源所有者的用户代理开始流程。客户端包括它的客户端标识、请求范围、本地状态和重定向URI，一旦访问被许可（或拒绝）授权服务器将传送用户代理回到该URI。
- （B）授权服务器验证资源拥有者的身份（通过用户代理），并确定资源所有者是否授予或拒绝客户端的访问请求。
- （C）假设资源所有者许可访问，授权服务器使用之前（在请求时或客户端注册时）提供的重定向URI重定向用户代理回到客户端。重定向URI在URI片段中包含访问令牌。
- （D）用户代理顺着重定向指示向Web托管的客户端资源发起请求（按[RFC2616][RFC2616]该请求不包含片段）。用户代理在本地保留片段信息。
- （E）Web托管的客户端资源返回一个网页（通常是带有嵌入式脚本的HTML文档），该网页能够访问包含用户代理保留的片段的完整重定向URI并提取包含在片段中的访问令牌（和其他参数）。
- （F）用户代理在本地执行Web托管的客户端资源提供的提取访问令牌的脚本。
- （G）用户代理传送访问令牌给客户端。

参见[1.3.2](../Section01/1.2.3.md)节和第[9](../Section09/9.md)节了解使用隐式许可的背景。

参见[10.3](../Section10/10.3.md)节和[10.16](../Section10/10.16.md)节了解当使用隐式许可时的重要安全注意事项。

[RFC2616]: http://tools.ietf.org/html/rfc2616 "HTTP/1.1"

#### 授权请求

客户端通过按[附录B](../AppendixB/b.md)使用“application/x-www-form-urlencoded”格式向授权端点URI的查询部分添加下列参数构造请求URI：
- response_type    
  必需的。值必须设置为“token”。
- client_id    
  必需的。如[2.2](../Section02/2.2.md)节所述的客户端标识。
- redirect_uri    
  可选的。如[3.1.2](../Section03/3.1.2.md)节所述。
- scope    
  可选的。如[3.3](../Section03/3.3.md)节所述的访问请求的范围。
- state    
  推荐的。客户度用于维护请求和回调之间的状态的不透明的值。当重定向用户代理回到客户端时，授权服务器包含此值。该参数应该用于防止如[10.12](../Section10/10.12.md)所述的跨站点请求伪造。

客户端使用HTTP重定向响应向构造的URI定向资源所有者，或者通过经由用户代理至该URI的其他可用方法。

例如，客户端使用TLS定向用户代理发起下述HTTP请求（额外的换行仅用于显示目的）：

    GET /authorize?response_type=token&client_id=s6BhdRkqt3&state=xyz&redirect_uri=https%3A%2F%2Fclient%2Eexample%2Ecom%2Fcb HTTP/1.1
    Host: server.example.com

授权服务器验证该请求，确保所有需要的参数已提交且有效。授权服务器必须验证它将重定向访问令牌的重定向URI与如[3.1.2](../Section03/3.1.2.md)节所述的客户端注册的重定向URI匹配。

如果请求是有效的，授权服务器对资源所有者进行身份验证并获得授权决定（通过询问资源所有者或通过经由其他方式确定批准）。

当确定决定后，授权服务器使用HTTP重定向响应向提供的客户端重定向URI定向用户代理，或者通过经由用户代理至该URI的其他可行方法。

#### 访问令牌响应
=======================
如果资源所有者许可访问请求，授权服务器颁发访问令牌，通过使用按[附录B](../AppendixB/b.md)的“application/x-www-form-urlencoded”格式向重定向URI的片段部分添加下列参数传递访问令牌至客户端：
- access_token    
  必需的。授权服务器颁发的访问令牌。
- token_type    
  必需的。如[7.1](../Section07/7.1.md)节所述的颁发的令牌的类型。值是大小写不敏感的。
- expires_in    
  推荐的。以秒为单位的访问令牌生命周期。例如，值“3600”表示访问令牌将在从生成响应时的1小时后到期。如果省略，则授权服务器应该通过其他方式提供过期时间，或者记录默认值。
- scope    
  可选的，若与客户端请求的范围相同；否则，是必需的。如[3.3](../Section03/3.3.md)节所述的访问令牌的范围。
- state    
  必需的，若“state”参数在客户端授权请求中提交。从客户端接收的精确值。授权服务器不能颁发刷新令牌。

例如，授权服务器通过发送以下HTTP响应重定向用户代理：（额外的换行符仅用于显示目的）：

    HTTP/1.1 302 Found
    Location: http://example.com/cb#access_token=2YotnFZFEjr1zCsicMWpAA&state=xyz&token_type=example&expires_in=3600
  
开发人员应注意，一些用户代理不支持在HTTP“Location”HTTP响应标头字段中包含片段组成部分。这些客户端需要使用除了3xx重定向响应以外的其他方法来重定向客户端——-例如，返回一个HTML页面，其中包含一个具有链接到重定向URI的动作的“继续”按钮。

客户端必须忽略无法识别的响应参数。本规范未定义授权码字符串大小。客户端应该避免假设代码值的长度。 授权服务器应记录其发放的任何值的大小。

###### 错误响应

如果由于缺失、无效或不匹配的重定向URI而请求失败，或者如果客户端表示缺失或无效，授权服务器应该通知资源所有者该错误且不能向无效的重定向URI自动重定向用户代理。

如果资源所有者拒绝访问请求，或者如果请求因为其他非缺失或无效重定向URI原因而失败，授权服务器通过按[附录B](../AppendixB/b.md)使用“application/x-www-form-urlencoded”格式向重定向URI的片段部分添加下列参数通知客户端：
- error    
  必需的。下列ASCII[USASCII]错误代码之一：
  - invalid_request    
  请求缺少必需的参数、包含无效的参数值、包含一个参数超过一次或其他不良格式。
  - unauthorized_client    
  客户端未被授权使用此方法请求授权码。
  - access_denied    
  资源所有者或授权服务器拒绝该请求。
  - unsupported_response_type    
  授权服务器不支持使用此方法获得授权码。
  - invalid_scope    
  请求的范围无效，未知的或格式不正确。
  - server_error    
  授权服务器遇到意外情况导致其无法执行该请求。（此错误代码是必要的，因为500内部服务器错误HTTP状态代码不能由HTTP重定向返回给客户端）。
  - temporarily_unavailable    
  授权服务器由于暂时超载或服务器维护目前无法处理请求。 （此错误代码是必要的，因为503服务不可用HTTP状态代码不可以由HTTP重定向返回给客户端）。
  
  “error”参数的值不能包含集合％x20-21 /％x23-5B /％x5D-7E以外的字符。
- error_description    
  可选的。提供额外信息的人类可读的ASCII[USASCII]文本，用于协助客户端开发人员理解所发生的错误。
  
  “error_description”参数的值不能包含集合％x20-21 /％x23-5B /％x5D-7E以外的字符。
- error_uri    
  可选的。指向带有有关错误的信息的人类可读网页的URI，用于提供客户端开发人员关于该错误的额外信息。
  
  “error_uri”参数值必须符合URI参考语法，因此不能包含集合％x21/%x23-5B /％x5D-7E以外的字符。
- state    
  必需的，若“state”参数在客户端授权请求中提交。从客户端接收的精确值。

例如，授权服务器通过发送以下HTTP响应重定向用户代理：

    HTTP/1.1 302 Found
    Location: https://client.example.com/cb#error=access_denied&state=xyz

## 资源所有者密码凭据许可

资源所有者密码凭据许可类型适合于资源所有者与客户端具有信任关系的情况，如设备操作系统或高级特权应用。当启用这种许可类型时授权服务器应该特别关照且只有当其他流程都不可用时才可以。

这种许可类型适合于能够获得资源所有者凭据（用户名和密码，通常使用交互的形式）的客户端。通过转换已存储的凭据至访问令牌，它也用于迁移现存的使用如HTTP基本或摘要身份验证的直接身份验证方案的客户端至OAuth。

     +----------+
     | Resource |
     |  Owner   |
     |          |
     +----------+
          v
          |    Resource Owner
         (A) Password Credentials
          |
          v
     +---------+                                  +---------------+
     |         |>--(B)---- Resource Owner ------->|               |
     |         |         Password Credentials     | Authorization |
     | Client  |                                  |     Server    |
     |         |<--(C)---- Access Token ---------<|               |
     |         |    (w/ Optional Refresh Token)   |               |
     +---------+                                  +---------------+
图5：资源所有者密码凭据流程

图5中的所示流程包含以下步骤：
- （A）资源所有者提供给客户端它的用户名和密码。
- （B）通过包含从资源所有者处接收到的凭据，客户端从授权服务器的令牌端点请求访问令牌。当发起请求时，客户端与授权服务器进行身份验证。
- （C）授权服务器对客户端进行身份验证，验证资源所有者的凭证，如果有效，颁发访问令牌。

#### 授权请求和响应

客户端获得资源所有者凭据所通过的方式超出了本规范的范围。一旦获得访问令牌，客户端必须丢弃凭据。

#### 访问令牌请求

客户端通过使用按[附录B](../AppendixB/b.md)“application/x-www-form-urlencoded”格式在HTTP请求实体正文中发送下列UTF-8字符编码的参数向令牌端点发起请求：
- grant_type    
  必需的。值必须设置为“password”。
- username    
  必需的。资源所有者的用户名。
- password    
  必需的。资源所有者的密码。
- scope    
  可选的。如[3.3](../Section03/3.3.md)节所述的访问请求的范围。
如果客户端类型是机密的或客户端被颁发了客户端凭据（或选定的其他身份验证要求），客户端必须如[3.2.1]((../Section03/3.2.1.md))节所述与授权服务器进行身份验证。

例如，客户端使用传输层安全发起如下HTTP请求（额外的换行仅用于显示目的）：

    POST /token HTTP/1.1
    Host: server.example.com
    Authorization: Basic czZCaGRSa3F0MzpnWDFmQmF0M2JW
    Content-Type: application/x-www-form-urlencoded
    grant_type=password&username=johndoe&password=A3ddj3w
  
授权服务器必须：
- 要求机密客户端或任何被颁发了客户端凭据（或有其他身份验证要求）的客户端进行客户端身份验证，
- 若包括了客户端身份验证，验证客户端身份，并
- 使用它现有的密码验证算法验证资源所有者的密码凭据。

由于这种访问令牌请求使用了资源所有者的密码，授权服务器必须保护端点防止暴力攻击（例如，使用速率限制或生成警报）。 

#### 访问令牌响应

如果访问令牌请求是有效的且被授权，授权服务器如[5.1](../Section05/5.1.md)节所述颁发访问令牌以及可选的刷新令牌。如果请求客户端身份验证失败或无效，授权服务器如[5.2](../Section05/5.2.md)节所述的返回错误响应。
一个样例成功响应：

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

## 客户端凭据许可

当客户端请求访问它所控制的，或者事先与授权服务器协商（所采用的方法超出了本规范的范围）的其他资源所有者的受保护资源，客户端可以只使用它的客户端凭据（或者其他受支持的身份验证方法）请求访问令牌。

客户端凭据许可类型必须只能由机密客户端使用。

     +---------+                                  +---------------+
     |         |                                  |               |
     |         |>--(A)- Client Authentication --->| Authorization |
     | Client  |                                  |     Server    |
     |         |<--(B)---- Access Token ---------<|               |
     |         |                                  |               |
     +---------+                                  +---------------+
图6：客户端凭证流程

图6中的所示流程包含以下步骤：
- （A）客户端与授权服务器进行身份验证并向令牌端点请求访问令牌。
- （B）授权服务器对客户端进行身份验证，如果有效，颁发访问令牌。

#### 授权请求和响应

由于客户端身份验证被用作授权许可，所以不需要其他授权请求。

#### 访问令牌请求

客户端通过使用按附录B“application/x-www-form-urlencoded”格式在HTTP请求实体正文中发送下列UTF-8字符编码的参数向令牌端点发起请求：
- grant_type    
  必需的。值必须设置为“client_credentials”。
- scope    
  可选的。如[3.3](../Section03/3.3.md)节所述的访问请求的范围。

客户端必须如[3.2.1](../Section03/3.2.1.md)所述与授权服务器进行身份验证。

例如，客户端使用传输层安全发起如下HTTP请求（额外的换行仅用于显示目的）：

    POST /token HTTP/1.1
    Host: server.example.com
    Authorization: Basic czZCaGRSa3F0MzpnWDFmQmF0M2JW
    Content-Type: application/x-www-form-urlencoded
    grant_type=client_credentials
授权服务器必须对客户端进行身份验证。

#### 访问令牌响应

如果访问令牌请求是有效的且被授权，授权服务器如[5.1](../Section05/5.1.md)节所述颁发访问令牌以及可选的刷新令牌。刷新令牌不应该包含在内。 如果请求因客户端身份验证失败或无效，授权服务器如[5.2](../Section05/5.2.md)节所述的返回错误响应。

一个样例成功响应：

    HTTP/1.1 200 OK
    Content-Type: application/json;charset=UTF-8
    Cache-Control: no-store
    Pragma: no-cache
    {
      "access_token":"2YotnFZFEjr1zCsicMWpAA",
      "token_type":"example",
      "expires_in":3600， "example_parameter":"example_value"
    }

## 扩展许可

通过使用绝对URI作为令牌端点的“grant_type”参数的值指定许可类型，并通过添加任何其他需要的参数，客户端使用扩展许可类型。

例如，采用[OAuth-SAML]定义的安全断言标记语言（SAML）2.0断言许可类型请求访问令牌，客户端可以使用TLS发起如下的HTTP请求（额外的换行仅用于显示目的）：

    POST /token HTTP/1.1
    Host: server.example.com
    Content-Type: application/x-www-form-urlencoded
    grant_type=urn%3Aietf%3Aparams%3Aoauth%3Agrant-type%3Asaml2bearer&assertion=PEFzc2VydGlvbiBJc3N1ZUluc3RhbnQ9IjIwMTEtMDU[...为简洁起见省略...]aG5TdGF0ZW1lbnQ-PC9Bc3NlcnRpb24-
如果访问令牌请求是有效的且被授权，授权服务器如[5.1](../Section05/5.1.md)节所述颁发访问令牌以及可选的刷新令牌。如果请求因客户端身份验证失败或无效，授权服务器如[5.2](../Section05/5.2.md)节所述的返回错误响应。