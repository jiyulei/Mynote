#next 

## **使用 App Router (`app` 目录) 需要注意的事情**

1. **页面文件必须命名为 `page.tsx`**
    
    - 不能用 `index.tsx`，比如 `/about` 页面需要创建 `app/about/page.tsx`。
2. **默认是 Server Component（服务端组件）**
    
    - **不用 `useEffect` 获取数据**，直接 `fetch()` 在组件内请求。
    - 如果要用状态（`useState`）、事件处理（`onClick`），需要加 `'use client'`。
3. **布局文件是 `layout.tsx`，替代 `_app.tsx`**
    
    - 可以共享 `header/footer`，自动作用于所有子页面。
4. **动态路由要用 `[param]` 目录**
    
    - 例如 `app/blog/[id]/page.tsx` 代表 `/blog/:id`。
5. **`loading.tsx` 可以加页面加载状态**
    
    - 自动处理 Suspense，不用自己写 `useState` 控制 loading。

---
### Special File Names in the 'app' Folder

| Name          | Purpose                                                                             |
| ------------- | ----------------------------------------------------------------------------------- |
| page.tsx      | Displays the primary content for the page                                           |
| layout.tsx    | Wraps up the currently displayed page. Use to show content common across many pages |
| not-found.tsx | Display when we call the 'notFound' function                                        |
| loading.tsx   | Displayed when a server component is fetching some data                             |
| error.tsx     | Displayed when an error occurs in a server component                                |
| route.tsx     | Defines API endpoints                                                               |
