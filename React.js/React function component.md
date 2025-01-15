#purefunction 
React function component根据输入的 `props` 和 `state` 返回一个确定的 UI（即 JSX 结构），本质上是：
- 相同的 `props` 和 `state` 会始终渲染相同的 UI。
- 组件本身不会直接执行副作用操作（如修改 DOM 或发送网络请求）。

#### 副作用破坏纯函数特性
副作用是指组件渲染之外的行为，如：
- 发起网络请求（例如 `fetch` 数据）。
- 修改 DOM（手动操作或通过第三方库）。
- 设置订阅或定时器。

[[useEffect]] 来专门管理副作用，将它与组件渲染逻辑（纯函数部分）分离.