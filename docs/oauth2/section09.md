# IANA考量

## OAuth访问令牌类型注册表

本规范建立OAuth访问令牌类型注册表。

在oauth-ext-review@ietf.org邮件列表上的两周的审查期后，根据一位或多位指定的专家的建议下，按规范需求（[RFC5226][RFC5226]）注册访问令牌类型。然而，为允许发表之前的值的分配，指定的专家（们）一旦他们对这样的规范即将发布感到满意可以同意注册。

注册请求必须使用正确的主题（例如“访问令牌类型example”的请求）发送到oauth-ext-review@ietf.org邮件列表来审查和评论。

在审查期间，指定的专家（们）将同意或拒绝该注册请求，向审查列表和IANA通报该决定。拒绝应该包含解释，并且可能的话，包含如何使请求成功的建议。

IANA必须只接受来自指定的专家（们）的注册表更新并且应该引导所有注册请求至审查邮件列表。

[RFC5226]:http://tools.ietf.org/html/rfc5226 "Guidelines for Writing an IANA Considerations Section in RFCs"

#### 注册模板

- Type name：

  请求的名称（例如，“example”）。
- Additional Token Endpoint Response Parameters:

  随“access_token”参数一起返回的其他响应参数。 新的参数都必须如[11.2](11.2.md)节所述在OAuth参数注册表中分别注册。
- HTTP Authentication Scheme(s):

  HTTP身份验证方案名称，如果有的话，用于使用这种类型的访问令牌对受保护资源进行身份验证。
- Change controller：

  对于标准化过程的RFC，指定为“IETF”。 对于其他，给出负责的部分的名称。 其他细节（例如，邮政地址，电子邮件地址，主页URI）也可以包括在内。
- Specification document(s):

  指定参数的文档的引用文献，最好包括可以用于检索文档副本的URI。 相关章节的指示也可以包含但不是必需的。

## OAuth参数注册表

本规范建立OAuth参数注册表。

在oauth-ext-review@ietf.org邮件列表上的两周的审查期后，根据一位或多位指定的专家的建议下，按规范需求（[RFC5226][RFC5226]）注册列入授权端点请求、授权端点响应、令牌端点请求或令牌端点响应的其他参数。然而，为允许发表之前的值的分配，指定的专家（们）一旦他们对这样的规范即将发布感到满意可以同意注册。

注册请求必须使用正确的主题（例如，参数“example”的请求）发送到oauth-ext-review@ietf.org邮件列表来审查和评论。

在审查期间，指定的专家（们）将同意或拒绝该注册请求，向审查列表和IANA通报该决定。拒绝应该包含解释，并且可能的话，包含如何使请求成功的建议。

IANA必须只接受来自指定的专家（们）的注册表更新并且应该引导所有注册请求至审查邮件列表。

[RFC5226]:http://tools.ietf.org/html/rfc5226 "Guidelines for Writing an IANA Considerations Section in RFCs"

#### 注册模板

- Parameter name:

  请求的名称（例如，“example”）。
- Parameter usage location:

  参数可以使用的位置。 可能的位置为授权请求、授权响应、令牌请求或令牌响应。
- Change controller:

  对于标准化过程的RFC，指定为“IETF”。对于其他，给出负责的部分的名称。其他细节（例如，邮政地址，电子邮件地址，主页URI）也可以包括在内。
- Specification document(s):

  指定参数的文档的引用文献，最好包括可以用于检索文档副本的URI。相关章节的指示也可以包含但不是必需的。

#### 初始注册表内容

OAuth参数注册表中的初始内容：
- Parameter name: client_id
- Parameter usage location: authorization request, token request
- Change controller: IETF
- Specification document(s): [RFC 6749][RFC6749]


- Parameter name: client_secret
- Parameter usage location: token request
- Change controller: IETF
- Specification document(s): [RFC 6749][RFC6749]


- Parameter name: response_type
- Parameter usage location: authorization request
- Change controller: IETF
- Specification document(s): [RFC 6749][RFC6749]


- Parameter name: redirect_uri
- Parameter usage location: authorization request, token request
- Change controller: IETF
- Specification document(s): [RFC 6749][RFC6749]


- Parameter name: scope
- Parameter usage location: authorization request, authorization response, token request, token response
- Change controller: IETF
- Specification document(s): [RFC 6749][RFC6749]


- Parameter name: state
- Parameter usage location: authorization request, authorization response
- Change controller: IETF
- Specification document(s): [RFC 6749][RFC6749]


- Parameter name: code
- Parameter usage location: authorization response, token request
- Change controller: IETF
- Specification document(s): [RFC 6749][RFC6749]


- Parameter name: error_description
- Parameter usage location: authorization response, token response
- Change controller: IETF
- Specification document(s): [RFC 6749][RFC6749]


- Parameter name: error_uri
- Parameter usage location: authorization response, token response
- Change controller: IETF
- Specification document(s): [RFC 6749][RFC6749]


- Parameter name: grant_type
- Parameter usage location: token request
- Change controller: IETF
- Specification document(s): [RFC 6749][RFC6749]


- Parameter name: access_token
- Parameter usage location: authorization response, token response
- Change controller: IETF
- Specification document(s): [RFC 6749][RFC6749]


- Parameter name: token_type
- Parameter usage location: authorization response, token response
- Change controller: IETF
- Specification document(s): [RFC 6749][RFC6749]


- Parameter name: expires_in
- Parameter usage location: authorization response, token response
- Change controller: IETF
- Specification document(s): [RFC 6749][RFC6749]


- Parameter name: username
- Parameter usage location: token request
- Change controller: IETF
- Specification document(s): [RFC 6749][RFC6749]


- Parameter name: password
- Parameter usage location: token request
- Change controller: IETF
- Specification document(s): [RFC 6749][RFC6749]


- Parameter name: refresh_token
- Parameter usage location: token request, token response
- Change controller: IETF
- Specification document(s): [RFC 6749][RFC6749]

[RFC6749]: ../index.md "RFC 6749"

## OAuth授权端点响应类型注册表

本规范建立OAuth授权端点响应类型注册表。

在oauth-ext-review@ietf.org邮件列表上的两周的审查期后，根据一位或多位指定的专家的建议下，按规范需求（[RFC5226][RFC5226]）注册授权端点使用的其他响应类型。然而，为允许发表之前的值的分配，指定的专家（们）一旦他们对这样的规范即将发布感到满意可以同意注册。

注册请求必须使用正确的主题（例如“响应类型example”的请求）发送到oauth-ext-review@ietf.org邮件列表来审查和评论。

在审查期间，指定的专家（们）将同意或拒绝该注册请求，向审查列表和IANA通报该决定。

IANA必须只接受来自指定的专家（们）的注册表更新并且应该引导所有注册请求至审查邮件列表。

[RFC5226]:http://tools.ietf.org/html/rfc5226 "Guidelines for Writing an IANA Considerations Section in RFCs"

#### 注册模板

- Response type name:

  请求的名称（例如，“example”）。
- Change controller:

  对于标准化过程的RFC，指定为“IETF”。对于其他，给出负责的部分的名称。其他细节（例如，邮政地址，电子邮件地址，主页URI）也可以包括在内。
- Specification document(s):

  指定参数的文档的引用文献，最好包括可以用于检索文档副本的URI。相关章节的指示也可以包含但不是必需的

#### 初始注册表内容

OAuth授权端点响应类型注册表的初始内容：
- Response type name: code
- Change controller: IETF
- Specification document(s): [RFC 6749][RFC6749]


- Response type name: token
- Change controller: IETF
- Specification document(s): [RFC 6749][RFC6749]

[RFC6749]: ../index.md "RFC 6749"


## OAuth扩展错误注册表

本规范建立OAuth扩展错误注册表。

在oauth-ext-review@ietf.org邮件列表上的两周的审查期后，根据一位或多位指定的专家的建议下，按规范需求（[RFC5226][RFC5226]）注册与其他协议扩展（例如，扩展的许可类型、访问令牌类型或者扩展参数）一起使用的其他错误代码。然而，为允许发表之前的值的分配，指定的专家（们）一旦他们对这样的规范即将发布感到满意可以同意注册。

注册请求必须使用正确的主题（例如“错误代码example”的请求）发送到oauth-ext-review@ietf.org邮件列表来审查和评论。

在审查期间，指定的专家（们）将同意或拒绝该注册请求，向审查列表和IANA通报该决定。拒绝应该包含解释，并且可能的话，包含如何使请求成功的建议。

IANA必须只接受来自指定的专家（们）的注册表更新并且应该引导所有注册请求至审查邮件列表。

[RFC5226]:http://tools.ietf.org/html/rfc5226 "Guidelines for Writing an IANA Considerations Section in RFCs"

#### 注册模板

- Error name:

  请求的名称（例如，“example”）。错误名称的值
不能包含集合%x20-21 /%x23-5B /%x5D-7E以外的字符。
- Error usage location:

  错误使用的位置。可能的位置是授权代码许可错误响应（[4.1.2.1](../Section04/4.1.2.1.md)节），隐式许可错误响应（[4.2.2.1](../Section04/4.2.2.1.md)节），令牌错误响应（[5.2](../Section05/5.2.md)节），或资源访问错误的响应（[7.2](../Section07/7.2.md)节）。
- Related protocol extension:

  与错语代码一起使用的扩展许可类型、访问令牌类型或扩展参数的名称。
- Change controller:

  对于标准化过程的RFC，指定为“IETF”。对于其他，给出负责的部分的名称。其他细节（例如，邮政地址，电子邮件地址，主页URI）也可以包括在内。
- Specification document(s):

  指定参数的文档的引用文献，最好包括可以用于检索文档副本的URI。相关章节的指示也可以包含但不是必需的。