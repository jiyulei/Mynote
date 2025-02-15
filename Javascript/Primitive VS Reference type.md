#javascript
Primitive（原始类型）：
	1. String 
	2. Number
	3. Boolean
	4. Null
	5. Undefined
	6. Symbol
	7. BigInt
Reference (引用类型) ：
	1. Array
	2. Object
	3. Function
### 原始类型判断方法：
``` javascript
typeof "hello" -> "string"
typeof 123 -> "number"
typeof true -> "boolean"
typeof undefined -> "undefined"
typeof Symbol() -> "symbol"
typeof 10n -> "bigint"
```
特例： `null` `typeof null` -> `‘object’` 所以要 `value === null`判断
### 非原始类型判断方法
1. Array: `Array.isArray()`
2. Plain Object:
``` JavaScript
function isPlainObject(value) {
  return (
  //排除普通null和undefined
    value != null &&
    typeof value === 'object' && // 先排除非对象类型
    !Array.isArray(value) &&     // 再排除数组
    Object.prototype.toString.call(value) === '[object Object]'
	// 处理set map等object
	&& (Object.getPrototypeOf(value) === Object.prototype ||
	// 允许Object.create(null)
    Object.getPrototypeOf(value) === null)
  );
}
```
3. Function
``` JavaScript
typeof value === 'function'
```
4. 非null Object（Date, RegExp,）
``` JavaScript
function isObject(value) {
  return value !== null && typeof value === 'object';
}
```
  5.其他
``` JavaScript
Object.prototype.toString.call(new Date());        // "[object Date]"
Object.prototype.toString.call(/abc/);             // "[object RegExp]"
Object.prototype.toString.call(function(){});      // "[object Function]"
Object.prototype.toString.call([]);                // "[object Array]"
Object.prototype.toString.call({});                // "[object Object]"
```

****
### Comparison

| Aspect                 | Primitive types         | Reference Types          |
| ---------------------- | ----------------------- | ------------------------ |
| Storage                | in **stack** [[Memory]] | In **heap** [[Memory]]   |
| Copy behavior          | Copied by value         | Copied by reference      |
| Mutability             | Immutable               | Mutable                  |
| Behavior on assignment | Creates a new copy      | Point to the same object |
*****


