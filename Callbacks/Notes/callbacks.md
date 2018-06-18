# Introduction: callbacks

**Callback** is function that is to be executed after another function has finished executing.

- In javascript, functions can take functions as arguements, and functions can be returned by another functions. Any function that is passed as an arguement is called callback function.

```
function loadScript(src) {
  let script = document.createElement('script');
  script.src = src;
  document.head.append(script);
}
```

```
loadScript('/my/script.js');
newFunction(); // no such function
```

```
function loadScript(src, callback) {
  let script = document.createElement('script');
  script.src = src;

  script.onload = () => callback(script);

  document.head.append(script);
}
```

```
loadScript('/my/script.js', function() {
  // the callback runs after the script is loaded
  newFunction(); // newFunction get's called
  ...
});
```

```
function loadScript(src, callback) {
  let script = document.createElement('script');
  script.src = src;
  script.onload = () => callback(script);
  document.head.append(script);
}
```

```
loadScript('https://cdnjs.cloudflare.com/ajax/libs/lodash.js/3.2.0/lodash.js', script => {
  alert(`Cool, the ${script.src} is loaded`);
  alert( _ ); // function declared in the loaded script
});
```

