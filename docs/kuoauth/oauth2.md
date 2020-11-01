# OAuth 流程
> 详情参考[OAuth2.0 简介](/oauth2/section01)

## 角色定义

- `Resource Owner` 资源所有者，即代表授权客户端访问本身资源信息的用户（User），也就是应用场景中的“开发者A”
- `Resource Server` 资源服务器，托管受保护的用户账号信息，比如Github
- `Authorization Server` 授权服务器，验证用户身份然后为客户端派发资源访问令牌，比如Github
- `Client` 客户端，即代表意图访问受限资源的第三方应用

**注:** `Resource Server` 和 `Authorization Server` 可以是同一台服务器，也可以是不同的服务器，视具体的授权平台而有所差异

## 授权流程

     +--------+                               +---------------+
     |        |--(A)- Authorization Request ->|   Resource    |
     |        |                               |     Owner     |
     |        |<-(B)-- Authorization Grant ---|               |
     |        |                               +---------------+
     |        |
     |        |                               +---------------+
     |        |--(C)-- Authorization Grant -->| Authorization |
     | Client |                               |     Server    |
     |        |<-(D)----- Access Token -------|               |
     |        |                               +---------------+
     |        |
     |        |                               +---------------+
     |        |--(E)----- Access Token ------>|    Resource   |
     |        |                               |     Server    |
     |        |<-(F)--- Protected Resource ---|               |
     +--------+                               +---------------+
<p style="text-align:center"> 图1：抽象的授权流程</p>

图1中所示，抽象的 `OAuth 2.0` 流程描述了四种角色之间的交互，包括以下步骤：

- (A) **用户**打开**客户端**以后，**客户端**要求**用户**给予授权。
- (B) **用户**同意给予**客户端**授权。
- (C) **客户端**使用上一步获得的授权，向**认证服务器**申请令牌。
- (D) **认证服务器**对**客户端**进行认证以后，确认无误，同意发放令牌
- (E) **客户端**使用令牌，向**资源服务器**申请获取资源。
- (F) **资源服务器**确认令牌无误，同意向**客户端**开放资源。

## 授权许可

- `Authorization Code` 结合普通服务器端应用使用(web端常用的授权方式)
- `Implicit` 结合移动应用或 Web App 使用
- `Resource Owner Password Credentials` 适用于受信任客户端应用，例如同个组织的内部或外部应用
- `Client Credentials` 适用于客户端调用主服务API型应用（比如百度API Store）

## 参考资料

关于OAuth2相关的内容、原理可以自行参阅以下资料：

- [The OAuth 2.0 Authorization Framework](https://tools.ietf.org/html/rfc6749)
- [OAuth 2.0 中文指南](http://docs.kupug.com/#/oauth2/)
- [OAuth 2.0](https://oauth.net/2/)