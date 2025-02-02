#gfe75 
[题目](https://www.greatfrontend.com/interviews/study/gfe75/questions/user-interface/holy-grail/react)
https://css-tricks.com/snippets/css/a-guide-to-flexbox/
注意事项：
	1.`body` 和 `#root`要成撑满viewport height
	2. `flex` 方向， `flex-grow`用法

``` css
body {
  font-family: sans-serif;
  font-size: 12px;
  font-weight: bold;
  margin: 0;
  height: 100vh;
}

#root {
  height: 100vh;
  display: flex;
  flex-direction: column;
}

* {
  box-sizing: border-box;
}

header,
nav,
main,
aside,
footer {
  padding: 12px;
  text-align: center;
}

.container {
  display: flex;
  flex-grow: 1;
}

header {
  background-color: tomato;
  height: 60px;
}

nav {
  background-color: coral;
  width: 100px;
}

main {
  background-color: moccasin;
  flex-grow: 1;
}

aside {
  background-color: sandybrown;
    width: 100px;

}

footer {
  background-color: slategray;
  height: 100px;
}

```