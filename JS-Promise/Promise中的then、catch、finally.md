# Promise中的then|catch|finally

## 题目一

```js
  const promise = new Promise((resolve, reject) => {
    resolve("success1");
    reject("error");
    resolve("success2");
  });
  promise
  .then(res => {
      console.log("then: ", res);
    }).catch(err => {
      console.log("catch: ", err);
    })
// then: success1
```

## 题目二 （*）

- catch不管被连接到哪里，都能捕获上层未捕捉过的错误
- catch()也会返回一个Promise，且由于这个Promise没有返回值，所以打印出来的是undefined

```js
  const promise = new Promise((resolve, reject) => {
    reject("error");
    resolve("success2");
  });
  promise
  .then(res => {
      console.log("then1: ", res);
    }).then(res => {
      console.log("then2: ", res);
    }).catch(err => {
      console.log("catch: ", err);
    }).then(res => {
      console.log("then3: ", res);
    })


//  catch: error
// then3: undefined
```

## 题目三 (*)

```js
  Promise.resolve(1)
    .then(res => {
      console.log(res);
      return 2;
    })
    .catch(err => {
      return 3;
    })
    .then(res => {
      console.log(res);
    });


// 1, 2
```

## 题目四

```js
Promise.reject(1)
  .then(res => {
    console.log(res);
    return 2;
  })
  .catch(err => {
    console.log(err);
    return 3
  })
  .then(res => {
    console.log(res);
  });

  // 1, 3
```

## 题目五

```js
  const promise = new Promise((resolve, reject) => {
    setTimeout(() => {
      console.log('timer')
      resolve('success')
    }, 1000)
  })
  const start = Date.now();
  promise.then(res => {
    console.log(res, Date.now() - start)
  })
  promise.then(res => {
    console.log(res, Date.now() - start)
  })

// start是固定值  res是success 先打印 timer 
```

## 题目六

> .then 或者 .catch 中 return 一个 error 对象并不会抛出错误，所以不会被后续的 .catch 捕获

```js
  Promise.resolve().then(() => {
    return new Error('error!!!')
  }).then(res => {
    console.log("then: ", res)
  }).catch(err => {
    console.log("catch: ", err)
  })

// then:  Error: error!!!
```

如果你抛出一个错误的话，可以用下面👇两的任意一种：

```js
  return Promise.reject(new Error('error!!!'));
  // or
  throw new Error('error!!!')
```

## 题目七(*)

```js
Promise.resolve(1)
  .then(2)
  .then(Promise.resolve(3))
  .then(console.log)
```

- .then 或 .catch 的参数应该是一个函数。
- 如果参数不是函数，则此 .then 或 .catch 将被忽略。

```js
// 1
```
