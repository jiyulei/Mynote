#javascript #api
1. **基本用法：创建查询参数**
``` javascript
const params = new URLSearchParams();
params.append("name", "Sophie");
params.append("age", "25");

console.log(params.toString()); 
// 输出: "name=Sophie&age=25"
```
2. **在 URL 里添加查询参数**
``` javascript
const baseURL = "https://example.com/api";
const params = new URLSearchParams({ search: "JavaScript", page: 2 });

const fullURL = `${baseURL}?${params.toString()}`;
console.log(fullURL); 
// 输出: "https://example.com/api?search=JavaScript&page=2"
```
3. **解析 URL 里的查询参数**
``` javascript
const url = "https://example.com/api?search=React&limit=10";
const params = new URLSearchParams(new URL(url).search);

console.log(params.get("search")); // "React"
console.log(params.get("limit"));  // "10"
```
4. **修改或删除查询参数**
``` javascript
const params = new URLSearchParams("name=Sophie&age=25");

params.set("age", "26");  // 修改参数值
params.delete("name");    // 删除参数

console.log(params.toString()); 
// 输出: "age=26"
```
5. **遍历查询参数**

``` javascript
const params = new URLSearchParams("name=Sophie&age=25&city=NY");

for (const [key, value] of params) {
  console.log(`${key}: ${value}`);
}
// 输出:
// name: Sophie
// age: 25
// city: NY
```