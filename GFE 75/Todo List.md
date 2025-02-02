#gfe75 
[题目](https://www.greatfrontend.com/interviews/study/gfe75/questions/user-interface/todo-list)
注意事项： 
 - 不添加空字符 trim（）
 - 每一项有一个unique id，因为要render list
 - 更新id的逻辑

``` javascript
import { useState } from 'react';

export default function App() {
  const [id, setId] = useState(3);
  // Lazy Initialization 
  // 初始化一次，要不每一次渲染都会赋值 即使没有用到，
  // 如果有大型数据计算比如call api会优化
  const [tasks, setTasks] = useState(() => [
  { id: 0, label: 'Walk the dog'},
  { id: 1, label: 'Water the plants'},
  { id: 2, label: 'Wash the dishes'}]);

  const [inputValue, setInputValue] = useState('');

  const handleOnClick = () => {
    if(inputValue.trim() === "") {
      return;
    }
    const newId = id + 1;
    setTasks([...tasks, {id: newId, label: inputValue}]);
    setId(newId);
    setInputValue("");
  }
  const handleDelete = (id) => {
    setTasks(tasks.filter((el) => el.id !== id));
  }

  return (
    <div>
      <h1>Todo List</h1>
      <div>
        <input 
		  aria-label="Add your task"
          type="text" 
          placeholder="Add your task" 
          value={inputValue}
          onChange={(event) => {setInputValue(event.target.value)}}
        />
        <div>
          <button onClick={handleOnClick}>Submit</button>
        </div>
      </div>
      <ul>
        {tasks.map((task) => (
          <li key={task.id}>
            <span>{task.label}</span>
            <button onClick={()=>{handleDelete(task.id)}}>Delete</button>
          </li>
    ))}
      </ul>
    </div>
  );
}

```

