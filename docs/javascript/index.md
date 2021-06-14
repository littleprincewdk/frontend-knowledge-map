## JS 数据类型

基本类型：`Number`、`Boolean`、`String`、`null`、`undefined`、`symbol`（ES6 新增的），`BigInt`（ES2020） 引用类型：`Object`，对象子类型（`Array`，`Function`）

### JS 整数是怎么表示的？

通过 `Number` 类型来表示，遵循 IEEE754 标准，通过 64 位来表示一个数字，（1 + 11 + 52），最大安全数字是 `Math.pow(2, 53) - 1`，对于 16 位十进制。（符号位 + 指数位 + 小数部分有效位）

### Number() 的存储空间是多大？如果后台发送了一个超过最大自己的数字怎么办

`Math.pow(2, 53)` ，53 为有效数字，会发生截断，等于 JS 能支持的最大数字。

## `0.1 + 0.2 === 0.3`？为什么？

JavaScirpt 使用 `Number` 类型来表示数字（整数或浮点数），遵循 IEEE 754 标准，通过 64 位来表示一个数字（1 + 11 + 52）

- 1 符号位，0 表示正数，1 表示负数 s
- 11 指数位（e）
- 52 尾数，小数部分（即有效数字）

最大安全数字：`Number.MAX_SAFE_INTEGER = Math.pow(2, 53) - 1`，转换成整数就是 16 位，所以 `0.1 === 0.1`，是因为通过 `toPrecision(16)` 去有效位之后，两者是相等的。
在两数相加时，会先转换成二进制，0.1 和 0.2 转换成二进制的时候尾数会发生无限循环，然后进行对阶运算，JS 引擎对二进制进行截断，所以造成精度丢失。
所以总结：精度丢失可能出现在进制转换和对阶运算中

## 闭包

一个函数和对其周围状态（`lexical environment`，词法环境）的引用捆绑在一起（或者说函数被引用包围），这样的组合就是闭包（`closure`）

## 作用域

ES5 中只存在两种作用域：全局作用域和函数作用域。在 JavaScript 中，我们将作用域定义为一套规则，这套规则用来管理引擎如何在当前作用域以及嵌套子作用域中根据标识符名称进行变量（变量名或者函数名）查找

1. 限制变量的可见性
2. 控制变量的查找

## this

绑定优先级：
New 绑定 > 显示绑定 > 隐式绑定 > 默认绑定

## 箭头函数

## Event Loop

![](https://raw.githubusercontent.com/littleprincewdk/figure-bed/master/20210420114502.png)

![](https://raw.githubusercontent.com/littleprincewdk/figure-bed/master/20210420114444.png)

![](https://raw.githubusercontent.com/littleprincewdk/figure-bed/master/20210420132823.png)

timers 最小 timeout 是 1ms，nodejs 和[Chrome’s timers cap](https://chromium.googlesource.com/chromium/blink/+/master/Source/core/frame/DOMTimer.cpp#93)
是对齐的

nodejs 中所有队列都是一次执行完，浏览器中宏任务是一轮循环执行一个，微任务

![](https://raw.githubusercontent.com/littleprincewdk/figure-bed/master/20210420165916.png)

![](https://raw.githubusercontent.com/littleprincewdk/figure-bed/master/20210420213907.png)
https://www.youtube.com/watch?v=sGTRmPiXD4Y&t=565s
https://www.youtube.com/watch?v=_c51fcXRLGw

https://blog.libtorrent.org/2012/10/asynchronous-disk-io/

![](https://raw.githubusercontent.com/littleprincewdk/figure-bed/master/%E6%88%AA%E5%B1%8F2021-04-20%20%E4%B8%8B%E5%8D%8810.34.23.png)

## 手写

- [new](https://github.com/littleprincewdk/javascript-algorithms/tree/master/src/javascript-implementation/new)
- [call](https://github.com/littleprincewdk/javascript-algorithms/tree/master/src/javascript-implementation/call)
- [apply](https://github.com/littleprincewdk/javascript-algorithms/tree/master/src/javascript-implementation/apply)
- [bind](https://github.com/littleprincewdk/javascript-algorithms/tree/master/src/javascript-implementation/bind)
- [Promise](https://github.com/littleprincewdk/javascript-algorithms/tree/master/src/javascript-implementation/Promise)
- [debounce](https://github.com/littleprincewdk/javascript-algorithms/tree/master/src/javascript-implementation/debounce)
- [throttle](https://github.com/littleprincewdk/javascript-algorithms/tree/master/src/javascript-implementation/throttle)
- [curry](https://github.com/littleprincewdk/javascript-algorithms/tree/master/src/javascript-implementation/curry)
- [compose](https://github.com/littleprincewdk/javascript-algorithms/tree/master/src/javascript-implementation/compose)
- [clone](https://github.com/littleprincewdk/javascript-algorithms/tree/master/src/javascript-implementation/clone)
- [unique](https://github.com/littleprincewdk/javascript-algorithms/tree/master/src/javascript-implementation/unique)
- [flatten](https://github.com/littleprincewdk/javascript-algorithms/tree/master/src/javascript-implementation/flatten)
