#hooks #react
### 目的：
处理副作用 保持react component pure
### 用法: 
``` javascript
useEffect(() => {
//副作用逻辑

return () => {
	//副作用清理（可选）
	// 相当于 componentWillUnmount 执行
}
}, [依赖项])
```
### 三种不同用法
``` javascript
// 每一次render都执行
useEffect(() => {
	
})
// 组件挂载之后执行一次（componentDidMount）
useEffect(() => {

}, [])
// state改变时执行
useEffect(() => {

}, [state])
```

需要理解的是第一个参数是一个回调函数，所以它被执行的时刻时在组件初始实例化（挂载）之后（componoentDidMount）

### 面试问题
1. 如果组件需要定期更新一些状态（比如每秒刷新时间），该如何使用 useEffect 实现？
解决方案：
使用 setInterval 定时更新状态，同时在 useEffect 的清理函数中清理定时器。
示例：每秒更新时间
``` javascript

function Clock() {
  const [time, setTime] = useState(new Date());

  useEffect(() => {
    const interval = setInterval(() => {
      setTime(new Date());
    }, 1000);

    return () => {
      clearInterval(interval); // 清理定时器
    };
  }, []); // 空依赖数组，定时器只在挂载时设置

  return <div>当前时间：{time.toLocaleTimeString()}</div>;
}
```
 初始化时打印一次时间，挂载后执行setInterval， 1000ms后更新了state，触发下一次渲染，打印新的date，1000ms后更新，再次渲染打印，循环。。。， 直到组件要卸载时，执行定时器清理。

2. **useEffect** 的 return () => {} 中的**清理回调**会在以下情况执行：

-  **组件卸载时**：当组件从页面上消失、从 DOM 树中被移除时，React 会调用清理回调函数。
- **依赖项发生变化时**：如果 useEffect 有依赖数组 [dependency]，当依赖项的值发生变化时，React 会先执行清理回调，然后再运行新的副作用逻辑。

3. useEffect处理异步函数：
	useEffect 不能直接返回异步函数。
	``` javascript
useEffect(() => {
	const fetchData = async () => {
		try {
			const response = await fetch(`http://xxxx.com/getUser`);
			if (!response.ok) {
				throw new Error(`error xxxxx`);
			}
			const data = await response.json();
			setData(data);
		} catch(error) {
			console.log(error); 
		}
	};

	fetchData();
}, []);

// 由参数的话需要添加依赖项防止取来错误的数据
useEffect(() => {
  const fetchData = async () => {
    const response = await fetch(`https://api.example.com/data/${id}`);
    const data = await response.json();
    setData(data);
  };

  fetchData();
}, [id]); // id 变化时重新执行
  ```


