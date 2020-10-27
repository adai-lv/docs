# 访问受保护资源

通过向资源服务器出示访问令牌，客户端访问受保护资源。资源服务器必须验证访问令牌，并确保它没有过期且其范围涵盖了请求的资源。资源服务器用于验证访问令牌的方法（以及任何错误响应）超出了本规范的范围，但一般包括资源服务器和授权服务器之间的互动或协调。

客户端使用访问令牌与资源服务器进行证认的方法依赖于授权服务器颁发的访问令牌的类型。通常，它涉及到使用具有所采用的访问令牌类型的规范定义的身份验证方案(如[RFC6750][RFC6750])的HTTP“Authorization”的请求标头字段[RFC2617][RFC2617]。

## 访问令牌类型

访问令牌的类型给客户端提供了成功使用该访问令牌（和类型指定的属性）发起受保护资源请求所需的信息 若客户端不理解令牌类型，则不能使用该访问令牌。

例如，[RFC6750][RFC6750]定义的“bearer”令牌类型简单的在请求中包含访问令牌字符串来使用：

```
GET /resource/1 HTTP/1.1
Host: example.com

Authorization: Bearer F_9.B5f-4.1JqM
```
    
而[OAuth-HTTP-MAC]定义的“mac”令牌类型通过与许可类型一起颁发用于对HTTP请求中某些部分签名的消息认证码（MAC）的密钥来使用。

```
GET /resource/1 HTTP/1.1
Host: example.com

Authorization: MAC id="h480djs93hd8",nonce="274312:dj83hs9s",mac="kDZvddkndxvhGRXZhvuDjEWhGeE="
```
    
提供上面的例子仅作说明用途。建议开发人员在使用前查阅[RFC6750][RFC6750]和[OAuth-HTTP-MAC]规范。

每一种访问令牌类型的定义指定与 `access_token` 响应参数一起发送到客户端的额外属性。它还定义了HTTP验证方法当请求受保护资源时用于包含访问令牌。

## 错误响应

如果资源访问请求失败，资源服务器应该通知客户端该错误。虽然规定这些错误响应超出了本规范的范围，但是本文档在[OAuth扩展错误注册表][OAuth扩展错误注册表]节建立了一张公共注册表，用作OAuth令牌身份验证方案之间分享的错误值。

主要为OAuth令牌身份验证设计的新身份验证方案应该定义向客户端提供错误状态码的机制，其中允许的错误值限于本规范建立的错误注册表中。

这些方案可以限制有效的错误代码是注册值的子集。如果错误代码使用命名参数返回，该参数名称应该是 `error`。

其他能够被用于OAuth令牌身份验证的方案，但不是主要为此目的而设计的，可以帮顶他们的错误值到相同方式的注册表项。

新的认证方案也可以选择指定使用 `error_description` 和 `error_uri` 参数，用于以本文档中用法相同的方式的返回错误信息。

[OAuth扩展错误注册表]: oauth2/section09#OAuth扩展错误注册表 "OAuth扩展错误注册表"
[RFC2617]: http://tools.ietf.org/html/rfc2617 "HTTP Authentication: Basic and Digest Access Authentication"
[RFC6750]: http://tools.ietf.org/html/rfc6750 "The OAuth 2.0 Authorization Framework: Bearer Token Usage"
[RFC6750]: http://tools.ietf.org/html/rfc6750 "The OAuth 2.0 Authorization Framework: Bearer Token Usage"