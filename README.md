# 1 - JavaScript Patterns

## 1.1 - Module Pattern

The module pattern is a great way to `split a larger file into multiple smaller`, reusable pieces.
It also `promotes code encapsulation`, since the `values within modules are kept private` inside the module by default, and cannot be modified. This protects values from leaking into the global scope or ending up in a naming collision.

> Only the values that are explicitly exported with the export keyword are accessible to other files.

In Node, you can use ES2015 modules either by:

- Using the .mjs extension
- Adding "type": "module" to your package.json

Example:

```json
// package.json
{
  "name": "node-test",
  "version": "0.0.0",
  "type": "module",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  }
}
```

```js
// math.js
const secret = "abc"; // not accesible variable outside the module as it hasn't been exported

export function sum(x, y) {
  return x + y;
}

export function multiply(x, y) {
  return x * y;
}

export function subtract(x, y) {
  return x - y;
}

export function divide(x, y) {
  return x / y;
}
```

```js
// index.js
import { sum, subtract, divide, multiply } from "./math.js";

console.log("Sum", sum(1, 2));
console.log("Subtract", subtract(1, 2));
console.log("Divide", divide(1, 2));
console.log("Multiply", multiply(1, 2));
```

## 1.2 - Singleton Pattern (Unnecessary)

> Unnecessary: ES2015 Modules are singletons by default. We no longer need to explicitly create singletons to achieve this global, non-modifiable behavior.

Shares a `single global instance throughout our application`.

Cons:

- Testing: It's really difficult to test singletons because we change it globally.

- Depedency Hiding: When importing another module, it may not always be obvious that that module is importing a Singleton. This could lead to unexpected value modification within the Singleton, which would be reflected throughout the application.

- Global Scope Pollution: The global behavior of Singletons is essentially the same as a global variable. Global Scope Pollution can end up in accidentally overwriting the value of a global variable, which can lead to a lot of unexpected behavior

Example:

```js
let instance;

class DBConnection {
  constructor(uri) {
    if (instance) {
      throw new Error("Only one connection can exist!");
    }
    this.uri = uri;
    instance = this;
  }

  connect() {
    this.isConnected = true;
    console.log(`DB ${this.uri} has been connected!`);
  }

  disconnect() {
    this.isConnected = false;
    console.log("DB disconnected");
  }
}

const databaseConnector = Object.freeze(new DBConnection());
const connection = databaseConnector;
```

We can also directly create objects without having to use a class, which can lead to much simpler and cleaner code.

To create a singleton using a regular object, we have to:

```js
let counter = 0;

// 1. Create an object containing the `getCount`, `increment`, and `decrement` method.
const counterObject = {
  getCount: () => counter,
  increment: () => ++counter,
  decrement: () => --counter,
};

// 2. Freeze the object using the `Object.freeze` method, it ensures immutability, which aligns with the singleton's non-modifiable behavior.
const singletonCounter = Object.freeze(counterObject);

// 3. Export the object as the `default` value to make it globally accessible.
export default singletonCounter;
```

We could even export the frozen object directly, without having to declare multiple variables.

```js
let counter = 0;

export default Object.freeze({
  getCount: () => counter,
  increment: () => ++counter,
  decrement: () => --counter,
});
```

## 1.3 - Difference Between Modules and Singletons

| Aspect            | **Module**                                                                                        | **Singleton**                                                     |
| ----------------- | ------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------- |
| **Definition**    | A file or function that exports values, functions, or objects for reuse.                          | A pattern ensuring only one instance of a class or object exists. |
| **Scope**         | Depends on what is exported: functions or objects can be reused with new instances or references. | Always maintains a single global instance.                        |
| **Encapsulation** | Strong encapsulation of variables and functions. Non-exported values remain private.              | Encapsulates the instance but usually exposes methods publicly.   |
| **State**         | Has no intrinsic state (unless explicitly created).                                               | Retains state because it is always the same object or instance.   |
| **Applications**  | Organizing code, utility libraries.                                                               | Managing global resources (e.g., DB connections, configuration).  |
