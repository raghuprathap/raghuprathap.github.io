object shorthand property definitions

```js
var foo = [1, 2, 3];

var obj = {
  foo // means `foo: foo`
};

obj.foo; // [1,2,3]
```

## Block-Scoped Declarations

the fundamental unit of variable scoping in JavaScript has always been the `function`

If we needed to create a block of scope, other than a regular functions was immediately invoked function expression (IIFE).

```js
var a = 2;

(function IIFE() {
  var a = 3;
  console.log(a); // 3
})();

console.log(a); // 2
```

### `let` Declarations

`let` keyword allows to create block scope.

```js
var a = 2;

{
  let a = 3;
  console.log(a); // 3
}

console.log(a);
```

Just careful

```js
{
  console.log(a); // undefined
  console.log(b); // ReferenceError!

  var a;
  let b;
}
```

#### `let` + `for`

consider

```js
var funcs = [];

for (let i = 0; i < 5; i++) {
  funcs.push(function() {
    console.log(i);
  });
}

funcs[3](); // 3
```

The `let i` in the `for` header declares an `i` not just for the `for` loop itself, but it redeclares a new `i` for each iteration of the loop.

### `const` Declarations

There's another form of block scoped declaration `const`
which creates _constants_.

```js
{
  const a = 2;
  console.log(a); // 2

  a = 3; // TypeError!
}
```

A `const` variable is read-only after it's initial value is set.

Constants are not a restriction on the value itself, but on the variable's assignment of that value.
the value is not frozen or immutable because of `const`, just the assignment of it.

A const variable with object or array are still can be modified.

```js
{
  const a = [1, 2, 3];
  a.push(4);
  console.log(a); // [1,2,3,4]

  a = 42; // TypeError!
}
```

The `a` variable doesn't actually hold a constant array; rather, it holds a constant reference to the array. The array itself is freely mutable.

### Block-scoped Functions

```js
{
  foo(); // works!

  function foo() {
    // ..
  }
}

foo(); // ReferenceError
```

## Spread/Rest

ES6 introduced a new operator that's called **spread** or **rest**.

```js
function foo(x, y, z) {
  console.log(x, y, z);
}

foo(...[1, 2, 3]); // 1 2 3
```

When `...` is used in front of an array, it acts to "spread" it out into its individual values.

other example

```js
var a = [2, 3, 4];
var b = [1, ...a, 5];

console.log(b); // [1,2,3,4,5]
//same as

[1].concat(a, [5]);
```

another example

```js
function foo(x, y, ...z) {
  console.log(x, y, z);
}

foo(1, 2, 3, 4, 5);
```

The `...z` in this snippet is essentially saying: "gather the _rest_ of the arguments (if any) into an array called `z`." Because `x` was assigned `1`, and `y` was assigned `2`, the rest of the arguments `3`, `4`, and `5` were gathered into `z`.

## Default Parameter Values

The most we have been doing this

```js
function foo(x, y) {
  x = x || 11;
  y = y || 31;

  console.log(x + y);
}

foo(); // 42
foo(5, 6); // 11
foo(5); // 36
foo(null, 6); // 17
```

ES6 style of default parameters

```js
function foo(x = 11, y = 31) {
  console.log(x + y);
}

foo(); // 42
foo(5, 6); // 11
foo(0, 42); // 42

foo(5); // 36
foo(5, undefined); // 36 <-- `undefined` is missing
foo(5, null); // 5  <-- null coerces to `0`

foo(undefined, 6); // 17 <-- `undefined` is missing
foo(null, 6); // 6  <-- null coerces to `0`
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

On appending the "immutableString" with a string value, following events occur:

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

## Object literals

```js
var x = 2,
  y = 3,
  o = {
    x: x,
    y: y
  };
```

can be written

```js
var x = 2,
  y = 3,
  o = {
    x,
    y
  };
```

old way of defining methods inside object

```js
var o = {
  x: function() {
    // ..
  },
  y: function() {
    // ..
  }
};
```

using ES6

```js
var o = {
  x() {
    // ..
  },
  y() {
    // ..
  }
};
```
