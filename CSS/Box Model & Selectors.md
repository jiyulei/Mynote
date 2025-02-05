#css
### BoxModel
--------------------------------------------------------------------
--- margin ---
  --- border ---
   --- padding ---
    --- content --- 

默认为 content-box, `width` and `height` 只应用content区域

若改为border-box , width and height 会包含 padding + border
``` css
div {
	box-sizing: border-box;
}
```

-------------------------------------------------------------------------
#### 优先级:
##### inline > ID > Class/ Attribute / PseudoClass > element / PseudoElement
### 基本选择器
1. 元素: `div {}`
2. 类: `.active {}`
3. ID: `#header {}`
**优先级: id > 类 > 元素**

### 组合选择
1. 后代 (空格) : 某元素中所有后代 `div p {}`

2. **子选择器**(>) : 某元素直接(第一代)子元素 `ul > li {}`  
  ***只有第一级被选中***
``` html
<ul>
  <li>一级子元素</li>
  <li>
    一级子元素
    <ul>
      <li>二级子元素</li>
    </ul>
  </li>
</ul>
```
3. **相邻兄弟选择**(+) : 某元素之后第一个兄弟元素 `h1 + p {}`
``` css
/* 当 h1 被鼠标悬停时，选中紧跟其后的 p 元素 */
h1:hover + p {
  color: red;
}

/* 选中紧跟在 class="title" 元素后的，且具有 data-info 属性的 p 元素 */
.title + p[data-info] {
  font-weight: bold;
}
```

4. **通用兄弟选择器**(~) : 某元素后所有同级兄弟 `h1 ~ p {}`

### 属性选择器
通过属性选择: `input[type="text"] {}` 选中所有type为text的input元素
其他用法:
1.包含: `[href*="google"]`
2.开头:`[href^="www"`
3.结尾:`[href$=".com"`

### 伪类选择器
动态伪类: `:hover` `:active` `:focus`
结构伪类: `:nth-child(n)` `:first-child` `:last-child`
``` css
a:hover { color: red; }       /* 元素选择器后跟伪类 */
#nav:focus { outline: none; }  /* ID 选择器后跟伪类 */
input:valid { border-color: green; } /* 属性选择器后跟伪类 */
```

### 伪元素选择器
`::before`   `::after`   `::first-line`   `::first-letter`
``` css
.btn::before { content: "★"; }      /* 类选择器后跟伪元素 */
a:hover::after { content: ">>"; }     /* 组合选择器中伪类后再跟伪元素 */
```
### 通用选择器
`* {}`

### 其他选择器
`:not()`
