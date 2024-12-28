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
