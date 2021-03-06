# JavaScript Recap

#### Semi-colons

- Expression - line of code that returns a value
- Statement - any line of code

Every expression needs a semi colon!
```Javascript
let x = 5;
```

Function declarations are statements...
```Javascript
function() {}
```

#### Curly Braces

Used to precisely delineate code blocks (ex: function definitions, loops, if blocks).
Also used to define JS objects:
```JavaScript
let object = { key: "value" };
```

## Data Types

#### 1. Primitives --- Numbers, Strings, Booleans, Undefined, Null
- NOT objects!
- Cannot have object methods

#### 2. Objects
- Can store state and behavior
- state/behavior treated the same way (**properties** of object)

##### Falsey values
  - 0, "", undefined, null, NaN

## Variables
Must be declared to introduce to environment (unless global..)

### `var`
- functionally-scoped
- not preferred in ES6

### `let`
- block-scoped
- will raise syntax error if declared twice in one block

### `const`
- used to declare constants that will not be re-declared/re-assigned
- block-scoped like `let`
  - will raise error if re-assignment is attempted in same scope
  - can declare same name constant in nested scope
- will raise error if declaring `var`/`let` with same name
- not immutable (constant objects can still be modified)

### `global` and `window`
- not declaring a variable will make it global. **This is bad.**


## Functions

#### Function-style
```JavaScript
function myFunction(arg1, arg2, arg3, argN) {}
```

#### Expression-style
```JavaScript
const myFunction = function (arg1, arg2, arg3, argN) {};
```

#### Arrow-style (ES6+)
```JavaScript
const myFunction = (arg1, arg2, arg3, argN) => {};
```
###### Misc
- referencing functions without `()` will return a pointer to function
- always use implicit returns!
  - exception: one-liner arrow functions
- **callback** --- a function passed as argument to another function

#### Anonymous function:
  - dynamically delcared at runtime
  - declared using the function operator

```JavaScript
[2, 4, 6].forEach(function (num) {
  if (condition) { doSomething; }
});
```

## Scope
- set of variables available for use within a method:
  - function's arguments
  - any local variables declared inside function
  - any variables already declared when function was defined

## Closures
- Functions that capture free variables
- free variables can be modified by closures
- used to pass down arguments to helper functions without listing them as arguments
- used to create private states

# `this`
- similar to ruby's `self`
- must be used to access attributes of object
- calling a function using function-style will set `this` to global scope

### `this` and passing callbacks:

**Don't do this:**
```JavaScript
function(arg, object.method);
```

**Do this instead:**

1. Use anonymous functions to capture object:
```JavaScript
function(arg, function () {
  object.method();
});
```

2. Use `Function.prototype.bind`:
```JavaScript
function(arg, object.method.bind(object))
```
`bind` can be used to bind functions to any scope:

  ```JavaScript
  function(arg, object.method.bind(differentObject))
  ```

When nesting, store the object in a capturable local variable.


## Arrow Functions (ES6+)

```JavaScript
// Syntax:
(arg1, arg2) => {
  statement;
  return value;
}
```
- another way of declaring functions
- used to solve inconveniences in normal callback function syntax

- `{ }` and `return` are implied in **single-expression** blocks and `( )` may be omitted:
```JavaScript
argument => expression; === (argument) => { return expression };
```
- arrow functions do not create new scope, unlike normal functions
- must explicitly return when using a block
- cannot reassign `this`

- empty objects need to be wrapped in parentheses, otherwise JS will default to empty block:
```JavaScript
const function = () => ({});
```

- cannot be used as constructors
- arrow functions are anonymous. assign to a variable to name it:
```JavaScript
const myFunc = (something) => console.log(`${something}`);
```


## Arguments

JS functions can take fewer/extra arguments than specified. Unspecified arguments will have an undefined value. Extra arguments will be placed in the `arguments` array-like object.

`Array.from(arguments)` can be used to place extra args into a true array. (ES6+)

- Rest Parameters (ES6+):
  - the Rest Operator `...` will capture arguments into actual array
  - `arguments` vs rest params:
    - rest params only grab un-named args
    - rest params give back a real array (allows for array methods)


- Spread (ES6+):
  - use `...` to pass a 'spread' array to a function: `function(...args);`


- Default values (ES6+);
  - set default values in declaration: `function example(arg = 45) {...`


## Calling functions

- function-stye will set `this` to global context
- method-stye will set `this` to the object
- constructor-style `new ClassName(arg1, arg2)`:
  - creates new, blank object
  - sets `__proto__` property to `ClassName.prototype`
  - called with `this` to set blank object
    - sets up key-value pairs (instance variables)
    - return value is ignored

#### Apply and Call
`Function.prototype.apply` takes 2 args:
  1. an object to bind to `this` (arg1)
  2. array of arguments ( [arg2] )

`Function.prototype.call`:
  1. an object to bind to `this` (arg1)
  2. individual arguments (arg2, arg3, arg4, etc....)




## Asynchronous functions

Used for executing asynchronous requests. Asynchronous functions do not wait for work to be completed. schedules work to be done in background. used when work may take a varying amount of time. Examples: timers, background web requests, events.

##### Call Stack
A stack data structure that stores information about the active subroutines of a computer program.


##### Event Loop
A queue of callback functions. When an async function executes, the callback function is pushed into the queue. The JavaScript engine doesn't start processing the event loop until the code after an async function has executed.


# Object Oriented Programming

### Constructor functions

```JavaScript
function Dog(name, breed) {
  this.name = name;
  this.breed = breed;
}
```
- Attributes are persisted inside the constructor using `this.key = value`

- Invoke the function with `new`, setting `this` to a new blank object that will be returned by constructor as a new `Dog` instance:
```JavaScript
let max = new Dog('Max', 'Golden Retreiver');
```

### Instance methods/prototype
- Use the constructor's `prototype` to set instance methods on the object
- Any new object of `Dog` will have the property `dog.__proto__` set to `Dog.prototype`
```JavaScript
Dog.prototype.speak = function () {
    console.log(`My name is ${this.name}. Woof!!!`);
};
```

## Classes in ES6+
```JavaScript
class Spaceship {
  constructor(shipClass, name, captain) {
    this.shipClass = shipClass
    this.name = name
    this.captain = captain
  }

  ready() {
    return "Warp engines are online!";
  }

  engage() {
    console.log(
      `Ensign: ${this.ready()} Captain ${this.captain}: Engage!`
    );
  }

  static moveFleet() {
    Spaceship.allShips.forEach(ship => ship.engage());
  }
}


const enterprise1 = new Spaceship('Constitution', 'USS Enterprise NCC-1701', 'Kirk');
const enterprise2 = new Spaceship('Galaxy', 'USS Enterprise NCC-1701-D', 'Picard');

Spaceship.allShips = [enterprise1, enterprise2]

Spaceship.moveFleet();
// Ensign: Warp engines are online! Captain Kirk: Engage!
// Ensign: Warp engines are online! Captain Picard: Engage!
```

## Module pattern

#### Node
- use `require` to load separate files
- required files need to be stored in a variable
- used files must be exported within the required file

```JavaScript
// ./wheel.js
module.exports = Wheel;

// ./engine.js
module.exports = Engine;

// ./car.js
const Wheel = require("./wheel");
const Engine = require("./engine");
```

Add an extra file to load them all:

```JavaScript
// ./parts.js
module.exports = {
  const Wheel = require("./wheel");
  const Engine = require("./engine");
  // ...
}

// ./car.js
const Parts = require("./parts")

const wheel = new Parts.Wheel();
const engine = new Parts.Engine();
// ...
```


# Prototypal inheritance

Calling a property on a JS object, interpreter will:
1. look in the object itself
2. if not found, search the object's prototype
3. if still not found, recursively look at prototype's `__proto__`

#### `Object.prototype.__proto__` and `Object.setPrototypeOf` **(AVOID)**

Both of these are generally bad practice and leads to poor performance:

```JavaScript
// use `__proto__` as an accessor for the prototype of the object it is called on:
SportsCar.prototype.__proto__ = Vehicle.prototype;
// SportsCar now has access to Vehicle methods

Object.setPrototypeOf(SportsCar.prototype, Vehicle.prototype);
```

## `Object.create`
Create an entirely new `prototype` object:
```JavaScript
SportsCar.prototype = Object.create(Vehicle.prototype);
```
```JavaScript
function Vehicle (model) {
  this.model = model;
};

Vehicle.prototype.startEngine = function () {
  console.log("VROOOMMMM");
};

function SportsCar (model, color) {
  Vehicle.call(this, model);
  this.color = color;
};

SportsCar.prototype.race = function () {
  console.log("Zoom zoom");
};

SportsCar.prototype = Object.create(Vehicle.prototype);

SportsCar.prototype.constructor = SportsCar;

const corvette = new SportsCar("Corvette", "Black");

corvette.startEngine();
corvette.race();
```

## ES6 inheritance

```JavaScript
class SportsCar {
  constructor(model, color) {
    this.model = model;
    this.color = color;
  }

  race() {
    return "Zoom zoom";
  }
}

class HyperCar extends SportsCar {
  constructor(model, color) {
    super(model, color);
  }

  race() {
    const slowRace = super.race();
    return `${slowRace} at LIGHT SPEED!`;
  }
}
```
