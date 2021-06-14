## Webpack 流程

1. 初始化阶段：
   a. **初始化参数**：从配置文件、 配置对象、Shell 参数中读取，与默认配置结合得出最终的参数
   b. **创建编译器对象**：用上一步得到的参数创建 `Compiler` 对象
   c. **初始化编译环境**：包括注入内置插件、注册各种模块工厂、初始化 RuleSet 集合、加载配置的插件等
   d. **开始编译**：执行 `compiler` 对象的 `run` 方法
   e. **确定入口**：根据配置中的 `entry` 找出所有的入口文件，调用 `compilition.addEntry` 将入口文件转换为 `dependence` 对象
2. 构建阶段：
   a. **编译模块(make)**：根据 `entry` 对应的 `dependence` 创建 `module` 对象，调用 `loader` 将模块转译为标准 JS 内容，调用 JS 解释器将内容转换为 AST 对象，从中找出该模块依赖的模块，再 递归 本步骤直到所有入口依赖的文件都经过了本步骤的处理
   b. **完成模块编译**：上一步递归处理所有能触达到的模块后，得到了每个模块被翻译后的内容以及它们之间的 依赖关系图
3. 生成阶段：
   a. 输出资源(seal)：根据入口和模块之间的依赖关系，组装成一个个包含多个模块的 `Chunk`，再把每个 `Chunk` 转换成一个单独的文件加入到输出列表，这步是可以修改输出内容的最后机会
   b. **写入文件系统(emitAssets)**：在确定好输出内容后，根据配置确定输出的路径和文件名，把文件内容写入到文件系统

## Compiler

![](https://raw.githubusercontent.com/littleprincewdk/figure-bed/master/20210503122731.png)

## Compilation

![](https://raw.githubusercontent.com/littleprincewdk/figure-bed/master/20210503122831.png)

`Compiler` 代表了整个 `Webpack` 从启动到关闭的生命周期，而 `Compilation` 只是代表了一次新的编译。

## hash chunkhash contenthash

- `hash`：对应整个项目。所有文件公用，一个改变都改变
- `chunkhash`：对应由`entry`生成的`chunk`。适用于`common`，`vendor`的缓存
- `contenthash`：对应文件内容。适用于提取`css`

[https://juejin.cn/post/6844903542893854734](https://juejin.cn/post/6844903542893854734)

## 热更新

`--hot` 或者

```javascript
devServer:{
   // 告诉 DevServer 要开启模块热替换模式
  hot: true,
},
plugins: [
  // 该插件的作用就是实现模块热替换，实际上当启动时带上 `--hot` 参数，会注入该插件，生成 .hot-update.json 文件。
  new HotModuleReplacementPlugin(),
],
```

然后：

```javascript
if (module.hot) {
  // accept 函数的第一个参数指出当前文件接受哪些子模块的替换，这里表示只接受 ./AppComponent 这个子模块
  // 第2个参数用于在新的子模块加载完毕后需要执行的逻辑
  module.hot.accept(["./AppComponent"], () => {
    // 新的 AppComponent 加载成功后重新执行下组建渲染逻辑
    render(<AppComponent />, window.document.getElementById("app"));
  });
}
```

## 优化

### 诊断

#### 速度分析

[speed-measure-webpack-plugin](https://github.com/stephencookdev/speed-measure-webpack-plugin)可以给出总耗时，每个`plugin`和`loader`耗时

#### 体积分析

[webpack-bundle-analyzer](https://github.com/webpack-contrib/webpack-bundle-analyzer)

### 打包速度优化

1. 使用[thread-loader](https://github.com/webpack-contrib/thread-loader) 和 [happypack](https://github.com/amireh/happypack)加速
   happypack: 多线程平行构建，对首次构建有用，后续变成同步模式

2. 使用[terser-webpack-plugin](https://github.com/webpack-contrib/terser-webpack-plugin)平行压缩 js
   `webpack5`默认带`terser-webpack-plugin`，且`parallel: true`
3. 使用`DllPlugin`预编译模块资源，`DllReferencePlugin`去使用
4. 使用缓存提升二次构建速度：`babel-loader`开启缓存(`cacheDirectory: true`)，`使用cache-loader`(deprecated, 使用 webpack5 `cache:true`代替)，``
5. 缩小构建目标/减少文件搜索范围：使用`exclude`和`include`，`resolve`
6. 开启模块热替换.`webpack-dev-server --hot`自动注入`HotModuleReplacementPlugin`
7. 区分环境。开发环境不做无意义操作：代码压缩、目录内容清理、计算文件 hash、提取 CSS 文件等
8. 选择合适的 `devtool`
9. 第三方依赖外链 script 引入：vue、ui 组件、JQuery 等

### 打包输出优化

1. 压缩代码
2. 开启` Tree-Shaking`
   a. 配置`@babel/preset-env` `"modules": false`，关闭 Babel 的模块转换功能，保留原本的 ES6 模块化语法
   b. 配置`resolve.mainFields`包含`jsnext:main`让 npm 包优先使用 es 的版本
   b. webpack 标记未使用的导出为 `unused harmony export xxx`;
   c. 压缩代码去除
3. 第三方依赖外链 `script` 引入：`react`、`JQuery`等
4. 剥离 `css` 文件，单独打包
5. 使用`SplitChunksPlugin`分割代码，提取公共包
6. 使用`import()`按需加载
7. 开启 `Scope Hoisting`使打包出来的代码文件更小、运行的更快（缺点：那些被引用了一次的模块才能被合并；必须采用 ES6 模块化语句）：`ModuleConcatenationPlugin`

## Rollup

### 一些个人看法

rollup 能从霸主嘴里分一杯羹的原因是创新性地提出了`Tree Shaking`和`Scope Hoisting`，可以减小输出文件大小，不过已经被 webpack 给模仿了去。优点是配置较 webpack 简单，输出可读性好，体积小，无那段加载代码，但优点也是缺点，无加载代码就不支持 `Code Spliting`，异步加载，所以 rollup 适合于打包库。另外 rollup 生态不完善，很多场景都找不到现成的解决方案。

## 参考

[https://mp.weixin.qq.com/s/KlxCK9KsY6KHw67YPD4Ftw](https://mp.weixin.qq.com/s/KlxCK9KsY6KHw67YPD4Ftw)
