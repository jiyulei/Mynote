#javascript #promise 

### Promise.all
``` js
/**
 * @param {Array} iterable
 * @return {Promise<Array>}
 */
export default function promiseAll(iterable) {
   return new Promise((resolve, reject) => {
      const result = [];
      let count = 0;

      if (iterable.length === 0) {
        resolve(result);
      }

      iterable.forEach(async (promise, index) => {
        try {
	    // 这里await会阻塞块级作用域，比如count++必须等await结束才执行
	    // 但是不会阻塞外面的forEach进行下一次循环
	    // 所以每一个promise会随机时间解决，那么就需要count来判断是否都解决了。
          result[index] = await promise;
          count++;
          if (count === iterable.length) {
            resolve(result);
          }
        } catch(err) {
          reject(err);
        }
      })
   })
}
```


### Promise.any
``` js
/**
 * @param {Array} iterable
 * @return {Promise}
 */
export default function promiseAny(iterable) {
  
   return new Promise((resolve, reject) => {
    let result;
    let errors = [];
    let errorCount = 0
    if(iterable.length === 0) {
      resolve([]);
    }

    iterable.forEach(async (promise, index) => {
      try {
        result = await promise;
        resolve(result);
      } catch(err) {
        errors[index] = err;
        errorCount++;
        if(errorCount === iterable.length) {
          reject(new AggregateError(errors));
        }
      }
    })
   })
}
```