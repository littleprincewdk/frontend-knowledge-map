## 两种架构

![](https://raw.githubusercontent.com/littleprincewdk/figure-bed/master/20210608220822.png)

1. 单进程 + 多线程
2. 多进程 + IPC 通信

## Chrome 多进程架构

![](https://raw.githubusercontent.com/littleprincewdk/figure-bed/master/20210608220447.png)

| 进程                                  | 作用                                                                                                                  |
| ------------------------------------- | --------------------------------------------------------------------------------------------------------------------- |
| Browser Process                       | 1. 负责包括地址栏，书签栏，前进后退按钮等部分的工作 2. 负责处理浏览器的一些不可见的底层操作，比如网络请求和文件访问； |
| Renderer Process                      | 负责一个 tab 内关于网页呈现的所有事情                                                                                 |
| Plugin Process                        | 负责控制一个网页用到的所有插件，如 flash                                                                              |
| GPU Process                           | 负责处理 GPU 相关的任务                                                                                               |
| Extension process、Utility process 等 |                                                                                                                       |

### 优点

- 某一渲染进程出问题不会影响其他进程
- 更为安全，在系统层面上限定了不同进程的权限

### 缺点

- 内存占用高。不同进程的内存常常需要包含相同的内容，比如每个
  Renderer Process 都有一个 V8 实例

chrome 为了节省内存限制了最大进程数（由设备的内存和 CPU 能力决定），当达到这一限制时，新打开的 Tab 会共用之前同一个站点的渲染进程

另一个优化是对 Browser Process 的服务化，服务可以方便的分割为不同的进程或者合并为一个进程。比如运行在强的硬件上，会将 Storage、Network、UI、Device 等服务分割为进程；而在资源紧张的硬件上则会把服务合并到 Browser Process，以线程形式存在

## 参考

[图解浏览器的基本工作原理](https://zhuanlan.zhihu.com/p/47407398)

[内部看现代 Web 浏览器](https://developers.google.com/web/updates/2018/09/inside-browser-part1)
