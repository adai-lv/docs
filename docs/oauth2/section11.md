# 附录A. 增强巴科斯-诺尔范式（ABNF）语法

本节提供了本文档中定义的元素按[RFC5234][RFC5234]记法的增强巴克斯诺尔范式（ABNF）的语法描述。下列ABNF用Unicode代码要点[W3C.REC-XML-20081126]的术语定义；这些字符通常以UTF-8编码。元素按首次定义的顺序排列。

一些定义遵循使用来自[RFC3986][RFC3986]“URI引用”的定义。

一些定义遵循使用这些通用的定义：

     VSCHAR     = %x20-7E
     NQCHAR     = %x21 / %x23-5B / %x5D-7E
     NQSCHAR    = %x20-21 / %x23-5B / %x5D-7E
     UNICODECHARNOCRLF = %x09 /%x20-7E / %x80-D7FF / %xE000-FFFD / %x10000-10FFFF
（UNICODECHARNOCRLF定义基于[W3C.REC-XML-20081126]2.2节中定义的字符，但忽略了回车和换行字符。）

## client_id 语法

“client_id”元素在[2.3.1](../Section02/2.3.1.md)节定义：

     client-id     = *VSCHAR

## client_secret 语法

“client_secret”元素在[2.3.1](../Section02/2.3.1.md)节定义：

    client-secret = *VSCHAR

## response_type 语法

“response_type”元素在[3.1.1](../Section03/3.1.1.md)节和[8.4](../Section08/8.4.md)节中定义：

     response-type = response-name *( SP response-name )
     response-name = 1*response-char
     response-char = "_" / DIGIT / ALPHA

## scope 语法

“scope”元素在[3.3](../Section03/3.3.md)节中定义：

     scope       = scope-token *( SP scope-token )
     scope-token = 1*NQCHAR

## state 语法

“state”元素在[4.1.1](../Section04/4.1.1.md)、[4.1.2](../Section04/4.1.2.md)、[4.1.2.1](../Section04/4.1.2.1.md)、[4.2.1](../Section04/4.2.1.md)、[4.2.2](../Section04/4.2.2.md)和[4.2.2.1](../Section04/4.2.2.1.md)节中定义：

     state      = 1*VSCHAR

## redirect_uri 语法

“redirect_uri”元素在[4.1.1](../Section04/4.1.1.md)、[4.1.3](../Section04/4.1.3.md)、[4.2.1](../Section04/4.2.1.md)定义：

     redirect-uri      = URI-reference

## error 语法

“error”元素在[4.1.2.1](../Section04/4.1.2.1.md)、[4.2.2.1](../Section04/4.2.2.1.md)、[5.2](../Section05/5.2.md)、[7.2](../Section07/7.2.md)和[8.5](../Section08/8.5.md)中定义：

    error             = 1*NQSCHAR

## error_description 语法

“error_description”元素在[4.1.2.1](../Section04/4.1.2.1.md)、[4.2.2.1](../Section04/4.2.2.1.md)、[5.2](../Section05/5.2.md)和[7.2](../Section07/7.2.md)中定义：

     error-description = 1*NQSCHAR

## error_uri 语法

“error_uri”元素在[4.1.2.1](../Section04/4.1.2.1.md)、[4.2.2.1](../Section04/4.2.2.1.md)、[5.2](../Section05/5.2.md)和[7.2](../Section07/7.2.md)中定义：

     error-uri         = URI-reference

## grant_type 语法

“grant_type”元素在[4.1.3](../Section04/4.1.3.md)、[4.3.2](../Section04/4.3.2.md)、[4.4.2](../Section04/4.4.2.md)、[4.5](../Section04/4.5.md)和[6](../Section06/6.md)中定义：

     grant-type = grant-name / URI-reference
     grant-name = 1*name-char
     name-char  = "-" / "." / "_" / DIGIT / ALPHA

## code 语法

“code”元素在[4.1.3](../Section04/4.1.3.md)节中定义：

     code       = 1*VSCHAR

## access_token 语法

“access_token”元素在[4.2.2](../Section04/4.2.2.md)节和[5.1](../Section05/5.1.md)节中定义：

     access-token = 1*VSCHAR

## token_type 语法

“token_type”元素在[4.2.2](../Section04/4.2.2.md)、[5.1](../Section05/5.1.md)和[8.1](../Section08/8.1.md)中定义：

     token-type = type-name / URI-reference
     type-name  = 1*name-char
     name-char  = "-" / "." / "_" / DIGIT / ALPHA

## expires_in 语法

“expires_in”元素在[4.2.2](../Section04/4.2.2.md)节和[5.1](../Section05/5.1.md)节中定义的：

     expires-in = 1*DIGIT

## username 语法

“username”元素在[4.3.2](../Section04/4.3.2.md)节中定义：

     username = *UNICODECHARNOCRLF

## password 语法

“password”元素在[4.3.2](../Section04/4.3.2.md)节中定义：

     password = *UNICODECHARNOCRLF

## refresh_token 语法

“refresh_token”元素在[5.1](../Section05/5.1.md)节和第[6](../Section06/6.md)节中定义：

     refresh-token = 1*VSCHAR

## 端点参数语法

新的端点参数的语法在[8.2](../Section08/8.2.md)节中定义：

     param-name = 1*name-char
     name-char  = "-" / "." / "_" / DIGIT / ALPHA

[RFC5234]:http://tools.ietf.org/html/rfc5234 "Augmented BNF for Syntax Specifications: ABNF"
[RFC3986]:http://tools.ietf.org/html/rfc3986 "Uniform Resource Identifier (URI): Generic Syntax"