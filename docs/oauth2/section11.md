# 附录A. 增强巴科斯-诺尔范式（ABNF）语法

本节提供了本文档中定义的元素按[RFC5234][RFC5234]记法的增强巴克斯诺尔范式（ABNF）的语法描述。下列ABNF用Unicode代码要点[W3C.REC-XML-20081126]的术语定义；这些字符通常以UTF-8编码。元素按首次定义的顺序排列。

一些定义遵循使用来自[RFC3986][RFC3986] `URI引用` 的定义。

一些定义遵循使用这些通用的定义：

```
VSCHAR     = %x20-7E
NQCHAR     = %x21 / %x23-5B / %x5D-7E
NQSCHAR    = %x20-21 / %x23-5B / %x5D-7E
UNICODECHARNOCRLF = %x09 /%x20-7E / %x80-D7FF / %xE000-FFFD / %x10000-10FFFF
```

（UNICODECHARNOCRLF定义基于[W3C.REC-XML-20081126]2.2节中定义的字符，但忽略了回车和换行字符。）

## client_id 语法

`client_id` 元素在[客户端密码][客户端密码]节定义：

```
client-id = *VSCHAR
```

## client_secret 语法

`client_secret` 元素在[客户端密码][客户端密码]节定义：

```
client-secret = *VSCHAR
```

## response_type 语法

`response_type` 元素在[授权端点-响应类型][授权端点-响应类型]节和[可扩展性-定义新的授权端点响应类型][可扩展性-定义新的授权端点响应类型]节中定义：

```
response-type = response-name *( SP response-name )
response-name = 1*response-char
response-char = "_" / DIGIT / ALPHA
```

## scope 语法

`scope` 元素在[协议端点-访问令牌范围][协议端点-访问令牌范围]节中定义：

```
scope       = scope-token *( SP scope-token )
scope-token = 1*NQCHAR
```

## state 语法

`state` 元素在[授权码许可-授权请求]、[授权码许可-授权响应][授权码许可-授权响应]、[授权响应-错误响应]、[隐式许可-授权请求][隐式许可-授权请求]、[获得授权-访问令牌响应][获得授权-访问令牌响应]和[颁发访问令牌-错误响应][颁发访问令牌-错误响应]节中定义：

```
state = 1*VSCHAR
```

## redirect_uri 语法

`redirect_uri` 元素在[授权码许可-授权请求]、[授权码许可-访问令牌请求][授权码许可-访问令牌请求])、[隐式许可-授权请求][隐式许可-授权请求]定义：

```
redirect-uri = URI-reference
```

## error 语法

`error` 元素在[授权响应-错误响应]、[颁发访问令牌-错误响应][颁发访问令牌-错误响应]、[访问受保护资源-错误响应][访问受保护资源-错误响应]和[可扩展性-定义其他错误代码][可扩展性-定义其他错误代码]中定义：

```
error = 1*NQSCHAR
```

## error_description 语法

`error_description` 元素在[授权响应-错误响应]、[颁发访问令牌-错误响应][颁发访问令牌-错误响应]和[访问受保护资源-错误响应][访问受保护资源-错误响应]中定义：

```
error-description = 1*NQSCHAR
```

## error_uri 语法

`error_uri` 元素在[授权响应-错误响应]、[颁发访问令牌-错误响应][颁发访问令牌-错误响应]和[访问受保护资源-错误响应][访问受保护资源-错误响应]中定义：

```
error-uri = URI-reference
```

## grant_type 语法

`grant_type` 元素在[授权码许可-访问令牌请求][授权码许可-访问令牌请求]、[资源所有者密码凭据许可-访问令牌请求][资源所有者密码凭据许可-访问令牌请求]、[获得授权-访问令牌请求][获得授权-访问令牌请求]、[获得授权-扩展许可][获得授权-扩展许可]和[刷新访问令牌][刷新访问令牌]中定义：

```
grant-type = grant-name / URI-reference
grant-name = 1*name-char
name-char  = "-" / "." / "_" / DIGIT / ALPHA
```

## code 语法

`code` 元素在[授权码许可-访问令牌请求][授权码许可-访问令牌请求]节中定义：

```
code = 1*VSCHAR
```

## access_token 语法

`access_token` 元素在[获得授权-访问令牌响应][获得授权-访问令牌响应]节和[颁发访问令牌-错误响应][颁发访问令牌-错误响应]节中定义：

```
access-token = 1*VSCHAR
```

## token_type 语法

`token_type` 元素在[获得授权-访问令牌响应][获得授权-访问令牌响应]、[颁发访问令牌-错误响应][颁发访问令牌-错误响应]和[可扩展性-定义其他错误代码][可扩展性-定义其他错误代码]中定义：

```
token-type = type-name / URI-reference
type-name  = 1*name-char
name-char  = "-" / "." / "_" / DIGIT / ALPHA
```

## expires_in 语法

`expires_in` 元素在[获得授权-访问令牌响应][获得授权-访问令牌响应]节和[颁发访问令牌-错误响应][颁发访问令牌-错误响应]节中定义的：

```
expires-in = 1*DIGIT
```

## username 语法

`username` 元素在[资源所有者密码凭据许可-访问令牌请求][资源所有者密码凭据许可-访问令牌请求]节中定义：

```
username = *UNICODECHARNOCRLF
```

## password 语法

`password` 元素在[资源所有者密码凭据许可-访问令牌请求][资源所有者密码凭据许可-访问令牌请求]节中定义：

```
password = *UNICODECHARNOCRLF
```

## refresh_token 语法

`refresh_token` 元素在[颁发访问令牌-错误响应][颁发访问令牌-错误响应]节和第[刷新访问令牌][刷新访问令牌]节中定义：

```
refresh-token = 1*VSCHAR
```

## 端点参数语法

新的端点参数的语法在[定义新的端点参数][定义新的端点参数]节中定义：

```
param-name = 1*name-char
name-char  = "-" / "." / "_" / DIGIT / ALPHA
```

[客户端密码]: oauth2/section02#客户端密码 "客户端密码"

[协议端点-访问令牌范围]: oauth2/section03#访问令牌范围 "协议端点-访问令牌范围"
[授权端点-响应类型]: oauth2/section03#响应类型 "授权端点-响应类型"

[定义新的端点参数]: oauth2/section07#定义新的端点参数 "定义新的端点参数"
[可扩展性-定义访问令牌类型]: oauth2/section07#定义访问令牌类型 "可扩展性-定义访问令牌类型"
[可扩展性-定义其他错误代码]: oauth2/section07#定义其他错误代码 "可扩展性-定义其他错误代码"
[可扩展性-定义新的授权端点响应类型]: oauth2/section07#定义新的授权端点响应类型 "可扩展性-定义新的授权端点响应类型"

[颁发访问令牌-错误响应]: oauth2/section05#错误响应 "颁发访问令牌-错误响应"
[刷新访问令牌]: oauth2/section05#刷新访问令牌 "刷新访问令牌"

[资源所有者密码凭据许可-访问令牌请求]: oauth2/section04#访问令牌请求-1 "资源所有者密码凭据许可-访问令牌请求"
[获得授权-访问令牌请求]: oauth2/section04#访问令牌请求 "获得授权-访问令牌请求"
[获得授权-访问令牌响应]: oauth2/section04#访问令牌响应 "获得授权-访问令牌响应"
[获得授权-扩展许可]: oauth2/section04#扩展许可 "获得授权-扩展许可"

[授权码许可-授权请求]: oauth2/section04#授权请求 "授权码许可-授权请求"
[授权码许可-授权响应]: oauth2/section04#授权响应 "授权码许可-授权响应"
[授权码许可-访问令牌请求]: oauth2/section04#访问令牌请求 "授权码许可-访问令牌请求"

[隐式许可-授权请求]: oauth2/section04#授权请求-1 "隐式许可-授权请求"
[授权响应-错误响应]: oauth2/section04#错误响应 "授权响应-错误响应"

[访问受保护资源-错误响应]: oauth2/section06#错误响应 "访问受保护资源-错误响应"

[RFC5234]:http://tools.ietf.org/html/rfc5234 "Augmented BNF for Syntax Specifications: ABNF"
[RFC3986]:http://tools.ietf.org/html/rfc3986 "Uniform Resource Identifier (URI): Generic Syntax"