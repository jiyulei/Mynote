#restful
### **1. 资源嵌套（Nested Resources）**
在 RESTful 设计中，**资源（Resource）可以相互关联**，常见的方式是通过**嵌套 URL 结构**表示层级关系。例如：

| 关系          | RESTful 路径               | 说明                     |
| ----------- | ------------------------ | ---------------------- |
| 获取所有公司      | `GET /companies`         | 返回所有公司                 |
| 获取单个公司      | `GET /companies/1`       | 获取 `id=1` 的公司          |
| 获取某公司下的所有用户 | `GET /companies/1/users` | 获取 `companyId=1` 的所有用户 |
| 获取单个用户      | `GET /users/23`          | 获取 `id=23` 的用户         |
**相关 REST 规范：**

- **层级结构表示资源关系**（Resource Hierarchy）
- **`/companies/:id/users` 表示“属于该公司”的用户**
- 这和 SQL 里的 **外键（Foreign Key）** 逻辑类似，但是用 REST API 的 URL 表示

### **2. 资源标识符（Resource Identifier）**

REST 规范建议：
- 资源名应使用 **复数（companies, users）**
- 资源 ID 应唯一，例如 `id=1`
- 关联字段（如 `companyId`）应和关联的资源名称一致

在 `db.json` 里：
``` json
{
  "users": [
    { "id": "23", "firstName": "Bill", "age": 20, "companyId": "1" }
  ],
  "companies": [
    { "id": "1", "name": "Apple", "description": "iphone" }
  ]
}
```

**`companyId` 之所以能匹配到 `companies/1`，是因为它和 `companies` 这个资源的 `id` 字段相对应**。如果改名为 `firmId`，就不符合 REST 资源命名最佳实践，JSON Server 也无法自动识别。

---
### 🔹 **3. 通过查询参数（Query Parameters）过滤数据**
REST API 允许使用 **查询参数（Query Parameters）** 来过滤数据：
``` bash
GET /users?companyId=1
```
这等价于：
``` sql
SELECT * FROM users WHERE companyId = 1;
```

JSON Server **内部会把 `/companies/1/users` 转换成 `GET /users?companyId=1`**，这也是它能自动匹配的原因。

**⏩ 相关 REST 规范：**

- **使用查询参数 (`?key=value`) 进行过滤**
- **`/users?companyId=1` 是标准的 RESTful 查询方式**
- JSON Server 只是利用这一规范，把 `companyId` 和 `companies/:id` 关联起来