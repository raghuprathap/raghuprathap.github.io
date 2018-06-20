*A Promise is a placeholder for a future value*
 
 - Serves same purpose as callbacks but has nicer syntax and easy to handle errors.

 ## Creating a Promise

 Instantiate a promise by calling `new` On the `Promise` class.

 ```
 const promise = new Promise((resolve, reject) => {
     //resolve?, reject?
 })
 ```

Inside this inner function we perform our asynchronous processing and then when we are ready we call `resolve()`, like so:


 ```
    const promise = new Promise((resolve, reject) => {
        setTimeout(() => {
            console.log('async work completed.');
            resolve()
        }, 1000);
    })
 ```

 We usually `return` this promise from a function, like so:

```
function doAsyncTask() {
    const promise = new Promise((resolve, reject) => {
        setTimeout(() => {
            console.log('async task completed');
            resolve()
        }, 1000);
    });
    return promise;
}
```

If there was an error in the async task then we call the `reject()` function like so:

```
function doAsyncTask() {
    const promise = new Promise((resolve, reject) => {
        setTimeout(() => {
             console.log('async task completed');
            if(error) {
                reject();
            } else {
                resolve()
            }
        }, 1000);
    });
    return promise;
}
```

## Promise Notifications
We can get notified when a promise resolves by attaching a success handler to its `then` function, like so:

`doAsyncTask().then(() => console.log('Task complete'))`

`then` can take two arguments, the second argument is a `error` handler that gets called if the promise is `rejected`, like so:


```
doAsyncTask().then(() => console.log('Task completed'), () => console.log('Task Errored'))
```

Any values we pass to the `resolve` and `reject` functions are passed along to the error and success handlers, like so:

```
function doAsyncTask() {
  let error = false;
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      if (error) {
        reject("error"); // pass values
      } else {
        resolve("done"); // pass values
      }
    }, 1000);
  });
}

doAsyncTask().then(val => console.log(val), err => console.error(err));
```

##Chaining

We can also connect a series of `then` handlers together in a chain, like so:


```
const prom = Promise.resolve("done");
prom
  .then(val => {
    console.log(val);
    return "done2"; // <-- !NOTE: We have to return something, otherwise it doesn't get passed
  })
  .then(val => console.log(val));
// 'done'
// 'done2'
```
- We have to return something from each `then`, otherwise it doesn't get passed to the next `then`

```
const prom = Promise.resolve("done");
prom
  .then(val => {
    console.log(val);
  })
  .then(val => console.log(val));
// 'done'
// 'undefined'
```

- This is different to forking a promise chain

```
const prom = Promise.resolve("done");
prom.then(val => {
  console.log(val);
  return "done2";
});
prom.then(val => console.log(val)); // <-- Doesn't get passed the result of the previous then
// 'done'
// 'done'
```
We can also pause execution waiting for another promise to resolve

```
Promise.resolve("done")
  .then(val => {
    console.log(val);
    return Promise.resolve("done2"); // <-- The next then waits for this promise to resolve before continueing
  })
  .then(val => console.log(val));
```

## Error Handling
Promises pass an error along the chain till it finds an error handler. So we don't need to define an error handler for each `then` function, we can just add one at the end like so:

```
Promise.reject("fail")
  .then(val => console.log(val)) // <-- Note we dont have an error handler here!
  .then(val => console.log(val), err => console.error(err));
```

If we throw an exception from our promise function or one of the success handlers, the promise gets rejected and the error handler is called, like so:

```
new Promise((resolve, reject) => {
  throw "fail";
})
  .then(val => {
    console.log(val);
  })
  .then(val => console.log(val), err => console.error(err));
// [Error: fail]
```

```
Promise.resolve("done")
  .then(val => {
    throw "fail";
  })
  .then(val => console.log(val), err => console.error(err));
// [Error: fail]
```
The `catch` function works exactly the same way as the `then` error handler, it's just clearer and more explicitly describes our intent to handle errors.

```
Promise.resolve("done")
  .then(val => {
    throw "fail";
  })
  .then(val => console.log(val))
  .catch(err => console.error(err));
  ```

  ## Finally
  The finally() method can be useful if you want to do some processing or cleanup once the promise is settled, regardless of its outcome.

  ```
  Promise.resolve("done")
  .then(val => {
    throw new Error("fail");
  })
  .then(val => console.log(val))
  .catch(err => console.error(err))
  .finally(_ => console.log("Cleaning Up")); // <-- Comming soon!
  ```



