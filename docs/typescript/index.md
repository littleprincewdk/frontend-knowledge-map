## interface vs type

### 相同点

- 都可以用来描述对象或函数
- 都可以扩展

### 不同点

- `interface`始终可扩展，`type`创建后不可重新打开添加新属性（不可修改）;重新声明`interface`会进行合并，而`type`会报错`Duplicate identifier`
- `interface`只能用来声明对象，不能重命名原始类型
- 错误信息中`interface`始终以原始形式出现
- 扩展时 TS 会检查一个`inteface`能否可以赋值给被扩展的接口

### 参考

[https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#differences-between-type-aliases-and-interfaces](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#differences-between-type-aliases-and-interfaces)
