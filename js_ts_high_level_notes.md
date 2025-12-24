
---

# 🚀 3-Week JS/TS Crash Course — Detailed Notes

## 🗓️ Week 1 — Core JavaScript

---

### **Day 1 — Basics & Data Types**

**Goal:** Understand JS syntax, variables, and fundamental data types.

#### 1. Variables

* **`let`** — Can be reassigned.

  ```js
  let age = 25;
  age = 26; // ✅ allowed
  ```
* **`const`** — Cannot be reassigned.

  ```js
  const name = "Alice";
  // name = "Bob"; ❌ error
  ```
* **FAQ:** Can `const` objects be changed?
  ✅ Yes, the reference cannot change, but object properties can:

  ```js
  const obj = { a: 1 };
  obj.a = 2; // ✅ allowed
  ```

#### 2. Primitive Types

* **String:** `"hello"`, `'world'`
* **Number:** `42`, `3.14`
* **Boolean:** `true` or `false`
* **Null:** `null` → intentional absence of value
* **Undefined:** `undefined` → variable declared but not assigned

```js
let name = "Alice";
let age;
console.log(age); // undefined
```

#### 3. Objects & Arrays

* **Objects** — key/value pairs

  ```js
  const person = { name: "Alice", age: 25 };
  console.log(person.name); // Alice
  console.log(person['age']); // 25
  ```
* **Arrays** — ordered list

  ```js
  const fruits = ["apple", "banana"];
  console.log(fruits[0]); // apple
  ```

#### 4. Practice Ideas

* Create an object `book` with `title` and `author`.
* Add a new property `year`.
* Create an array of 5 numbers and log each number.

---

### **Day 2 — Functions**

**Goal:** Learn how to write reusable blocks of code.

#### 1. Function Declaration

```js
function greet(name) {
  return `Hello, ${name}!`;
}
console.log(greet("Alice"));
```

#### 2. Function Expression

```js
const greet = function(name) {
  return `Hello, ${name}!`;
};
```

#### 3. Arrow Functions

```js
const greet = name => `Hello, ${name}!`;
```

#### 4. Parameters & Defaults

```js
function add(a, b = 0) {
  return a + b;
}
console.log(add(5)); // 5
```

#### 5. Practice Ideas

* Function to calculate area of rectangle.
* Pass a function as argument:

  ```js
  function operate(a, b, fn) {
    return fn(a, b);
  }
  console.log(operate(2, 3, (x, y) => x * y));
  ```

---

### **Day 3 — Control Flow**

**Goal:** Handle logic and loops effectively.

#### 1. Conditional Statements

* **If / Else**

  ```js
  if (age >= 18) {
    console.log("Adult");
  } else {
    console.log("Minor");
  }
  ```
* **Switch**

  ```js
  const fruit = "apple";
  switch (fruit) {
    case "apple": console.log("Red fruit"); break;
    case "banana": console.log("Yellow fruit"); break;
    default: console.log("Unknown fruit");
  }
  ```

#### 2. Loops

* **for loop**

  ```js
  for (let i = 0; i < 5; i++) console.log(i);
  ```
* **for...of**

  ```js
  for (const fruit of ["apple","banana"]) console.log(fruit);
  ```
* **while loop**

  ```js
  let i = 0;
  while(i < 3) { console.log(i); i++; }
  ```

#### 3. Ternary Operator

```js
const status = age >= 18 ? "Adult" : "Minor";
```

#### 4. Practice Ideas

* Loop through array of numbers and print only even numbers.
* Simulate simple conditional rendering logic like:

  ```js
  const loggedIn = true;
  console.log(loggedIn ? "Show Logout" : "Show Login");
  ```

---

### **Day 4 — ES6+ Features**

**Goal:** Write modern, idiomatic JS.

#### 1. Template Literals

```js
const name = "Alice";
console.log(`Hello ${name}!`);
```

#### 2. Destructuring

```js
const person = { name: "Alice", age: 25 };
const { name, age } = person;
const arr = [1, 2, 3];
const [first, second] = arr;
```

#### 3. Spread & Rest

```js
const arr1 = [1, 2];
const arr2 = [...arr1, 3]; // [1,2,3]

function sum(...numbers) {
  return numbers.reduce((a,b)=>a+b, 0);
}
```

#### 4. Optional Chaining & Nullish Coalescing

```js
const obj = { a: { b: 2 } };
console.log(obj.a?.b); // 2
console.log(obj.x?.y ?? "default"); // default
```

#### 5. Practice Ideas

* Clone an object and add a property without modifying original.
* Access deeply nested properties safely.

---

### **Day 5 — Arrays & Methods**

**Goal:** Transform and filter data efficiently.

#### 1. Array Methods

* **map** — transform

  ```js
  [1,2,3].map(x => x*2); // [2,4,6]
  ```
* **filter** — select

  ```js
  [1,2,3].filter(x => x>1); // [2,3]
  ```
* **reduce** — aggregate

  ```js
  [1,2,3].reduce((sum,x)=>sum+x,0); // 6
  ```
* **find, some, every**

  ```js
  [1,2,3].find(x=>x>1); // 2
  [1,2,3].some(x=>x>2); // true
  [1,2,3].every(x=>x>0); // true
  ```

#### 2. Immutability

* Never modify original array/object in modern JS.

  ```js
  const arr = [1,2,3];
  const newArr = [...arr, 4]; // safe
  ```

#### 3. Practice Ideas

* Filter students with marks > 50.
* Transform list of objects to get names only.
* Sum all numbers using reduce.

---

## 🗓️ Week 2 — Browser APIs & Asynchronous JS

---

### **Day 6 — Modules**

**Goal:** Split code into reusable files.

```js
// math.js
export function add(a,b){ return a+b; }
export const pi = 3.14;

// app.js
import { add, pi } from './math.js';
console.log(add(pi,2));
```

* **Default vs Named Exports**

  ```js
  export default function greet(){...}
  import greet from './file.js';
  ```

---

### **Day 7 — Asynchronous JS**

**Goal:** Handle async operations like API calls.

* **Promises**

  ```js
  fetch("url").then(res => res.json()).then(data => console.log(data));
  ```
* **Async / Await**

  ```js
  async function getData(){
    try {
      const res = await fetch("url");
      const data = await res.json();
    } catch(err){
      console.error(err);
    }
  }
  ```
* **Promise.all** — parallel requests

#### Practice Ideas

* Fetch data from `https://jsonplaceholder.typicode.com/posts`
* Handle network errors gracefully

---

### **Day 8 — Browser & Events**

* `addEventListener` for clicks, input

  ```js
  const btn = document.querySelector("button");
  btn.addEventListener("click", ()=>console.log("Clicked!"));
  ```

* Event objects:

  ```js
  input.addEventListener("input", e => console.log(e.target.value));
  ```

* Practice: build counter or input logger.

---

### **Day 9 — DOM & Basic HTML/CSS**

* Create/modify elements

  ```js
  const div = document.createElement("div");
  div.textContent = "Hello";
  document.body.appendChild(div);
  ```

* Basic CSS: classes, flexbox

  ```css
  .container { display: flex; gap: 10px; }
  ```

* Practice: dynamic list with basic styling.

---

### **Day 10 — Error Handling & Debugging**

* `try/catch/finally`

  ```js
  try { riskyOperation(); } 
  catch(err){ console.error(err); } 
  finally{ console.log("Done"); }
  ```

* Debugging:

  * `console.log`, `console.table`
  * DevTools breakpoints

* Practice: validate user input, handle API errors.

---

## 🗓️ Week 3 — TypeScript & Advanced Patterns

---

### **Day 11 — TypeScript Basics**

* Add types:

  ```ts
  let name: string = "Alice";
  let age: number = 25;
  ```
* Function typing:

  ```ts
  function add(a:number, b:number): number { return a+b; }
  ```
* Interfaces & type aliases:

  ```ts
  interface Person { name: string; age: number; }
  const p: Person = { name: "Alice", age: 25 };
  ```

---

### **Day 12 — TypeScript Advanced**

* Union types:

  ```ts
  let id: string | number;
  ```
* Generics:

  ```ts
  function identity<T>(arg: T): T { return arg; }
  ```
* Typing async functions:

  ```ts
  async function fetchData(): Promise<Post[]> { ... }
  ```

---

### **Day 13 — State & Immutability Patterns**

* Update arrays/objects immutably:

  ```ts
  const newArr = arr.map(obj => obj.id===2 ? {...obj, done:true} : obj);
  ```
* Value vs reference behavior in JS/TS.

---

### **Day 14 — Event & Callback Patterns**

* Pass callbacks:

  ```ts
  function handleClick(cb: () => void) { cb(); }
  handleClick(() => console.log("Clicked!"));
  ```
* Custom events in Svelte:
  Conceptual understanding of parent-child communication.

---

### **Day 15 — Mini Project**

**Build:**

* Fetch data from API
* Display list
* Input handling
* Update state immutably

**Outcome:** Ready for Svelte 5 & SvelteKit.

---
