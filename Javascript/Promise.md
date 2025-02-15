#javascript #promise 
Promiseall

``` js
// /**

//  * @param {Array} iterable

//  * @return {Promise<Array>}

//  */

// export default function promiseAll(iterable) {

//   return new Promise((resolve, reject) => {

//     const result = new Array(iterable.length);

//      if (iterable.length === 0) {

//       resolve(result);

//     }

//     let count = 0;

//     iterable.forEach((promise, index) => {

//       Promise.resolve(promise).then((val) => {

//         result[index] = val;

//         count++;

//         if(count === iterable.length) {

//           resolve(result);

//         }

//       }).catch((err) => reject(err));

//     })

  

//   })

// }

export default function promiseAll(iterable) {

  return new Promise((resolve, reject) => {

    const result = new Array(iterable.length);

    let count = 0;

    if (iterable.length === 0 ) {

      resolve(result);

    }

  

     iterable.forEach(async(promise, index) => {

      try {

        const value = await promise;

        result[index] = value;

        count++;

        if(count === iterable.length) {

          resolve(result);

        }

      } catch(err) {

        reject(err);

      }

    })

  })

}```