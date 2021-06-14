## 特点

- 建立在 TCP 协议之上，服务器端的实现比较容易
- 与 HTTP 协议有着良好的兼容性。默认端口也是 80 和 443，并且握手阶段采用 HTTP 协议，因此握手时不容易屏蔽，能通过各种 HTTP 代理服务器
- 数据格式比较轻量，性能开销小，通信高效
- 可以发送文本，也可以发送二进制数据
- 没有同源限制，客户端可以与任意服务器通信
- 协议标识符是`ws`（如果加密，则为`wss`），服务器网址就是 URL

## 使用方式

```javascript
const ws = new WebSocket('ws://localhost:8080/socket');

// 错误处理
ws.onerror = (error) => { ... }

// 连接关闭
ws.onclose = () => { ... }

// 连接建立
ws.onopen = () => {
  // 向服务端发送消息
  ws.send("ping");
}

// 接收服务端发送的消息
ws.onmessage = (msg) => {
  if(msg.data instanceof Blob) {
  // 处理二进制信息
    processBlob(msg.data);
  } else {
    // 处理文本信息
    processText(msg.data);
  }
}
```

## 注意

- 浏览器兼容性。
  - IE10 以下不支持
  - Safari 下不允许使用非标准接口建立连接
- 心跳. WebSocket 本身不会维护心跳机制，一些 Websocket 实现在空闲一段时间会自动断开。所以`socket.io`、`sockjs`这些库会帮你维护心跳
- 一些负载均衡或代理不支持 Websocket。
- 会话和消息队列维护。这些不是 Websocket 协议的职责，而是应用的职责。sockjs 会为每个 Websocket 连接维护一个会话，且这个会话里面会维护一个消息队列，当 Websocket 意外断开时，不至于丢失数据

## 参考

[https://segmentfault.com/a/1190000019697463](https://segmentfault.com/a/1190000019697463)
