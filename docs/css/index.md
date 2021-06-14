## CSS 权重和优先级

![](https://raw.githubusercontent.com/littleprincewdk/figure-bed/master/download.png)

优先级：

1. `!important`
2. 行内样式
3. ID 选择器，权重:1-0-0
4. class、属性选择器和伪类选择器，权重:0-1-0
5. 标签选择器和伪元素选择器，权重:0-0-1

规则：

- 单独使用一个选择器的时候，不能跨等级使 css 规则生效（有些浏览器（ie,edge）256 个 class 能干掉 id, 最新的 chrome 不能）
  eg: 无论多少个 class 组成的选择器，都没有一个 ID 选择器权重高
- 权重不同，权重高的生效
- 权重相同，后来居上

## flex

## visibility vs display vs opacity

- `visibility`设置`hidden`会隐藏元素，但是其位置还存在于文档流中，不会被删除，所以还会触发回流和重绘
- `display`设置了`none`属性会隐藏元素，且其位置也不会被保留下来，所以不会触发回流和重绘
- `opacity`会将元素设置为透明，但是其位置还存在于文档流中，不会被删除，所以还会触发回流和重绘

## 盒模型

完整的 CSS 盒模型应用于块级盒子，内联盒子只使用盒模型中定义的部分内容

块级盒子 = 外边距（margin）+ 边框（border）+ 内边距（padding）+ 实际内容（content）

CSS 盒模型分为标准模型和替代（IE）模型

标准模型：宽度 = 内容的宽度（`content`）+ `border` + `padding`
替代模型：宽度 = 内容宽度（`content` + `border` + `padding`）

默认浏览器会使用标准模型。如果需要使用替代模型，可以通过为其设置 `box-sizing: border-box` 来实现

### 块级盒子（Block box） 和 内联盒子（Inline box）

块级盒子表现：

- 盒子会在内联的方向上扩展并占据父容器在该方向上的所有可用空间，在绝大数情况下意味着盒子会和父容器一样宽
- 每个盒子都会换行
- `width` 和 `height` 属性可以发挥作用
- 内边距（`padding`）, 外边距（`margin`） 和 边框（`border`） 会将其他元素从当前盒子周围“推开”

内联盒子表现：

- 盒子不会产生换行
- `width` 和 `height` 属性将不起作用
- 垂直方向的内边距、外边距以及边框会被应用但是不会把其他处于 `inline` 状态的盒子推开
- 水平方向的内边距、外边距以及边框会被应用且会把其他处于 `inline` 状态的盒子推开

`display: inline-block`表现：

- 不会换行
- `width` 和 `height` 属性可以发挥作用
- `padding`, `margin`, 以及 `border` 会推开其他元素

[https://developer.mozilla.org/zh-CN/docs/Learn/CSS/Building_blocks/The_box_model](https://developer.mozilla.org/zh-CN/docs/Learn/CSS/Building_blocks/The_box_model)

## box-sizing

定义了 `user agent` 应该如何计算一个元素的总宽度和总高度。

属性值：

- `content-box` 是默认值。如果你设置一个元素的宽为`100px`，那么这个元素的内容区会有`100px`宽，并且任何边框和内边距的宽度都会被增加到最后绘制出来的元素宽度中。
- `border-box` 告诉浏览器：你想要设置的边框和内边距的值是包含在`width`内的。也就是说，如果你将一个元素的`width`设为`100px`，那么这`100px`会包含它的`border`和`padding`，内容区的实际宽度是`width`减去(`border` + `padding`)的值。大多数情况下，这使得我们更容易地设定一个元素的宽高。

## BFC（Block Formatting Context）块级格式化上下文

是 Web 页面的可视 CSS 渲染的一部分，是块盒子的布局过程发生的区域，也是浮动元素与其他元素交互的区域

形成 BFC 的条件：

- 根元素（`<html>`）
- 浮动元素（元素的 `float` 不是 `none`）
- 定位元素（元素的 `position` 为 `absolute` 或 `fixed`）
- 行内块元素（元素的 `display` 为 `inline-block`）
- 表格单元格（元素的 `display` 为 `table-cell`，HTML 表格单元格默认为该值）
- 表格标题（元素的 `display` 为 `table-caption`，HTML 表格标题默认为该值）
- 匿名表格单元格元素（元素的 `display` 为 `table`、`table-row`、 `table-row-group`、`table-header-group`、`table-footer-group`（分别是 HTML `table`、`row`、`tbody`、`thead`、`tfoot` 的默认属性）或 `inline-table`）
- `overflow` 计算值(Computed)不为 `visible` 的块元素
- 弹性元素（`display` 为 `flex` 或 `inline-flex` 元素的直接子元素）
- 网格元素（`display` 为 `grid` 或 `inline-grid` 元素的直接子元素）
- 多列容器（元素的 `column-count` 或 `column-width` 不为 `auto`，包括 `column-count` 为 1）
  [https://developer.mozilla.org/zh-CN/docs/Web/Guide/CSS/Block_formatting_context](https://developer.mozilla.org/zh-CN/docs/Web/Guide/CSS/Block_formatting_context)

## 外边距折叠

块的上外边距(`margin-top`)和下外边距(`margin-bottom`)有时合并(折叠)为单个边距，其大小为单个边距的最大值(或如果它们相等，则仅为其中一个)，这种行为称为边距折叠

规则：同正取最大的，同负取绝对值最大的，一正一负，相加

有三种情况会形成外边距重叠：

- 同一层相邻元素之间。相邻的两个元素之间的外边距重叠，除非后一个元素加上`clear-fix`清除浮动
- 没有内容将父元素和后代元素分开
- 空的块级元素

[https://developer.mozilla.org/zh-CN/docs/Web/CSS/CSS_Box_Model/Mastering_margin_collapsing](https://developer.mozilla.org/zh-CN/docs/Web/CSS/CSS_Box_Model/Mastering_margin_collapsing)

## 清除浮动

不清除浮动会发生高度塌陷：浮动元素父元素高度自适应（父元素不写高度时，子元素写了浮动后，父元素会发生高度塌陷）

- `clear`清除浮动（添加空`div`法）在浮动元素下方添加空`div`,并给该元素写 css 样式：`{clear:both;height:0;overflow:hidden;}`
- 给浮动元素父级设置高度
- 父级同时浮动
- 父级设置成 `inline-block`，其 `margin: 0 auto` 居中方式失效
- 给父级添加 `overflow:hidden`
- 万能清除法 `after` 伪类（现在主流方法，推荐使用）

```css
.float:after {
  content: ".";
  clear: both;
  display: block;
  height: 0;
  overflow: hidden;
  visibility: hidden;
}
.float {
  zoom: 1;
}
```

## flex

### 语法

```css
/* 关键字值 */
flex: auto; /* flex: 1 1 auto */
flex: initial; /* flex: 0 1 auto; */
flex: none; /* flex: 0 0 auto */

/* 一个值, 无单位数字: flex-grow */
flex: 2; /* flex: 2 1 0; */

/* 一个值, width/height: flex-basis */
flex: 10em; /* flex: 1 1 10em; */
flex: 30px;
flex: min-content;

/* 两个值: flex-grow | flex-basis */
flex: 1 30px; /* flex: 1 1 30px; */

/* 两个值: flex-grow | flex-shrink */
flex: 2 2; /* flex: 1 1 0%; */

/* 三个值: flex-grow | flex-shrink | flex-basis */
flex: 2 2 10%;

/*全局属性值 */
flex: inherit;
flex: initial;
flex: unset;
```

[https://developer.mozilla.org/zh-CN/docs/Web/CSS/flex](https://developer.mozilla.org/zh-CN/docs/Web/CSS/flex)

## 移动端 1px 问题

### 产生原因

DPR(devicePixelRatio) 设备像素比 = 物理像素 / css 像素

### 解决方案

| 方案                      | 优点                    | 缺点                                      |
| ------------------------- | ----------------------- | ----------------------------------------- |
| 使用 0.5px 实现           | 代码简单，使用 css 即可 | IOS 及 Android 老设备不支持               |
| 使用 border-image 实现    | 兼容目前所有机型        | 修改颜色不方便                            |
| 通过 viewport + rem 实现  | 一套代码，所有页面      | 和 0.5px 一样，机型不兼容                 |
| 使用伪类 + transform 实现 | 兼容所有机型            | 不支持圆角                                |
| box-shadow 模拟边框实现   | 兼容所有机型            | box-shadow 不在盒子模型，需要注意预留位置 |
