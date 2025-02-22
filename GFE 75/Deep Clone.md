#gfe75 
[题目](https://www.greatfrontend.com/interviews/study/gfe75/questions/javascript/deep-clone)
注意事项：
	1.处理各种原始类型
	2.递归处理引用类型
``` js
/**
 * @template T
 * @param {T} value
 * @return {T}
 */
export default function deepClone(value) {
  if (value === null) {
    return null;
  }

  if (typeof value === 'boolean' || typeof value ==='number' || typeof value === 'string') {
    return value;
  }

  if (Array.isArray(value)) {
    const result = [];

    value.forEach((el) => {
      result.push(deepClone(el));
    });
    return result;
  }

  if (typeof value === 'object') {
    const result = {};

    for (let key in value) {
      result[key] = deepClone(value[key]);
    } 

    return result;   
  }
}
```