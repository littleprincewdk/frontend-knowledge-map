## 浏览器内核

- Trident 内核：IE,MaxThon,TT,The World,360,搜狗浏览器等。[又称 MSHTML]
- Gecko 内核：Netscape6 及以上版本，FF,MozillaSuite/SeaMonkey 等
- Presto 内核：Opera7 及以上。[Opera 内核原为：Presto，现为：Blink;]
- Webkit 内核：Safari,Chrome 等。[ Chrome 的 Blink（WebKit 的分支）]

## 页面渲染

1. 解析 HTML 生成 DOM 树
2. 解析 CSS 生成 CSSOM 规则树
3. 将 DOM 树与 CSSOM 规则树合并生成 Render(渲染)树
4. 遍历 Render(渲染)树开始布局， 计算每一个节点的位置大小信息
5. 将渲染树每个节点绘制到屏幕上

![](https://raw.githubusercontent.com/littleprincewdk/figure-bed/master/20210503170004.png)

[渲染页面：浏览器的工作原理](https://developer.mozilla.org/zh-CN/docs/Web/Performance/How_browsers_work)

## 页面更新

![](https://raw.githubusercontent.com/littleprincewdk/figure-bed/master/20210503170730.png)

## 重排和重绘

### 重排发生时机

- 添加|删除可见的 DOM 元素
- 元素位置发生改变
- 元素本省的尺寸发生改变
- 内容变化
- 页面渲染器初始化
- 浏览器窗口大小发生改变

### 如何减少重排

- 避免`table`布局
- 分离读写操作
- 样式集中改变。eg: 修改类名而不是修改样式
- `position`属性为`absolute`或`fixed`
