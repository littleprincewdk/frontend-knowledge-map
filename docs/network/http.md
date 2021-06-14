- HTTP 协议构建于 TCP/IP 协议之上，是一个应用层协议，默认端口号是 80
- HTTP 是**无状态**的
- HTTP 明文传输不安全

## 报文

### 请求报文

```html
<!-- 状态行 -->
<method>
  <request-URL>
    <version>
      <!-- 请求头 -->
      <headers>
        <!-- 请求主体 -->
        <entity-body></entity-body></headers></version></request-URL
></method>
```

### 响应报文

```html
<!-- 状态行 -->
<version>
  <status-code>
    <!-- 响应头 -->
    <headers>
      <!-- 响应主体 -->
      <entity-body></entity-body></headers></status-code
></version>
```

## 请求方法

- GET： 请求一个指定资源的表示形式. 使用 GET 的请求应该只被用于获取数据；无副作用（不会修改数据，不会影响资源的状态。）；幂等（同一 URL 的多个请求应该返回同样的结果）
- HEAD： 请求一个与 GET 请求的响应相同的响应，但没有响应体
- POST： 提交数据；有副作用
- PUT：用请求有效载荷替换目标资源的所有当前表示
- DELETE： 删除指定的资源
- CONNECT： 建立一个到由目标资源标识的服务器的隧道
- OPTIONS：用于描述目标资源的通信选项
- TRACE：沿着到目标资源的路径执行一个消息环回测试
- PATCH：用于对资源应用部分修改

[https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Methods](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Methods)

### GET vs POST

浏览器环境内：
![](https://raw.githubusercontent.com/littleprincewdk/figure-bed/master/%E6%88%AA%E5%B1%8F2021-05-01%20%E4%B8%8B%E5%8D%885.25.43.png)

- GET 用于获取信息，是无副作用的，是幂等的，且可缓存
- POST 用于修改服务器上的数据，有副作用，非幂等，不可缓存

## 状态码

### 1xx 信息响应

|     |                    |
| --- | ------------------ |
| 100 | Continue           |
| 101 | Switching Protocol |

### 2xx 成功响应

|     |                 |
| --- | --------------- |
| 200 | OK              |
| 201 | Created         |
| 202 | Accepted        |
| 204 | No Content      |
| 206 | Partial Content |

### 3xx 重定向

| 300 | Multiple Choice |
| 301 | Moved Permanently |
| 302 | Found |
| 303 | See Other |
| 304 | Not Modified |
| 307 | Temporary Redirect |
| 308 | Permanent Redirect |

### 4xx 客户端响应

|     |                    |
| --- | ------------------ |
| 400 | Bad Request        |
| 401 | Unauthorized       |
| 403 | Forbidden          |
| 405 | Method Not Allowed |

### 5xx 服务端响应

|     |                            |
| --- | -------------------------- |
| 500 | Internal Server Error      |
| 501 | Not Implemented            |
| 502 | Bad Gateway                |
| 503 | Service Unavailable        |
| 504 | Gateway Timeout            |
| 505 | HTTP Version Not Supported |
|     |                            |
|     |                            |
|     |                            |
|     |                            |

[https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Status](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Status)
[https://tools.ietf.org/html/rfc2616#section-10](https://tools.ietf.org/html/rfc2616#section-10)
[https://en.wikipedia.org/wiki/List_of_HTTP_status_codes](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes)
[https://www.iana.org/assignments/http-status-codes/http-status-codes.xhtml](https://www.iana.org/assignments/http-status-codes/http-status-codes.xhtml)

## 301 和 302 场景

301：

- `http://baidu.com` -> `https://baidu.com`
- 更换域名

302：

- 需要登录，跳转到登录页

## HTTP 缓存

![](https://raw.githubusercontent.com/littleprincewdk/figure-bed/master/20210503161057.png)
![](https://raw.githubusercontent.com/littleprincewdk/figure-bed/master/20210503161158.png)
![](https://raw.githubusercontent.com/littleprincewdk/figure-bed/master/20210504134816.png)

### 强缓存

如果命中强缓存(`cache-control`和`expires`)，则直接从缓存中取

#### `expires`

`http1.0` 的规范，它的值为一个绝对时间的 GMT 格式的时间字符串，如 `Mon,10Jun201521:31:12GMT` ，如果发送请求的时间在 `expires` 之前，那么本地缓存始终有效，否则就会发送请求到服务器来获取资源

#### `pragma: no-cache`

`http1.0` 的规范，相当于 `cache-control: no-cache`

#### `cache-control`

`cache-control：max-age=seconds` ，这是 `http1.1` 时出现的 `header` 信息，是一个相对值，优先级高于 `expires`

其他值：

- `no-cache`：不使用本地缓存，使用缓存协商
- `no-store`：直接禁止游览器缓存数据，每次用户请求该资源，都会向服务器发送一个请求，每次都会下载完整的资源
- `public`：可以被所有的用户缓存，包括终端用户和 CDN 等中间代理服务器。
- `private`：只能被终端用户的浏览器缓存，不允许 CDN 等中继缓存服务器对其缓存。是默认值

### 协商缓存

如果没有命中强缓存，则进入协商缓存。浏览器发送请求到服务器，带上相关`header`（Etag/If-None-Match 和 Last-Modified/If-Modified-Since），服务器根据`header`信息判断是否命中协商缓存。如果命中则服务器返回新的响应 `header` 信息更新缓存中的对应 `header` 信息，但是并不返回资源内容，它会告知浏览器可以直接从缓存获取；否则返回最新的资源内容

Etag: 强校验器
Last-Modified：弱校验器（说它弱是因为它只能精确到一秒）

### 缓存优先级

查找浏览器缓存时会按顺序查找: Service Worker-->Memory Cache-->Disk Cache-->Push Cache

### 刷新对于强缓存和协商缓存的影响

1. 当`ctrl+f5`强制刷新网页时，直接从服务器加载，跳过强缓存和协商缓存
2. 当`f5`刷新网页时，跳过强缓存，但是会检查协商缓存
3. 浏览器地址栏中写入 URL，回车 浏览器发现缓存中有这个文件了，不用继续请求了，直接去缓存拿。（最快）

## HTTP 如何实现长连接？什么时候会超时

通过在头部（请求和响应头）设置 Connection:keep-alive，开启长连接。HTTP1.1 以后，默认都是长连接

HTTP 一般会有 httpd 守护进程，里面可以设置 keep-alive timeout，当 tcp 连接闲置超过这个时间就会关闭，也可以在 HTTP 的 header 里面设置超时时间

实际上，HTTP 没有长短连接，只有 TCP 有，TTCP 长连接可以复用一个 TCP 连接来发起多次 HTTP 请求，这样可以减少资源消耗，比如一次请求 HTML，可能还需要请求后续的 jss/css/img 等

## 长轮询 vs 短轮询

![](https://raw.githubusercontent.com/littleprincewdk/figure-bed/master/20210510233406.png)

## HTTP 如何实现安全传输

1. 明文传输 -> 加密。比如 md5 加密密码
2. 防篡改 -> 数据验证
3. 身份认证 ->

## HTTP 2.0

HTTP 1.1 的问题：队头阻塞

### 多路复用

一个 TCP 连接多条流，即多个请求

![](https://raw.githubusercontent.com/littleprincewdk/figure-bed/master/20210510233833.png)

### 二进制传输

### HEAD 压缩

HPACK 压缩

两端维护索引表

### 服务端 Push

## QUIC

谷歌出品，基于 UDP，传输层协议，希望替代 TCP

- 支持多路复用。TPC 的重传机制会加重队头阻塞
- 实现了自己的加密协议，通过雷士 TCP 的 TFO 机制可以实现 0-RTT，也就是说 QUIC 可以在 0-RTT 的情况下建立安全连接诶并传输数据
- 支持重传和纠错机制。丢失一个包的情况下不需要重传，使用纠错机制恢复丢失的包
  - 纠错机制：通过异或的方式，算出发出去的数据的异或值并单独发出一个包，服务端在发现有一个包丢失的情况下，通过其他数据包和异或值包算出丢失宝
  - 在丢失两个包或以上的情况就使用重传机制，因为算不出来了

## 参考

[https://hit-alibaba.github.io/interview/basic/network/HTTP.html](https://hit-alibaba.github.io/interview/basic/network/HTTP.html)
[一文读懂 HTTP/1HTTP/2HTTP/3](https://zhuanlan.zhihu.com/p/102561034)
