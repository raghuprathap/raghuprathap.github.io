# Array Iterators (map, filter, reduce)

## Array forEach

```js
function getStockSymbols(stocks) {
  let stockSymbols = [];
  stocks.forEach(stock => stockSymbols.push(stock.symbol));
  return stockSymbols;
}
var stockSymbols = getStockSymbols([
  { symbol: "infy", price: 100.0, volume: 10000 },
  { symbol: "WPR", price: 120.0, volume: 100000 },
  { symbol: "ESTI", price: 150.0, volume: 100001 }
]);
console.log(stockSymbols);
```

## Array.map

```js
function getStockSymbols(stocks) {
  return stocks.map(stock => stock.symbol);
}
var stockSymbols = getStockSymbols([
  { symbol: "infy", price: 100.0, volume: 10000 },
  { symbol: "WPR", price: 120.0, volume: 100000 },
  { symbol: "ESTI", price: 150.0, volume: 100001 }
]);
console.log(stockSymbols);

Array.prototype.map = function(fn) {
  var results = [];
  this.forEach(item => {
    results.push(fn(item));
  });
  return results;
};
```

is same as

```js
function getStockSymbols(stocks) {
  return stocks.map(stock => stock.symbol);
}
var stockSymbols = getStockSymbols([
  { symbol: "infy", price: 100.0, volume: 10000 },
  { symbol: "WPR", price: 120.0, volume: 100000 },
  { symbol: "ESTI", price: 150.0, volume: 100001 }
]);
console.log(stockSymbols);
```

## Array filter

```js
function getStocksOver(stocks, minPrice) {
  var results = [];
  stocks.forEach(stock => {
    if (stock.price > minPrice) {
      results.push(stocks);
    }
  });
  return results;
}
var expensiveStocks = getStocksOver(
  [
    { symbol: "infy", price: 100.0, volume: 10000 },
    { symbol: "WPR", price: 120.0, volume: 100000 },
    { symbol: "ESTI", price: 150.0, volume: 100001 }
  ],
  100.0
);
console.log(expensiveStocks);
```

```js
function getStocksOver(stocks, minPrice) {
  return stocks.filter(function(stock) {
    return stock.price > minPrice;
  });
}
Array.prototype.filter = function(predicate) {
  var results = [];
  this.forEach(item => {
    if (predicate(item)) {
      results.push(item);
    }
  });
  return results;
};
var expensiveStocks = getStocksOver(
  [
    { symbol: "infy", price: 100.0, volume: 10000 },
    { symbol: "WPR", price: 120.0, volume: 100000 },
    { symbol: "ESTI", price: 150.0, volume: 100001 }
  ],
  100.0
);
console.log(expensiveStocks);
```

```js
function getStocksOver(stocks, minPrice) {
  return stocks.filter(function(stock) {
    return stock.price > minPrice;
  });
}

var expensiveStocks = getStocksOver(
  [
    { symbol: "infy", price: 100.0, volume: 10000 },
    { symbol: "WPR", price: 120.0, volume: 100000 },
    { symbol: "ESTI", price: 150.0, volume: 100001 }
  ],
  100.0
);
console.log(expensiveStocks);
```

## Chaining map & filter

```js
var stocks = [
  { symbol: "infy", price: 100.0, volume: 10000 },
  { symbol: "WPR", price: 120.0, volume: 100000 },
  { symbol: "ESTI", price: 150.0, volume: 100001 }
];
var filteredStocks = stocks
  .filter(stock => {
    return stock.price > 100.0;
  })
  .map(stock => stock.symbol);

console.log(filteredStocks);
```

## array.reduce

```js
let result = arr.reduce(callback);
// Optionally, you can specify an initial value
let result = arr.reduce(callback, initValue);
```

- result — the single value that is returned.
- arr — the array to run the reduce function on.
- callback — the function to execute on each element in the array.
- initValue — an optionally supplied initial value. If this value is not supplied, the 0th element is used as the initial value.

```js
let arr = [1, 2, 3, 4];
let sum = arr.reduce((acc, val) => {
  return acc + val;
}, 100);
console.log(sum);
```

## Destructuring

ES6 introduces a new feature called destructuring

```js
function foo() {
  return [1, 2, 3];
}

var tmp = foo(),
  a = tmp[0],
  b = tmp[1],
  c = tmp[2];

console.log(a, b, c);
```

we created a manual assignment of the values in the array that foo() returns to individual variables a, b, and c, and to do so we needed the tmp variable.
ES6 destructured

similarly we can do the same thing with objects

```js
function bar() {
  return {
    x: 4,
    y: 5,
    z: 6
  };
}

var tmp = bar(),
  x = tmp.x,
  y = tmp.y,
  z = tmp.z;

console.log(x, y, z);
```

Manually assigning indexed values from an array or properties from an object can be thought of as structured assignment. ES6 adds a dedicated syntax for destructuring, specifically array destructuring and object destructuring. This syntax eliminates the need for the tmp variable in the previous snippets, making them much cleaner.

Array destructuring

```js
function foo() {
  return [1, 2, 3];
}
var [a, b, c] = foo();
console.log(a, b, c);
```

Object destructuring

```js
function bar() {
  return {
    x: 4,
    y: 5,
    z: 6
  };
}
var { x: x, y: y, z: z } = bar();
console.log(x, y, z);
```

## Spread/Rest

ES6 introduces a new `...` operator that's typically referred to as the spread or rest operator

```js
function foo(x, y, z) {
  console.log(x, y, z);
}

foo(...[1, 2, 3]);
```

When `...` is used in front of an array, it acts to "spread" it out into its individual values.

`...` can be used to spread out/expand a value in other contexts as well, such as inside another array declaration:

```js
var a = [2, 3, 4];
var b = [1, ...a, 5];

console.log(b);
```

here `...` is basically replacing concat(..), as it behaves like [1].concat( a, [5] ) here.

Other common usage is

```js
function foo(x, y, ...z) {
  console.log(x, y, z);
}

foo(1, 2, 3, 4, 5); // 1 2 [3,4,5]
```

and if we dont have any named parameters the `...` gathers all arguements.

```js
function foo(...args) {
  console.log(args);
}

foo(1, 2, 3, 4, 5); // [1,2,3,4,5]
```

## mutability and immutability

A mutable object is an object whose state can be modified after it is created. In JavaScript, only objects and arrays are mutable, not primitive values.

Immutables are the objects whose state cannot be changed once the object is created.

String and Numbers are Immutable.

```js
var immutableString = "Hello";

// In the above code, a new object with string value is created.

immutableString = immutableString + "World";

// We are now appending "World" to the existing value.
```

On appending the "immutableString" with a string value, following happening:

1.  Existing value of "immutableString" is retrieved
2.  "World" is appended to the existing value of "immutableString"
3.  The resultant value is then allocated to a new block of memory
4.  "immutableString" object now points to the newly created memory space
5.  Previously created memory space is now available for garbage collection.

why is immutability important? Mutating data can produce code that’s hard to read and error prone. For primitive values (like numbers and strings), it is pretty easy to write ‘immutable’ code, because primitive values cannot be mutated themselves. Variables containing primitive types always point to the actual value. If you pass it to another variable, the other variable get’s a fresh copy of that value.

Objects (and arrays) are a different story, they are passed by reference. This means that if you would pass an object to another variable, they will both refer to the same object. If you would then mutate the object from either variable, they will both reflect the changes. Example:

```js
const car = {
  company: "volvo",
  model: "s60"
};
const newCar = car;
newCar.model = "s90";
console.log(newCar === car); // true
console.log(car); // { name: 'volvo', model: s90 }
```

### Going immutable

Instead of passing the object and mutating it, we will be better off creating a completely new object:

```js
const car = {
  company: "volvo",
  model: "s60"
};
const newCar = Object.assign({}, car, {
  model: "s90"
});
console.log(newCar === car);
console.log(car);
console.log(newCar);
```

immutable with object spread

```js
const car = {
  company: "volvo",
  model: "s60"
};
const newCar = {
  ...car,
  model: "s90"
};
console.log(newCar === car);
console.log(car);
console.log(newCar);
```

the `spread` operator (...), copies all the properties from car object to the new object.then we define new model property that overrides old one.

### Mutating arrays

```js
const characters = ["Obi-Wan", "Vader"];
const newCharacters = characters;
newCharacters.push("Luke");
console.log(characters === newCharacters); // true
```

using ES6 `spread` operator for arrays

```js
const characters = ["Obi-Wan", "Vader"];
const newCharacters = [...characters, "Luke"];
console.log(characters === newCharacters); // false
console.log(characters); // [ 'Obi-Wan', 'Vader' ]
console.log(newCharacters); // [ 'Obi-Wan', 'Vader', 'Luke' ]
```

Let's see some other operations on arrays with out mutating original one.

```js
const characters = ["Obi-Wan", "Vader", "Luke"];
// Removing Vader
const withoutVader = characters.filter(char => char !== "Vader");
console.log(withoutVader); // [ 'Obi-Wan', 'Luke' ]
// Changing Vader to Anakin
const backInTime = characters.map(char => (char === "Vader" ? "Anakin" : char));
console.log(backInTime); // [ 'Obi-Wan', 'Anakin', 'Luke' ]
// All characters uppercase
const shoutOut = characters.map(char => char.toUpperCase());
console.log(shoutOut); // [ 'OBI-WAN', 'VADER', 'LUKE' ]
// Merging two character sets
const otherCharacters = ["Yoda", "Finn"];
const moreCharacters = [...characters, ...otherCharacters];
console.log(moreCharacters); // [ 'Obi-Wan', 'Vader', 'Luke', 'Yoda', 'Finn' ]
```

## Performance

Isn’t creating new objects time and memory consuming?
But that disadvantage is very small compared to the advantages.
`Object.observe(object, callback)` is very heavy. If we keep our state immutable `object === newObject` would do.

second advantage this thinking encourages to programming in a more functional way.

## EventEmitter

All objects that emit events are members of EventEmitter class. These objects expose an eventEmitter.on() function that allows one or more functions to be attached to named events emitted by the object.

When the EventEmitter object emits an event, all of the functions attached to that specific event are called synchronously.

```js
const EventEmitter = require("events");
class MyEmitter extends EventEmitter {}
const myEmitter = new MyEmitter();
myEmitter.on("event", function(a, b) {
  console.log(a, b, this);
  // Prints:
  //   Technoetics Club MyEmitter {
  //     domain: null,
  //     _events: { event: [Function] },
  //     _eventsCount: 1,
  //     _maxListeners: undefined }
});
myEmitter.emit("event", "Technoetics", "Club");
```

Here we create a myEmitter object and emit event at the end which triggers the callback function
