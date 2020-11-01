# 快速入门

## 相关术语

- `开发者` 指使用 `KuOAuth` 工具类库的软件研发工程师
- `第三方` 指开发者对接的第三方授权登录平台，比如：Github平台、Gitee平台、微信平台、微博平台
- `用户` 指最终服务的真实用户，产品服务平台的使用者
- `clientId` 客户端身份标识符（应用id），在申请完 `Oauth应用` 后，由 **第三方** 颁发，具有唯一性
- `clientSecret` 客户端密钥，在申请完 `Oauth应用` 后，由 **第三方** 颁发
- `redirectUri` 开发者项目中的有效api地址。用户在确认第三方平台授权（登录）后，第三方平台会重定向到该地址，并携带code等参数
- `state` 用来保持授权会话流程完整性，防止CSRF攻击的安全的随机的参数，由 **开发者** 生成
- `alipayPublicKey` 支付宝公钥。当选择支付宝登录时，必传该值，由 **开发者** 生成
- `openId` 为 `第三方` 平台的用户ID。
- `unionId` 为 `第三方` 平台多应用联合ID。
- `platform` KuOAuth 支持的 `第三方` 平台，比如：GITHUB、GITEE、QQ、Wechat等

## 准备工作

- 申请注册第三方平台的开发者账号
- 创建第三方平台的应用，获取配置信息(accessKey, secretKey, redirectUri)
- 使用集成 `KuOAuth` 工具类库，实现授权登陆

## 快速开始

1. **引入依赖**

```maven
<dependency>
  <groupId>com.kupug.kuoauth</groupId>
  <artifactId>kupug-kuoauth</artifactId>
  <version>${latest.version}</version>
</dependency>
```

2. **创建授权登录平台对象**

每个平台都对应一个具体的 `KuOAuthPlatform` 类，构建方式如下：

```java
// 构建 OAuth 平台配置
// 配置信息，可以参考：com.kupug.kuoauth.model.KuOAuthConfig
KuOAuthConfig config = KuOAuthConfig.builder()
    .clientId("clientId")
    .clientSecret("clientSecret")
    .redirectUri("redirectUri")
    .build();

// 创建授权登录平台对象
// 授权平台类型，可以参考：com.kupug.kuoauth.model.Platform
KuOAuthPlatform platform = PlatformFactory.newInstance(Platform.xxx, config);
```

3. **获取授权链接**

```java
// 授权登录后会回调 redirectUri，并带上 code、state
String authorizeUrl = platform.authorize("state");
```

**注：** `state` 建议 `必传`！`state` 的主要作用就是保证请求完整性，防止CSRF风险。

获取到 `authorizeUrl` 后，可以直接跳转到 `authorizeUrl` 上，或者交付给前端页面进行跳转到 `authorizeUrl` 上

DEMO（SpringBoot）

```java
@GetMapping("/qq/authorize")
public String generateAuthorizeUrl() {

    // @todo 构建 KuOAuthPlatform 对象

    String authorizeUrl = platform.authorize(OAuthUtils.randomState());

    return authorizeUrl;
}
```

4. **登录(获取用户信息)** 

```java
// 构建回调参数对象
KuOAuthCallback oAuthCallback = KuOAuthCallback.buider()
    .code("authorize code")
    .state("authorize state")
    .build();

// 授权登录
KuOAuthLogin oAuthLogin = platform.login(oAuthCallback);
```

**注：** 可以用 `KuOAuthCallback` 类作为 `Login` 接口的入参

DEMO（SpringBoot）

```java
// 方式一（推荐）
@PostMapping("/qq/login")
public Object login(@RequestBody KuOAuthCallback oAuthCallback) {

    // @todo 构建 KuOAuthPlatform 对象

    KuOAuthLogin oAuthLogin = platform.login(oAuthCallback);

    // @todo 登录逻辑处理

    return Object;
}

// 方式二
@PostMapping("/qq/login")
public KuOAuthLogin login(@RequestParam("code") String code, @RequestParam("state") String state) {

    // @todo 构建 KuOAuthPlatform 对象

    KuOAuthCallback oAuthCallback = KuOAuthCallback.buider()
        .code(code)
        .state(state)
        .build();

    KuOAuthLogin oAuthLogin = platform.login(oAuthCallback);

    // @todo 登录逻辑处理

    return Object;
}
```

更多操作，请参考【**集成平台**】章节下各授权平台的详解。