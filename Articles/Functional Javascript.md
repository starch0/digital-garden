# Functional Programming in JavaScript

Functional programming (FP) is a programming paradigm that treats computation as the evaluation of mathematical functions and avoids changing state or mutable data. JavaScript, although primarily known as an object-oriented and prototype-based language, has strong support for functional programming concepts. 


## Why should bother about using FP? 

- Predictability: Pure functions and immutability lead to fewer bugs.
- Reusability: Small, composable functions can be reused in different contexts.
- Easier Debugging: No hidden side effects make debugging simpler.
- Concurrency-Friendly: FP avoids shared state, making concurrent execution easier.

## Key Concepts of Functional Programming

### 1. **First-Class Functions**

JavaScript treats functions as first-class citizens, meaning they can be assigned to variables, passed as arguments, and returned from other functions.

```javascript
const greet = (name) => `Hello, ${name}!`;
const sayHello = greet;
console.log(sayHello("Alice")); // Output: Hello, Alice!
```

### 2. **Pure Functions**

A pure function is a function that:

- Always returns the same output for the same input.
- Does not cause side effects (i.e., it does not modify external state).

```javascript
const add = (a, b) => a + b;
console.log(add(2, 3)); // Output: 5
```

### 3. **Immutability**

In FP, data should not be changed once created. Instead, new data structures are returned when modifications are needed.

```javascript
const numbers = [1, 2, 3];
const newNumbers = [...numbers, 4]; // Creating a new array instead of modifying the original
console.log(numbers); // Output: [1, 2, 3]
console.log(newNumbers); // Output: [1, 2, 3, 4]
```

### 4. **Higher-Order Functions**

Higher-order functions are functions that take other functions as arguments or return them as results.

```javascript
const double = (n) => n * 2;
const mapArray = (arr, fn) => arr.map(fn);
console.log(mapArray([1, 2, 3], double)); // Output: [2, 4, 6]
```

### 5. **Function Composition**

Function composition is the process of combining multiple functions into a single function.

```javascript
const multiply = (x) => x * 2;
const subtract = (x) => x - 1;
const compose = (f, g) => (x) => f(g(x));
const multiplyThenSubtract = compose(subtract, multiply);
console.log(multiplyThenSubtract(5)); // Output: 9
```

### 6. **Recursion**

Recursion is a technique where a function calls itself to solve a problem instead of using loops.

```javascript
const factorial = (n) => (n === 0 ? 1 : n * factorial(n - 1));
console.log(factorial(5)); // Output: 120
```

## Functional Programming in Practice

### **Using Array Methods**

JavaScript provides several built-in array methods that align with FP principles, such as:

- `map`: Transforms an array by applying a function to each element.
- `filter`: Returns a new array with elements that pass a given condition.
- `reduce`: Accumulates values into a single result.

```javascript
const numbers = [1, 2, 3, 4, 5];
const doubled = numbers.map((n) => n * 2);
const evens = numbers.filter((n) => n % 2 === 0);
const sum = numbers.reduce((acc, curr) => acc + curr, 0);
console.log(doubled); // Output: [2, 4, 6, 8, 10]
console.log(evens); // Output: [2, 4]
console.log(sum); // Output: 15
```
