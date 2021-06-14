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

HTTP 缓存分为强缓存和协商缓存

- 首先通过`Cache-Control`验证强缓存是否可用，如果强缓存可用，那么直接读取缓存
- 如果不可以，那么进入协商缓存阶段，发起 HTTP 请求，服务器通过请求头中是否带上`If-Modified-Since`和`If-None-Match`这些条件请求字段检查资源是否更新
  - 若资源更新，那么返回资源和 200 状态码
  - 如果资源未更新，那么高数浏览器直接使用缓存

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
