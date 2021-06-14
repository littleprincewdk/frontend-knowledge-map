![](https://raw.githubusercontent.com/littleprincewdk/figure-bed/master/20210503161057.png)
![](https://raw.githubusercontent.com/littleprincewdk/figure-bed/master/20210503161158.png)
![](https://raw.githubusercontent.com/littleprincewdk/figure-bed/master/20210504134816.png)

## 强缓存

如果命中强缓存(`cache-control`和`expires`)，则直接从缓存中取

### `expires`

`http1.0` 的规范，它的值为一个绝对时间的 GMT 格式的时间字符串，如 `Mon,10Jun201521:31:12GMT` ，如果发送请求的时间在 `expires` 之前，那么本地缓存始终有效，否则就会发送请求到服务器来获取资源

### `pragma: no-cache`

`http1.0` 的规范，相当于 `cache-control: no-cache`

### `cache-control`

`cache-control：max-age=seconds` ，这是 `http1.1` 时出现的 `header` 信息，是一个相对值，优先级高于 `expires`

其他值：

- `no-cache`：不使用本地缓存，使用缓存协商
- `no-store`：直接禁止游览器缓存数据，每次用户请求该资源，都会向服务器发送一个请求，每次都会下载完整的资源
- `public`：可以被所有的用户缓存，包括终端用户和 CDN 等中间代理服务器。
- `private`：只能被终端用户的浏览器缓存，不允许 CDN 等中继缓存服务器对其缓存。是默认值

## 协商缓存

如果没有命中强缓存，则进入协商缓存。浏览器发送请求到服务器，带上相关`header`（Etag/If-None-Match 和 Last-Modified/If-Modified-Since），服务器根据`header`信息判断是否命中协商缓存。如果命中则服务器返回新的响应 `header` 信息更新缓存中的对应 `header` 信息，但是并不返回资源内容，它会告知浏览器可以直接从缓存获取；否则返回最新的资源内容

Etag: 强校验器
Last-Modified：弱校验器（说它弱是因为它只能精确到一秒）

## 缓存优先级

查找浏览器缓存时会按顺序查找: Service Worker-->Memory Cache-->Disk Cache-->Push Cache

## 刷新对于强缓存和协商缓存的影响

1. 当`ctrl+f5`强制刷新网页时，直接从服务器加载，跳过强缓存和协商缓存
2. 当`f5`刷新网页时，跳过强缓存，但是会检查协商缓存
3. 浏览器地址栏中写入 URL，回车 浏览器发现缓存中有这个文件了，不用继续请求了，直接去缓存拿。（最快）
