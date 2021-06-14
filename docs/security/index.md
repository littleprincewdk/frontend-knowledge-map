## XSS 攻击

XSS 即 Cross Site Scripting（跨站脚本攻击），指的是攻击者想尽一切办法将一些可执行的代码注入到网页中，利用这些恶意脚本，攻击者可获取用户的敏感信息如 Cookie、Session 等，进而危害数据安全

XSS 可以分为存储型 XSS（持久型 XSS）和反射型 XSS（非持久性 XSS）

### 存储型

存储型是攻击的代码被服务端写到数据库中，这种攻击危害性很大，因为如果网站访问量很大的话，就会导致大量正常访问页面的用户都受到攻击。

这种攻击常见于带有用户保存数据的网站功能，如论坛发帖、商品评论、用户私信等。具有攻击性的脚本被保存到了服务器并且可以被普通用户完整的从服务器取得并执行，从而获得了在网络上传播的能力。

### 反射型

反射型也叫非持久型，相比于前者危害就小一些，一般通过修改 URL 参数的方式加入攻击代码，诱导用户访问链接从而进行攻击。

这种常见于通过 URL 传递参数的功能，如网站搜索、跳转等。由于需要用户主动打开恶意的 URL 才能生效，攻击者往往会结合多种手段诱导用户点击

### 如何防御

- 输入输出过滤
- Cookie 设置`HttpOnly`
- CSP(Content Security Policy, 内容安全策略)
  开发者明确告诉浏览器哪些外部资源可以加载和执行
  启用 CSP：
  - 通过 HTTP 头信息的`Content-Security-Policy`的字段
  - 通过网页的`<meta>`标签

## CSRF 攻击

CSRF：跨站点请求伪造（Cross-Site Request Forgeries），也被称为 one-click attack 或者 session riding。冒充用户发起请求（在用户不知情的情况下）， 完成一些违背用户意愿的事情（如修改用户信息，删除评论等）

CSRF 攻击的特点：

- 通常发生在第三方网站
- 攻击者不能获取 cookie 等信息，只是使用

### 如何防御

- 验证码
- token 验证
- Cookie 设置 SameSite
- Origin, Referer check：请求来源限制
- 尽量使用 post ，限制 get 使用

## 点击劫持

点击劫持（click hijacking）也称为 UI 覆盖攻击。它通过一些内容（如游戏）误导被攻击者点击，虽然被攻击者点击的是他所看到的网页，但其实所点击的是另一个置于原网页上面的透明页面

### 如何防御

设置我们的网页不允许使用 iframe 被加载到其他网页中。通过在响应头中设置 X-Frame-Options（服务器端进行），X-Frame-Options 可以设置以下三个值：

- DEBY: 不允许任何网页使用 iframe 加载这个页面。
- SAMEORIGIN: 只允许在相同域名（也就是自己的网站）下使用 iframe 加载这个页面。
- ALLOWED-FROM origin: 允许任何网页通过 iframe 加载我这个网页。

## 中间人攻击

中间人(Man-in-the-middle attack, MITM)是指攻击者和通讯的两端分别创建独立的连接，并交换其得到的数据，攻击者可以拦截通信双方的通话并插入新的内容

### 如何防御

HTTPS

## 参考

[https://mp.weixin.qq.com/s/67H9dj5rpYxPvllMllJp5w](https://mp.weixin.qq.com/s/67H9dj5rpYxPvllMllJp5w)
