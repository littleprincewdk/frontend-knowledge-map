## DNS 高速缓存

1. 浏览器缓存
2. 操作系统缓存
3. host 文件
4. 路由器缓存
5. DNS 递归解析器缓存，是否有 A 记录缓存，是否有 NS 记录缓存，是否有 TLD 缓存

## DNS 查询

3 种 DNS 查询类型：

- **递归查询**-在递归查询中，DNS 客户端要求 DNS 服务器（一般为 DNS 递归解析器）将使用所请求的资源记录响应客户端，或者如果解析器无法找到该记录，则返回错误消息
- **迭代查询**-在这种情况下，DNS 客户端将允许 DNS 服务器返回其能够给出的最佳应答。如果所查询的 DNS 服务器与查询名称不匹配，则其将返回对较低级别域名空间具有权威性的 DNS 服务器的引用。然后，DNS 客户端将对引用地址进行查询。此过程继续使用查询链中的其他 DNS 服务器，直至发生错误或超时为止
- **非递归查询**-

以 `www.example.com` 为例， DNS 解析器递归查询：
I. 解析器向 DNS 根域名服务器查询 `.com`（如无缓存）
II. 根域名服务器返回顶级域名服务器(TLD) `.com`
III. 解析器向 `.com` TLD 发出请求
IV. TLD 返回 `example.com` 的 IP 地址
V. 解析器将查询发送到域的域名服务器
VI. 域名服务器返回 `www.example.com` 的 IP 地址
VII. 解析器返回`www.example.com` 的 IP 地址给浏览器

![](https://raw.githubusercontent.com/littleprincewdk/figure-bed/master/20210501135156.png)
![](https://raw.githubusercontent.com/littleprincewdk/figure-bed/master/20210501143250.png)

## 参考

[https://www.cloudflare.com/zh-cn/learning/dns/what-is-dns/](https://www.cloudflare.com/zh-cn/learning/dns/what-is-dns/)
