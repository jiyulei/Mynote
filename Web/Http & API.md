#http #axios

``` javascript
fetch(url, {
  method: "请求方法",
  headers: {
    "请求头": "值"
  },
  body: "请求体"
})
  .then(response => response.json())
  .then(data => console.log(data))
  .catch(error => console.error("Error:", error));
```
--------------------------------------------------------------------------------
**Method:**

| 方法     | 作用             |
| ------ | -------------- |
| GET    | 获取数据，不带 `body` |
| POST   | 发送数据到服务器       |
| PUT    | 更新数据（整个对象）     |
| PATCH  | 更新数据（部分字段）     |
| DELETE | 删除数据           |
### **什么时候用 PUT？**

- 当你需要**更新整个对象**（即使只修改了部分字段）。
- 适用于**替换**资源，例如表单提交。

### ** 什么时候用 PATCH？**

- 只想修改**部分字段**，避免发送整个对象。
- 适用于**增量更新**，例如修改用户的某个属性。
*****
**Headers:**

| Header                                              | 作用         |
| --------------------------------------------------- | ---------- |
| "Content-Type": "application/json"                  | 发送 JSON 数据 |
| "Content-Type": "application/x-www-form-urlencoded" | 发送表单数据     |
| "Authorization": "Bearer TOKEN"                     | 认证授权       |
| "Accept": "application/json"                        | 服务器返回 JSON |

---------------------------------------------------------------------------------
### 示例:
``` javascript
fetch(url, {
	method: '请求方法',
	header: {
		'Content-Type': "application/json",
	},
	body: {
		JSON.stringify({ name: "john" })
	}
})
.then(response => response.json())
.then(data => console.log(data))
.catch(error => console.error(error));

```

**Get**
``` javascript
fetch("https://api.example.com/users?name=Sophie&age=25", { method: "GET" });
```

**Post**
``` javascript
fetch("https://api.example.com/users", {
  method: "POST",
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify({ name: "Sophie", age: 25 })
});
```

**带参数的POST**
[[URLSearchParams]]
``` javascript
async function postWithQuery() {
  const queryParams = new URLSearchParams({ token: "abc123" }).toString();
  const url = `https://example.com/api/data?${queryParams}`;

  const response = await fetch(url, {
    method: "POST",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify({ username: "sophie", age: 25 })
  });

  const data = await response.json();
  console.log(data);
}

postWithQuery();
```
