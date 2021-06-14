## [生命周期](https://zh-hans.reactjs.org/docs/react-component.html)

当组件实例被创建并插入 DOM 中时，其生命周期调用顺序如下：

- `constructor()`
- `static getDerivedStateFromProps()`
- `render()`
- `componentDidMount()`

当组件的 props 或 state 发生变化时会触发更新。组件更新的生命周期调用顺序如下：

- `static getDerivedStateFromProps()`
- `shouldComponentUpdate()`
- `render()`
- `getSnapshotBeforeUpdate()`
- `componentDidUpdate()`

当组件从 DOM 中移除时会调用如下方法：

- `componentWillUnmount()`

当渲染过程，生命周期，或子组件的构造函数中抛出错误时，会调用如下方法：

- `static getDerivedStateFromError()`
- `componentDidCatch()`

[https://zh-hans.reactjs.org/blog/2018/03/27/update-on-async-rendering.html](https://zh-hans.reactjs.org/blog/2018/03/27/update-on-async-rendering.html)

## state

`setState`更新队列

`setState`是同步执行，异步更新视图

[深入 setState 机制](https://github.com/sisterAn/blog/issues/26)

[为什么 setState 有时同步有时异步](https://github.com/Advanced-Frontend/Daily-Interview-Question/issues/17)

[为什么 React 不能立即更新 this.state，而不对组件进行重新渲染呢?](https://zh-hans.reactjs.org/docs/faq-state.html#why-doesnt-react-update-thisstate-synchronously)

- 这样会破坏掉 `props` 和 `state` 之间的一致性，造成一些难以 `debug` 的问题。
- 这样会让一些我们正在实现的新功能变得无法实现。

## Synthetic Event

- React 借鉴事件委托的方式将大部分事件委托给了 `Document` 对象
- React 中的事件分为 3 类。分别是 `DiscreteEvent`（离散事件），`UserBlockingEvent`（用户阻塞事件），`ContinuousEvent`（连续事件）。不同类型的事件代表了不同的优先级
- 事件委托需要区分捕获和冒泡，有些事件由于没有冒泡过程，只能在捕获阶段进行事件委托。
- 没有进行委托的事件是 `Form` 事件和 `Media` 事件，原因是这些事件针对特性类型元素，委托意义不大，React 将其直接注册到了目标对象。

### 事件代理优点

- 减少事件注册,节省内存
- 简化了 dom 节点更新时, 相应事件的更新。比如一个列表插入新节点不用再给新节点绑定事件

### why 合成事件

- 浏览器兼容性处理，符合`DOM Level3`
- 跨平台处理。`react-native`、`react-art`
- 原生事件升级、改造。比如 `onChange` 事件，它为表单元素定义了统一的变动事件，例如 `blur、change、focus、input` 等

[https://zhuanlan.zhihu.com/p/165089379](https://zhuanlan.zhihu.com/p/165089379)

## Fiber

React 在一些响应体验要求较高的场景不适用，比如动画，布局和手势

根本原因是渲染/更新过程一旦开始无法中断，持续占用主线程，主线程忙于执行 JS，无暇他顾（布局、动画），造成掉帧、延迟响应（甚至无响应）等不佳体验

`reconciliation`包含`diff`和`patch`阶段

1. incremental rendering
2. pause，abort, or reuse work
3. 更新优先级队列
4. concurrency & error boundaries

A fiber represents a unit of work.
Fiber is reimplementation of the stack, specialized for React components. You can think of a single fiber as a virtual stack frame.
A fiber corresponds to a stack frame, but it also corresponds to an instance of a component.
you can think of a child fiber as a tail-called function

`fiber`相当于实现了一个虚拟函数调用栈，每个`fiber`就是一个`stack frame`。函数调用栈里是一个函数调用另一个函数，在`react`里是一个组件调用另一个组件，所以一个`fiber`就是一个组件实例

自定义一个虚拟栈也解锁了另外的功能：`concurrency` 和 `error boundaries`

`fiber tree`是一个单链表结构。从`Stack reconciler`到`Fiber reconciler`，源码层面就是干了一件递归改循环的事情

![](https://raw.githubusercontent.com/littleprincewdk/figure-bed/master/20210510232530.png)

### 参考

[https://github.com/acdlite/react-fiber-architecture](https://github.com/acdlite/react-fiber-architecture)
[https://makersden.io/blog/look-inside-fiber](https://makersden.io/blog/look-inside-fiber)
[https://www.youtube.com/watch?v=ZCuYPiUIONs](https://www.youtube.com/watch?v=ZCuYPiUIONs)
[http://www.ayqy.net/blog/dive-into-react-fiber/](http://www.ayqy.net/blog/dive-into-react-fiber/)

## Hooks

### 规则

- **只在最顶层使用 Hook**。不要在循环，条件或嵌套函数中调用 Hook
- **只在 React 函数中调用 Hook**。不要在普通的 JavaScript 函数中调用 Hook

如果我们想要有条件地执行一个 effect，可以将判断放到 Hook 的内部

### why hooks

`class`组件的弊端：
we often can’t break complex components down any further because the logic is stateful and can’t be extracted to a function or another component

- **Huge components** that are hard to refactor and test.
- **Duplicated logic** between different components and lifecycle methods.
- **Complex patterns** like render props and higher-order components.（unnecessary nesting）
- 在组件之间复用状态逻辑很难。`render props` 和 高阶组件这类方案需要重新组织组件结构，很麻烦，使代码难以理解
- 复杂组件变得难以理解。每个生命周期常常包含一些不相关的逻辑
- 难以理解的 `class`。`class` 是学习 React 的一大屏障，必须去理解 JavaScript 中 `this` 的工作方式；还不能忘记绑定事件处理器；还要在函数组件、`class` 组件、高阶组件和`render props`之间进行决择

hooks 局限：

- 一些生命周期还没有对应的 hook：`getDerivedStateFromError`, `componentDidCatch`
- 函数组件没有实例，一些场景不方便：实例属性（`useRef`）；封装一个`Video`组件暴露一些方法，父组件拿不到（`useImperativeHandle`）
- `useEffect`依赖不好理清（拆分`useEffect`, 拆分组件，细化粒度）
- 复杂组件的 hooks 拆分是个问题，如何拆，什么粒度

### Concurrent 模式

`Concurrent` 模式是一组 React 的新功能，可帮助应用保持响应，并根据用户的设备性能和网速进行适当的调整。

### Suspense

`Suspense` 不是一个数据请求的库，而是一个机制。这个机制是用来给数据请求库向 React 通信说明某个组件正在读取的数据当前仍不可用

```jsx
const ProfilePage = React.lazy(() => import("./ProfilePage")); // 懒加载

// 在 ProfilePage 组件处于加载阶段时显示一个 spinner
<Suspense fallback={<Spinner />}>
  <ProfilePage />
</Suspense>;
```

## [性能优化](https://zh-hans.reactjs.org/docs/optimizing-performance.html)

- 使用生产版本
- 使用开发者工具中的分析器对组件进行分析
- 虚拟化长列表
- 避免调停。`shouldComponentUpdate`，`React.PureComponent`
- 不可变数据的力量

## 发散

react 相比于其他两个框架优秀的一点是对跨端的设计支持，能够这样高瞻远瞩设计的不只是技术专家了，简直就是大师手笔
