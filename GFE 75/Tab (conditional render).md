#gfe75 
[题目](https://www.greatfrontend.com/interviews/study/gfe75/questions/user-interface/tabs)
注意事项：

面试时候先问需要controlled还是uncontrolled component

1. inline css `style={{'color' : tab === 'html' ? 'blue' : 'black'}}>`
2. conditional render`（cond && <>）`
3. css的优先级别  内联样式 (`style=""`) > ID 选择器 (`#id`) > 类选择器 (`.class`) > 元素选择器 (`button`)  !important > 内联样式 > ID 选择器 > 类选择器/伪类 > 元素选择器
4. css 变量 伪类 级联
5.  map render一定不要忘了key ， map用小括号不用写return

``` javascript
import { useState } from 'react';
import "./styles.css";

export default function Tabs({ items }) {
  const [tab, setTab] = useState(items[0].value);
  return (
    <div>
      <div className='buttonGroup'>
        {items.map((item) => (
          <button 
            key={item.value} 
            className={`button ${item.value === tab ? 'buttonactive' : 'button'}`}
            onClick={() => setTab(item.value)}
          >
            {item.label}
          </button>
        ))}
      </div>
      <div>
        {items.map((item) => (
          item.value === tab && 
            <p key={item.value}>
              {item.content}
            </p>
        ))}
      </div>
    </div>
  );
}
```

``` css
body {
  font-family: sans-serif;
}

.buttonGroup {
  display: flex;
  flex-direction: row;
  gap: 4px;
}

.button {
  --hoverColor: blueviolet;
  cursor: pointer;
  border: 1px solid black;
  border-radius: 4px;
  padding: 6px 10px;
}

 .button:hover {
  color: var(--hoverColor);
  border-color: var(--hoverColor);
}

.buttonactive,
.buttonactive:hover {
  color: white;
  background-color: blueviolet;
} 

```