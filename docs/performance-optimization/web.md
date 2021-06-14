## 性能度量

### [performance](https://developer.mozilla.org/zh-CN/docs/Web/API/Performance)

## 优化手段

### 雅虎军规

![](https://raw.githubusercontent.com/littleprincewdk/figure-bed/master/20210503154756.png)

### 网络&服务器

1. 减少 HTTP 请求数。打包，雪碧图，按需加载
2. 启用 gzip
3. 使用 CDN
4. DNS 预加载

```html
<link rel="dns-prefetch" href="https://github.githubassets.com" />
```

5. 减少 DNS 查询。DNS 缓存
6. 配置 HTTP 缓存
7. 使用 HTTP2
8. 减少`iframe`的使用

### 图片

1. 压缩图片。`imagemin`
2. 使用雪碧图
3. 图片懒加载。`Intersection Observer`或`<img loading="lazy" />`
   `loading="lazy"`原理：先发送请求`range: bytes=0-2047`请求头部数据获取宽高，生成空白占位，滚动到范围时再发完整的请求
4. 使用渐进式图片。先加载低质量或模糊的图片，等完全加载后替换
5. 使用`webp`
6. 使用 svg/字体图标 iconfont 代替图片图标
7. 尽可能使用 css 替代图片

[https://zhuanlan.zhihu.com/p/76820878](https://zhuanlan.zhihu.com/p/76820878)

### HTML

1. 压缩 HTML
2. 服务端渲染

### JavaScript

1. 压缩 JavaScript
2. 放到页面底部
3. webpack 打包的优化。分包，按需加载

### CSS

1. 首屏关键样式表放到`<head>`中
2. [使用`<link>替代`@import`](https://www.stevesouders.com/blog/2009/04/09/dont-use-import/)。`@import`不能并发，可能被延后
3. 不要使用 css 表达式
4. 降低选择器的复杂性

### Cookie

1. 减少 Cookie 大小
2. 静态资源使用无 Cookie 域名

### 其他

1. 延迟加载、按需加载、预加载、 [`preload`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Preloading_content)
2. 减少重绘重排
3. 使用事件委托
4. 使用 requestAnimationFrame 来实现视觉变化
