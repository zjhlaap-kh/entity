### 登录接口
```javascript
{keycloak_domain}/auth/realms/{realm}/protocol/openid-connect/auth?client_id={client_id}&response_type=code&redirect_uri={service_url}
```
##### 需要再地址栏里面用

| 变量 | 值 |
|---| --- |
| {keycloak_domain} | https://keycloak.dev.coherent.com.hk |
| {realm} | 商户名字 例：aia-sg |
| {client_id} | 新加坡的是 connect |
| {service_url} | 你希望用户登录成功之后返回到哪里 |

### 获取token
```javascript
  {keycloak_domain}/auth/realms/{realm}/protocol/openid-connect/token
  post方式
  Content-Type: application/x-www-form-urlencoded
  body: {
    grant_type: authorization_code
    client_id: {client_id}
    code: {code}
    redirect_uri: {service_url}
  }
```
| 变量 | 值 |
| --- | --- |
| {keycloak_domain} | 同上 |
| {realm} | 同上 |
| {client_id} | 同上 |
| {code} | 登录成功之后在query里面的code的值 |
| {service_url} | 这里的地址必须是你访问登录接口的时候的地址 |

### 通过refresh_token去刷新token

```javascript
  Method: POST
  Endpoint: {keycloak_domain}/auth/realms/{realm}/protocol/openid-connect/token
  Header: 'Content-Type: application/x-www-form-urlencoded'
  Body: grant_type=refresh_token
      client_id={client_id}
      refresh_token={refresh_token}
```

| 变量 | 值 |
| --- | --- |
| {keycloak_domain} | 同上 |
| {realm} | 同上 |
| {client_id} | 同上 |
| {refresh_token} | 请求token的时候会一同在返回值中存在 |

### 退出登录
```javascript
{keycloak_domain}/auth/realms/{realm}/protocol/openid-connect/logout?redirect_uri={url}
```
| 变量 | 值 |
| --- | --- |
| {keycloak_domain} | 同上 |
| {realm} | 同上 |
| {url} | 可选，退出登录之后你想跳到哪个页面 |
