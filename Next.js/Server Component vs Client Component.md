#next 
### Client component: 
	1.运行在浏览器
	2.可以用hook（useState, useEffect）
	3.用'use client'声明
### Server component:
	1.运行在next server 
	2.不能用hook
	3.可以直接fetch（）调用数据
	4.用async定义
	
![[Pasted image 20250220152858.png]]

***
###  Next渲染流程：

### **🔹 渲染流程**

1. **Next.js 服务器先执行 Server Components**
    - 这些组件 **直接在服务器上运行**，并且 **返回纯 HTML**。
    - **不会包含 JavaScript**（节省浏览器计算量）。
    - 例如 `fetch()` 请求、数据库查询等，**都在服务器完成**。
    
2. **服务器返回完整的 HTML 给浏览器**
    - 这个 HTML **已经包含了 Server Components 渲染后的静态内容**。
    - 这意味着**用户可以立即看到页面内容**（类似 SSR）。
    
3. **浏览器收到 HTML 后，开始下载 Client Components 的 JavaScript**
    - 任何包含 `"use client"` 的组件，Next.js **不会在服务器渲染它们的交互逻辑**，而是：
        1. **先返回一个占位符（可能是 HTML）**。
        2. **然后在浏览器下载 JavaScript**，使 Client Components 可以交互（Hydration）。

4. **客户端 Hydration（激活）**    
    - **Client Components 的 JavaScript 被加载并执行**，让交互生效（如 `useState`）。
    - 此时，**事件监听、按钮点击、状态管理等交互功能才会生效**。
*** 
### 传统react渲染流程：
## **🔹 1️⃣ 传统 React（CSR）渲染流程**

1. **浏览器请求 HTML**
    - 服务器返回一个**几乎是空的 HTML 文件**，只有一个 `div#root`：

```js
<html>
  <head>
    <title>My React App</title>
  </head>
  <body>
    <div id="root"></div> <!-- 空的 HTML 容器 -->
    <script src="/bundle.js"></script> <!-- React 应用的 JS -->
  </body>
</html>
```

2. **浏览器下载 React JS**

    - 浏览器开始下载 `bundle.js`，这个文件包含了整个 React 应用的代码。
3. **React 在浏览器执行 & Hydration**
    - `ReactDOM.render(<App />, document.getElementById("root"))` 开始运行：
    - React **生成完整的 HTML 结构** 并插入 `#root`：
    - 这个过程叫做 **"Hydration"**，即 React 将 JavaScript 绑定到 HTML，使其变得可交互。
4. **用户可以与页面交互**
    - 现在按钮的 `onClick` 事件等交互逻辑开始生效。