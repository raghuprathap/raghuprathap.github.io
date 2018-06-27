**async function** declaration defines an _asynchronous_ function which returns an AsyncFunction object.

```js
async function f() {
  return 1;
}

f().then(console.log); // 1
```

```js
async function f() {
  return Promise.resolve(1);
}
f().then(conole.log);
```

So, async ensures that the function returns a promise.
But await keyword only works inside an async function.

The await operator is used to wait for a Promise. It can only be used inside an async function.

```js
async function f() {
  let promise = new Promise((resolve, reject) => {
    setTimeout(() => resolve("done!"), 1000);
  });

  let result = await promise; // wait till the promise resolves (*)

  alert(result); // "done!"
}

f();
```

The function execution “pauses” at the line (\*) and resumes when the promise settles, with result becoming its result. So the code above shows “done!” in one second.

await literally makes JavaScript wait until the promise settles, and then go on with the result. That doesn’t cost any CPU resources, because the engine can do other jobs meanwhile: execute other scripts, handle events etc.

#### Warning

Can’t use await in regular functions

```js
function f() {
  let promise = Promise.resolve(1);
  let result = await promise; // Syntax error
}
```

```js
function resolveAfter2Seconds() {
  return new Promise(resolve => {
    setTimeout(() => {
      resolve("resolved");
    }, 2000);
  });
}

async function asyncCall() {
  console.log("calling");
  var result = await resolveAfter2Seconds();
  console.log(result);
  // expected output: "resolved"
}

asyncCall();
```
