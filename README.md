# <img src="http://i.imgur.com/yy1sACZ.png" width="100px"/> ECMAScript 6 equivalents in ES5

*Please note this document is very much a work in progress. Contributions are welcome.*

## Arrow Functions

An arrow function expression (also known as fat arrow function) has a shorter syntax compared to function expressions
and lexically binds the this value. Arrow functions are always anonymous.

ES6:

```js
[1, 2, 3].map(n => n * 2);
```

ES5 equivalent:

```js
[1, 2, 3].map(function(n) { return n * 2; });
```

ES6:

```js
// Expression bodies
var odds = evens.map(v => v + 1);
var nums = evens.map((v, i) => v + i);

// Statement bodies
nums.forEach(v => {
    if (v % 5 === 0)
        fives.push(v);
});

// Lexical this
var bob = {
    _name: "Bob",
    _friends: [],
    printFriends() {
        this._friends.forEach(f =>
            console.log(this._name + " knows " + f));
    }
}
```

ES5:

```js
"use strict";

// Expression bodies
var odds = evens.map(function (v) {
  return v + 1;
});
var nums = evens.map(function (v, i) {
  return v + i;
});

// Statement bodies
nums.forEach(function (v) {
  if (v % 5 === 0) fives.push(v);
});

// Lexical this
var bob = {
  _name: "Bob",
  _friends: [],
  printFriends: function printFriends() {
    var _this = this;
    this._friends.forEach(function (f) {
      return console.log(_this._name + " knows " + f);
    });
  }
};
```

## Block Scoping Functions

Block scoped bindings provide scopes other than the function and top level scope. This ensures your variables don't
leak out of the scope they're defined:

ES6:

```js
// let declares a block scope local variable,
// optionally initializing it to a value in ES6

"use strict";

var a = 5;
var b = 10;

if (a === 5) {
    let a = 4; // The scope is inside the if-block
    var b = 1; // The scope is inside the function

    console.log(a);  // 4
    console.log(b);  // 1
}

console.log(a); // 5
console.log(b); // 1
```

ES5:

```js
"use strict";

var a = 5;
var b = 10;

if (a === 5) {
    var _a = 4;
    var b = 1;

    console.log(_a); // 4
    console.log(b); // 1
}

console.log(a); // 5
console.log(b); // 1
```


ES6:

```js
// const creates a read-only named constant in ES6.
"use strict";
// define favorite as a constant and give it the value 7
const favorite = 7;
// Attempt to overwrite the constant
try {
    favorite = 15;
} catch (e) {
    console.log("my favorite number is still: " + favorite);
    // will still print 7
}
```

ES5:

```js
"use strict";
// define favorite as a non-writable "constant" and give it the value 7
Object.defineProperties(window, {
  "favorite": { value: 7, writable: false }
});
var favorite = 7;
// Attempt to overwrite the constant
favorite = 15;
// will still print 7
console.log("my favorite number is still: " + favorite);
```

## Template Literals

ES6 Template Literals are strings that can include <strong>embedded expressions</strong>. This is sometimes referred
to as string interoplation.

ES6:

```js
// Basic usage with an expression placeholder
var person = 'Addy Osmani';
console.log(`Yo! My name is ${person}!`);

// Expressions work just as well with object literals
var user = {name: 'Caitlin Potter'};
console.log(`Thanks for getting this into V8, ${user.name}.`);

// Expression interpolation. One use is readable inline math.
var a = 50;
var b = 100;
console.log(`The number of JS frameworks is ${a + b} and not ${2 * a + b}.`);

// Multi-line strings without needing \n\
console.log(`string text line 1
string text line 2`);

// Functions inside expressions
function fn() { return "result"; }
console.log(`foo ${fn()} bar`);
```

ES5:

```js
"use strict";

// Basic usage with an expression placeholder
var person = "Addy Osmani";
console.log("Yo! My name is " + person + "!");

// Expressions work just as well with object literals
var user = { name: "Caitlin Potter" };
console.log("Thanks for getting this into V8, " + user.name + ".");

// Expression interpolation. One use is readable inline math.
var a = 50;
var b = 100;
console.log("The number of JS frameworks is " + (a + b) + " and not " + (2 * a + b) + ".");

// Multi-line strings:
console.log("string text line 1\nstring text line 2");

// Functions inside expressions
function fn() {
    return "result";
}
console.log("foo " + fn() + " bar");
```


## Computed Property Names

Computed property names allow you to specify properties in object literals based on expressions:

ES6:

```js
var prefix = "foo";
var myObject = {
    [prefix + "bar"]: "hello",
    [prefix + "baz"]: "world"
};

console.log(myObject["foobar"]);
console.log(myObject["foobaz"]);
```

ES5:

```js
"use strict";

var prefix = "foo";
var myObject = {};

myObject[prefix + "bar"] = "hello";
myObject[prefix + "baz"] = "world";

console.log(myObject["foobar"]);
console.log(myObject["foobaz"]);
```

## Destructuring Assignment

The destructuring assignment syntax is a JavaScript expression that makes it possible to extract data from arrays
or objects using a syntax that mirrors the construction of array and object literals.


ES6:

```js
var {foo, bar} = {foo: "lorem", bar: "ipsum"};
// foo => lorem and bar => ipsum
```

ES5:

```js
"use strict";

var _ref = { foo: "lorem", bar: "ipsum" };

// foo => lorem and bar => ipsum
var foo = _ref.foo;
var bar = _ref.bar;
```

ES6:

```js
var [a, , b] = [1,2,3];
```

ES5:

```js
"use strict";

var _slicedToArray = function (arr, i) {
    if (Array.isArray(arr)) {
        return arr;
    } else {
        var _arr = [];

        for (var _iterator = arr[Symbol.iterator](), _step; !(_step = _iterator.next()).done;) {
            _arr.push(_step.value);

            if (i && _arr.length === i) break;
        }

        return _arr;
    }
};

var _ref = [1, 2, 3];

var _ref2 = _slicedToArray(_ref, 3);

var a = _ref2[0];
var b = _ref2[2];
```


## Default Parameters

Default parameters allow your functions to have optional arguments without needing to check arguments.length or
check for undefined.

ES6:

```js
function greet(msg="hello",name="world"){
    console.log(msg,name);
}
```

ES5:

```js
"use strict";

function greet() {
    var msg = arguments[0] === undefined ? "hello" : arguments[0];
    var name = arguments[1] === undefined ? "world" : arguments[1];
    console.log(msg, name);
}
```

ES6:

```js
function f(x, y=12) {
  // y is 12 if not passed (or passed as undefined)
  return x + y;
}
f(3) == 15
```

ES5:

```js
"use strict";

function f(x) {
  var y = arguments[1] === undefined ? 12 : arguments[1];
  // y is 12 if not passed (or passed as undefined)
  return x + y;
}
f(3) == 15;
```

## Iterators And For-Of

Iterators are objects that can traverse a container. It's a useful way to make a class work inside a for of loop.
The interface is similar to the iterators-interface. Iterating with a for of loop looks like:

ES6:

```js
for (let element of [1, 2, 3]) {
    console.log(element);
}
```

ES5:

```js
"use strict";

for (var _iterator = [1, 2, 3][Symbol.iterator](), _step; !(_step = _iterator.next()).done;) {
    var element = _step.value;
    console.log(element);
}
```

Note the use of `Symbol`. The ES5 equivalent would require a Symbol polyfill in order to correctly function.

## Classes

This implements class syntax and semantics as described in the ES6 draft spec. Classes are a great way to reuse code.
Several JS libraries provide classes and inheritance, but they aren't mutually compatible.

ES6:

```js
class Hello {
  constructor(name) {
    this.name = name;
  }

  hello() {
    return "Hello " + this.name + "!";
  }
}

class HelloWorld extends Hello {
  constructor() {
    super("World");
  }

  echo() {
    alert(super.hello());
  }
}

var hw = new HelloWorld();
hw.echo();
```

ES5:

```js
var Hello = function() {
  var Hello = function Hello(name) {
    this.name = name;
  };

  Hello.prototype.hello = function hello() {
    return "Hello " + this.name + "!";
  };

  return Hello;
}();

var HelloWorld = function() {
  var HelloWorld = function HelloWorld() {
    Hello.call(this, "World");
  };

  var HelloWorldPrototype = function() {};
  HelloWorldPrototype.prototype = Hello.prototype;
  HelloWorld.prototype = new HelloWorldPrototype();
  HelloWorld.prototype.constructor = HelloWorld;

  HelloWorld.prototype.echo = function echo() {
    alert(Hello.prototype.hello.call(this));
  };

  return HelloWorld;
}();

var hw = new HelloWorld();
hw.echo();
```

## Modules

Modules are mostly implemented, with some parts of the Loader API still to be corrected. Modules try to solve many
issues in dependencies and deployment, allowing users to create modules with explicit exports, import specific
exported names from those modules, and keep these names separate.


app.js - ES6

```js
import math from "lib/math";
console.log("2π = " + math.sum(math.pi, math.pi));
```

app.js - ES5

```js
"use strict";

var _interopRequire = function (obj) {
  return obj && (obj["default"] || obj);
};

var math = _interopRequire(require("lib/math"));

console.log("2π = " + math.sum(math.pi, math.pi));
```

lib/math.js - ES6

```js
export function sum(x, y) {
  return x + y;
}
export var pi = 3.141593;
```

lib/math/js - ES5

```js
"use strict";

exports.sum = sum;
function sum(x, y) {
  return x + y;
}
var pi = exports.pi = 3.141593;
```

lib/mathplusplus.js - ES6

```js
export * from "lib/math";
export var e = 2.71828182846;
export default function(x) {
    return Math.exp(x);
}
```

lib/mathplusplus.js - ES5

```js
"use strict";

var _extends = function (target) {
  for (var i = 1; i < arguments.length; i++) {
    var source = arguments[i];
    for (var key in source) {
      target[key] = source[key];
    }
  }

  return target;
};

var _interopRequireWildcard = function (obj) {
  return obj && obj.constructor === Object ? obj : {
    "default": obj
  };
};

var _exportsWildcard = function (obj) {
  for (var i in obj) {
    if (exports[i] !== undefined) {
      exports[i] = obj[i];
    }
  }
};

_exportsWildcard(_interopRequireWildcard(require("lib/math")));

var e = exports.e = 2.71828182846;
exports["default"] = function (x) {
  return Math.exp(x);
};

module.exports = _extends(exports["default"], exports);
```

## Numeric Literals

ES6:

```js
var binary = [
  0b0,
  0b1,
  0b11
];
console.assert(binary === [0, 1, 3]);

var octal = [
  0o0,
  0o1,
  0o10,
  0o77
];
console.assert(octal === [0, 1, 8, 63]);
```

ES5:

```js
"use strict";

var binary = [0, 1, 3];
console.assert(binary === [0, 1, 3]);

var octal = [0, 1, 8, 63];
console.assert(octal === [0, 1, 8, 63]);
```

## Property Method Assignment

Method syntax is supported in object initializers, for example see toString():

ES6:

```js
var object = {
  value: 42,
  toString() {
    return this.value;
  }
};
console.assert(object.toString() === 42);
```

ES5:

```js
"use strict";

var object = {
  value: 42,
  toString: function toString() {
    return this.value;
  }
};
console.assert(object.toString() === 42);
```

## Object Initializer Shorthand

This allows you to skip repeating yourself when the property name and property value are the same in an object literal.

ES6:

```js
function getPoint() {
    var x = 1;
    var y = 10;

    return {x, y};
}
expect(getPoint()).to.be.eql({
    x: 1,
    y: 10
});
```

ES5:

```js
"use strict";

function getPoint() {
    var x = 1;
    var y = 10;

    return { x: x, y: y };
}
expect(getPoint()).to.be.eql({
    x: 1,
    y: 10
});
```

## Rest Parameters

Rest parameters allows your functions to have variable number of arguments without using the arguments object.
The rest parameter is an instance of Array so all the array methods just work.

ES6:

```js
function f(x, ...y) {
    // y is an Array
    return x * y.length;
}
console.assert(f(3, "hello", true) == 6);
```

ES5:

```js
"use strict";

function f(x) {
    var y = [];

    for (var _key = 1; _key < arguments.length; _key++) {
        y[_key - 1] = arguments[_key];
    }

    // y is an Array
    return x * y.length;
}
console.assert(f(3, "hello", true) == 6);
```

## Spread Operator

The spread operator is like the reverse of rest parameters. It allows you to expand an array into multiple formal parameters.

ES6:

```js
function add(a, b) {
    return a + b;
}

let nums = [5, 4];

console.log(add(...nums));
```

ES5:

```js
"use strict";

var _toArray = function (arr) {
    return Array.isArray(arr) ? arr : Array.from(arr);
};

function add(a, b) {
    return a + b;
}

var nums = [5, 4];

console.log(add.apply(undefined, _toArray(nums)));
```

ES6:

```js
function f(x, y, z) {
  return x + y + z;
}
// Pass each elem of array as argument
f(...[1,2,3]) == 6
```

ES5:

```js
"use strict";

function f(x, y, z) {
  return x + y + z;
}
// Pass each elem of array as argument
f.apply(undefined, [1, 2, 3]) == 6;
```

## Proxying a function object

ES6:

```js
var target = function () { return 'I am the target'; };
var handler = {
    apply: function (receiver, ...args) {
    return 'I am the proxy';
}
};

var p = new Proxy(target, handler);
console.assert(p() === 'I am the proxy');
```

ES5:

```js
"use strict";
var target = function () {
    return "I am the target";
};
var handler = {
    apply: function (receiver) {
        var args = [];

        for (var _key = 1; _key < arguments.length; _key++) {
            args[_key - 1] = arguments[_key];
        }

        return "I am the proxy";
    }
};

var p = new Proxy(target, handler);
console.assert(p() === "I am the proxy");
```

## About

Inspired by:

* [ES6 Feature Proposals](http://tc39wiki.calculist.org/es6/)
* [ES6 Features](https://github.com/lukehoban/es6features)
* [ECMAScript 6 support in Mozilla](https://developer.mozilla.org/en-US/docs/Web/JavaScript/New_in_JavaScript/ECMAScript_6_support_in_Mozilla)
* [6to5](https://6to5.org)

## License

This work is licensed under a [Creative Commons Attribution 4.0 International](http://creativecommons.org/licenses/by/4.0/) License.
