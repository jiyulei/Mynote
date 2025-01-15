#javascript #purefunction
### A pure function is **a function that always produces the same output for the same input and has no side effects.**

1. **No side effects:**  a pure function won’t change :
- global variables 
- Passed params
- DOM

2. Same **input** same **output**.

>eg.1 not pure:
``` javascript 
let a = 0;
function add(x) {
	a++;
	return a;
}
// different output
```
>eg2. pure:
```javascript
function add(a, b) {
	return a + b;
}
```

##### 在前端开发中，纯函数有哪些应用场景?
1. [[React function component]] (no state)
2. Reducer
3. map, filter

##### 为什么 Redux 的 reducer 必须是纯函数？
1. Redux 的状态更新依赖 [[reducer]] 返回的新状态，纯函数确保状态可预测性。
2. 保持状态不可变性([[state Immutability]])，避免副作用。

##### 纯函数如何影响 React 的性能?
1. 通过 [[React.memo]] 优化组件重新渲染
2. 减少不必要的 DOM 更新
