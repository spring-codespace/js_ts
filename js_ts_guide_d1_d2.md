# 🚀 3-Week JS/TS Crash Course — Comprehensive Guide for Svelte

This guide is designed to take you from JavaScript basics to TypeScript mastery, with a focus on concepts that will directly help you build better Svelte 5 and SvelteKit applications.

---

## 🗓️ Week 1 — Core JavaScript Fundamentals

---

### **Day 1 — Variables, Data Types & Basic Operations**

**Why This Matters for Svelte:** Understanding variables and data types is fundamental because Svelte's reactivity system tracks changes to variables. Knowing when to use `let` vs `const` directly impacts how your Svelte components update.

---

#### **1. Variables: The Foundation of Reactive Programming**

JavaScript has three ways to declare variables, but in modern code (and especially in Svelte), you'll primarily use `let` and `const`.

##### **`let` — For Values That Change**

Use `let` when you expect the value to change over time. In Svelte, reactive statements watch `let` variables for changes.

```js
let count = 0;
count = count + 1; // ✅ This works
count = 5; // ✅ You can reassign as many times as needed

let username = "guest";
username = "Alice"; // ✅ User logs in, name changes
```

**When to use `let` in Svelte:**
- Component state that changes (counters, toggles, form inputs)
- Loop variables
- Values calculated from user interactions

##### **`const` — For Values That Stay Fixed**

Use `const` for values that shouldn't be reassigned. This prevents accidental changes and makes your code more predictable.

```js
const API_KEY = "abc123xyz";
// API_KEY = "newkey"; // ❌ TypeError: Assignment to constant variable

const maxUsers = 100;
const appName = "My Svelte App";
```

**Common Misconception:**
Many beginners think `const` means "this can never change." That's not quite right!

```js
const user = { name: "Alice", age: 25 };

// ✅ This WORKS — you're modifying the object's contents
user.age = 26;
user.email = "alice@example.com";

// ❌ This FAILS — you're trying to reassign the variable itself
// user = { name: "Bob", age: 30 };
```

**Think of it this way:** `const` means the *variable binding* is constant, not that the value is frozen. The reference to the object can't change, but the object's properties can.

**When to use `const` in Svelte:**
- Configuration values
- Imported functions or components
- Arrays/objects that you'll modify in place (though prefer immutable patterns)
- Props that shouldn't be reassigned

##### **The Old Way: `var` (Avoid This)**

You might see `var` in old tutorials. Don't use it. It has confusing scoping rules that `let` and `const` fixed.

```js
// ❌ Don't do this
var x = 10;

// ✅ Do this instead
let x = 10;
```

---

#### **2. Primitive Data Types: The Building Blocks**

JavaScript has 7 primitive types, but you'll use these 5 constantly:

##### **String — Text Data**

Strings hold text. You can use single quotes, double quotes, or backticks (template literals — more on this Day 4).

```js
let firstName = "Alice";
let lastName = 'Smith';
let greeting = `Hello, ${firstName}!`; // Template literal with interpolation

// Common string operations
let message = "hello world";
console.log(message.length); // 11
console.log(message.toUpperCase()); // "HELLO WORLD"
console.log(message.includes("world")); // true
console.log(message.split(" ")); // ["hello", "world"]
```

**In Svelte:** Strings are used for displaying text, binding to inputs, and passing data:
```svelte
<script>
  let name = "Alice";
</script>

<h1>Hello {name}!</h1>
<input bind:value={name} />
```

##### **Number — All Numeric Values**

Unlike some languages, JavaScript has only one number type — no separate integers and floats.

```js
let age = 25; // Integer
let price = 19.99; // Decimal
let negative = -42;
let large = 1_000_000; // You can use underscores for readability (ES2021)

// Special numeric values
let divideByZero = 1 / 0; // Infinity
let notANumber = "hello" / 2; // NaN (Not a Number)

console.log(typeof NaN); // "number" (yes, really! 😅)
```

**Common operations:**
```js
let x = 10;
let y = 3;

console.log(x + y); // 13 (addition)
console.log(x - y); // 7 (subtraction)
console.log(x * y); // 30 (multiplication)
console.log(x / y); // 3.333... (division)
console.log(x % y); // 1 (remainder/modulo)
console.log(x ** y); // 1000 (exponentiation)

// Useful Math methods
console.log(Math.round(4.7)); // 5
console.log(Math.floor(4.7)); // 4
console.log(Math.ceil(4.1)); // 5
console.log(Math.random()); // Random number between 0 and 1
```

##### **Boolean — True or False**

Booleans represent logical states. They're essential for conditional logic.

```js
let isLoggedIn = true;
let hasPermission = false;
let isActive = 1 > 0; // true (comparison results in boolean)

// Common in conditionals
if (isLoggedIn) {
  console.log("Welcome back!");
}
```

**Truthy and Falsy Values** (Important!)

In JavaScript, values can be "truthy" or "falsy" when used in boolean contexts:

```js
// FALSY values (only 6 of them!)
false
0
"" (empty string)
null
undefined
NaN

// Everything else is TRUTHY, including:
true
1, -1, 42 (any non-zero number)
"hello", "0", "false" (any non-empty string)
[], {} (arrays and objects, even if empty)
```

This matters in conditionals:
```js
let username = "";

if (username) {
  console.log("User logged in"); // Won't run — empty string is falsy
} else {
  console.log("Guest user"); // This runs
}
```

##### **Null — Intentional Absence**

`null` represents "no value" on purpose. Use it when you want to explicitly say "there's nothing here."

```js
let selectedItem = null; // No item selected yet

function findUser(id) {
  // If user not found, return null
  return null;
}

// Common pattern: checking for null
if (selectedItem === null) {
  console.log("Nothing selected");
}
```

##### **Undefined — Missing or Not Yet Set**

`undefined` means a variable has been declared but not assigned a value yet.

```js
let result; // Declared but not assigned
console.log(result); // undefined

function greet(name) {
  console.log(name); // undefined if called without argument
}
greet(); // name parameter is undefined

let person = { name: "Alice" };
console.log(person.age); // undefined — property doesn't exist
```

**Null vs Undefined:**
- `undefined`: "JavaScript doesn't know about this yet"
- `null`: "I deliberately set this to nothing"

```js
let notYetSet; // undefined
let deliberatelyEmpty = null; // null

// Checking for both
if (value == null) { // Loose equality checks both null and undefined
  console.log("No value");
}

// Or be explicit
if (value === null || value === undefined) {
  console.log("No value");
}
```

---

#### **3. Complex Data Types: Objects and Arrays**

##### **Objects — Collections of Key-Value Pairs**

Objects are the most important data structure in JavaScript. They're like dictionaries or maps in other languages.

```js
// Creating an object
const person = {
  name: "Alice",
  age: 25,
  email: "alice@example.com",
  isActive: true
};

// Accessing properties (two ways)
console.log(person.name); // "Alice" (dot notation — preferred)
console.log(person['age']); // 25 (bracket notation — needed for dynamic keys)

// When to use bracket notation:
let propertyName = "email";
console.log(person[propertyName]); // "alice@example.com"

// Properties with spaces or special characters (avoid this in your own code)
const weird = {
  "first name": "Bob", // Needs quotes
  "user-id": 123
};
console.log(weird["first name"]); // Must use brackets

// Adding new properties
person.city = "Stockholm";
person['country'] = "Sweden";

// Modifying properties
person.age = 26;

// Deleting properties
delete person.isActive;

// Checking if property exists
console.log('name' in person); // true
console.log('salary' in person); // false
```

**Nested objects:**
```js
const user = {
  name: "Alice",
  address: {
    street: "123 Main St",
    city: "Stockholm",
    country: "Sweden"
  },
  preferences: {
    theme: "dark",
    notifications: true
  }
};

// Accessing nested properties
console.log(user.address.city); // "Stockholm"
console.log(user.preferences.theme); // "dark"

// Be careful with deeply nested access
// console.log(user.profile.bio); // ❌ Error if 'profile' doesn't exist
console.log(user.profile?.bio); // ✅ Returns undefined (optional chaining — Day 4)
```

**Object methods:**
```js
const calculator = {
  value: 0,
  add: function(x) {
    this.value += x;
  },
  // Shorthand method syntax (modern, preferred)
  subtract(x) {
    this.value -= x;
  },
  // Arrow functions DON'T work well for methods (more on Day 2)
  reset: () => {
    this.value = 0; // ❌ 'this' doesn't work as expected here
  }
};

calculator.add(10);
console.log(calculator.value); // 10
```

**In Svelte:** Objects are used everywhere — component props, reactive state, stores:
```svelte
<script>
  let user = {
    name: "Alice",
    age: 25
  };
  
  // Svelte detects changes to properties
  function incrementAge() {
    user.age += 1; // Svelte won't detect this!
    user = user; // Need to reassign to trigger reactivity
    
    // Or better: use immutable update
    user = { ...user, age: user.age + 1 };
  }
</script>
```

##### **Arrays — Ordered Lists**

Arrays store multiple values in a numbered sequence (indexed from 0).

```js
// Creating arrays
const fruits = ["apple", "banana", "orange"];
const numbers = [1, 2, 3, 4, 5];
const mixed = [1, "hello", true, null, { name: "Alice" }]; // Can mix types

// Accessing elements (zero-indexed!)
console.log(fruits[0]); // "apple" (first item)
console.log(fruits[1]); // "banana" (second item)
console.log(fruits[fruits.length - 1]); // "orange" (last item)

// Array properties
console.log(fruits.length); // 3

// Modifying arrays (we'll learn better methods on Day 5)
fruits[1] = "mango"; // Change an element
fruits.push("grape"); // Add to end
fruits.pop(); // Remove from end
fruits.unshift("kiwi"); // Add to start
fruits.shift(); // Remove from start

console.log(fruits); // ["apple", "mango", "orange"]
```

**Why array index starts at 0:**
This is a programming convention from C. Think of the index as an "offset" from the beginning. The first element is at offset 0 (no offset), the second is at offset 1, etc.

**Common array patterns:**
```js
const scores = [85, 92, 78, 90, 88];

// Check if array contains a value
console.log(scores.includes(92)); // true

// Find index of a value
console.log(scores.indexOf(90)); // 3

// Check if array is empty
if (scores.length === 0) {
  console.log("No scores yet");
}

// Last item
const lastScore = scores[scores.length - 1]; // 88
```

**Array of objects (very common in Svelte!):**
```js
const users = [
  { id: 1, name: "Alice", active: true },
  { id: 2, name: "Bob", active: false },
  { id: 3, name: "Charlie", active: true }
];

// Access first user's name
console.log(users[0].name); // "Alice"

// Loop through users (Day 3)
for (const user of users) {
  console.log(user.name);
}
```

**In Svelte:** Arrays power your lists and iterations:
```svelte
<script>
  let todos = [
    { id: 1, text: "Learn JS", done: false },
    { id: 2, text: "Learn Svelte", done: false }
  ];
</script>

{#each todos as todo}
  <div>{todo.text}</div>
{/each}
```

---

#### **4. Type Checking and Conversion**

**Checking types:**
```js
console.log(typeof "hello"); // "string"
console.log(typeof 42); // "number"
console.log(typeof true); // "boolean"
console.log(typeof undefined); // "undefined"
console.log(typeof null); // "object" (this is a JavaScript bug, but it's staying)
console.log(typeof {}); // "object"
console.log(typeof []); // "object" (arrays are objects!)

// Better array check
console.log(Array.isArray([])); // true
console.log(Array.isArray({})); // false
```

**Type conversion:**
```js
// String to Number
console.log(Number("42")); // 42
console.log(parseInt("42")); // 42
console.log(parseFloat("3.14")); // 3.14
console.log(+"42"); // 42 (unary plus operator)

// Number to String
console.log(String(42)); // "42"
console.log((42).toString()); // "42"
console.log(42 + ""); // "42" (concatenation coercion)

// To Boolean
console.log(Boolean(1)); // true
console.log(Boolean(0)); // false
console.log(Boolean("")); // false
console.log(Boolean("hello")); // true
console.log(!!"hello"); // true (double negation trick)
```

---

#### **5. Practice Exercises**

**Exercise 1: Personal Info Object**
```js
// Create an object representing yourself
const me = {
  firstName: "Your Name",
  lastName: "Your Last Name",
  age: 25,
  city: "Stockholm",
  hobbies: ["coding", "reading", "gaming"]
};

// Add a new property 'country'
me.country = "Sweden";

// Change your age
me.age = 26;

// Add a new hobby
me.hobbies.push("hiking");

// Log full name
console.log(`${me.firstName} ${me.lastName}`);
```

**Exercise 2: Book Collection**
```js
// Create an array of book objects
const books = [
  { title: "1984", author: "George Orwell", year: 1949, read: true },
  { title: "To Kill a Mockingbird", author: "Harper Lee", year: 1960, read: false },
  { title: "The Great Gatsby", author: "F. Scott Fitzgerald", year: 1925, read: true }
];

// Log the title of the first book
console.log(books[0].title);

// Mark the second book as read
books[1].read = true;

// Add a new book
books.push({ title: "Brave New World", author: "Aldous Huxley", year: 1932, read: false });

// Count how many books you've read (we'll learn a better way on Day 5)
let readCount = 0;
for (const book of books) {
  if (book.read) readCount++;
}
console.log(`I've read ${readCount} books`);
```

**Exercise 3: Shopping Cart**
```js
const cart = {
  items: [],
  total: 0,
  
  addItem(name, price) {
    this.items.push({ name, price });
    this.total += price;
  },
  
  removeItem(index) {
    const removedItem = this.items[index];
    this.total -= removedItem.price;
    this.items.splice(index, 1);
  }
};

cart.addItem("Laptop", 999);
cart.addItem("Mouse", 25);
console.log(cart.total); // 1024
```

---

### **Day 2 — Functions: Reusable Code Blocks**

**Why This Matters for Svelte:** Functions are the backbone of component logic. Every event handler, every computed value, every data transformation uses functions. Mastering functions will make your Svelte components clean and maintainable.

---

#### **1. Function Declarations: The Traditional Way**

This is the classic way to define functions. They're "hoisted," meaning you can call them before they're defined in your code.

```js
// Basic function
function greet(name) {
  return `Hello, ${name}!`;
}

console.log(greet("Alice")); // "Hello, Alice!"

// Function with multiple parameters
function add(a, b) {
  return a + b;
}

console.log(add(5, 3)); // 8

// Function without return (returns undefined)
function logMessage(message) {
  console.log(message);
  // No return statement
}

const result = logMessage("Hi"); // Logs "Hi"
console.log(result); // undefined
```

**Hoisting in action:**
```js
// This works! Function declarations are hoisted
sayHello();

function sayHello() {
  console.log("Hello!");
}
```

**When to use function declarations:**
- Top-level utility functions
- When you need hoisting behavior
- Traditional, readable syntax

---

#### **2. Function Expressions: Functions as Values**

Functions are "first-class citizens" in JavaScript — they can be assigned to variables, passed as arguments, and returned from other functions.

```js
// Function expression
const greet = function(name) {
  return `Hello, ${name}!`;
};

console.log(greet("Bob")); // "Hello, Bob!"

// Function expressions are NOT hoisted
// greetEarly(); // ❌ ReferenceError
const greetEarly = function() {
  console.log("Hello!");
};
```

**Anonymous vs named function expressions:**
```js
// Anonymous (no function name after 'function')
const add = function(a, b) {
  return a + b;
};

// Named (helpful for debugging — shows in stack traces)
const subtract = function subtractNumbers(a, b) {
  return a - b;
};
```

---

#### **3. Arrow Functions: Modern and Concise**

Arrow functions are the modern way to write functions. They're shorter and have different `this` binding behavior (important for Day 4).

**Basic syntax:**
```js
// Traditional function expression
const add = function(a, b) {
  return a + b;
};

// Arrow function equivalent
const add = (a, b) => {
  return a + b;
};

// If only one statement that returns, you can omit braces and 'return'
const add = (a, b) => a + b;

// Single parameter? Parentheses are optional
const double = x => x * 2;
const greet = name => `Hello, ${name}!`;

// No parameters? Need empty parentheses
const getRandom = () => Math.random();

// Returning an object? Wrap in parentheses
const makePerson = (name, age) => ({ name, age });
// Without parentheses, JavaScript thinks { } is a block, not an object!
```

**When arrow function body needs multiple lines:**
```js
const processUser = (user) => {
  const fullName = `${user.firstName} ${user.lastName}`;
  const greeting = `Welcome, ${fullName}!`;
  console.log(greeting);
  return fullName;
};
```

**Array methods love arrow functions (Day 5 deep dive):**
```js
const numbers = [1, 2, 3, 4, 5];

// Traditional
const doubled = numbers.map(function(num) {
  return num * 2;
});

// Arrow function (concise!)
const doubled = numbers.map(num => num * 2);

// Even more readable
const doubled = numbers.map(n => n * 2);
```

**When NOT to use arrow functions:**
```js
// ❌ Don't use for object methods (when you need 'this')
const calculator = {
  value: 10,
  double: () => {
    this.value * 2; // 'this' doesn't refer to calculator!
  }
};

// ✅ Use regular function for object methods
const calculator = {
  value: 10,
  double() {
    return this.value * 2;
  }
};
```

**In Svelte:** Arrow functions are everywhere:
```svelte
<script>
  let count = 0;
  
  // Event handlers are often arrow functions
  const increment = () => {
    count += 1;
  };
  
  // Or inline
  const decrement = () => count -= 1;
</script>

<button on:click={increment}>+</button>
<button on:click={() => count += 1}>+ Inline</button>
```

---

#### **4. Parameters and Arguments: Inputs to Functions**

**Basic parameters:**
```js
function greet(name, age) {
  return `${name} is ${age} years old`;
}

greet("Alice", 25); // "Alice is 25 years old"
greet("Bob"); // "Bob is undefined years old" (age is undefined)
```

**Default parameters:**
```js
function greet(name = "Guest", greeting = "Hello") {
  return `${greeting}, ${name}!`;
}

console.log(greet()); // "Hello, Guest!"
console.log(greet("Alice")); // "Hello, Alice!"
console.log(greet("Bob", "Hi")); // "Hi, Bob!"

// Default can be any expression
function createId(prefix = "user", suffix = Date.now()) {
  return `${prefix}_${suffix}`;
}
```

**Rest parameters (collecting multiple arguments):**
```js
// Collects all arguments into an array
function sum(...numbers) {
  let total = 0;
  for (const num of numbers) {
    total += num;
  }
  return total;
}

console.log(sum(1, 2, 3)); // 6
console.log(sum(1, 2, 3, 4, 5)); // 15

// Rest must be the last parameter
function describe(action, ...items) {
  return `${action}: ${items.join(", ")}`;
}

console.log(describe("Bought", "apple", "banana", "orange"));
// "Bought: apple, banana, orange"
```

**Arguments object (old way, avoid in modern code):**
```js
// Old way (don't use this)
function oldSum() {
  let total = 0;
  for (let i = 0; i < arguments.length; i++) {
    total += arguments[i];
  }
  return total;
}

// Modern way (use rest parameters instead)
const sum = (...numbers) => numbers.reduce((a, b) => a + b, 0);
```

---

#### **5. Return Values: Getting Data Out**

```js
// Explicit return
function add(a, b) {
  return a + b;
  console.log("This never runs"); // Code after return is unreachable
}

// No return = undefined
function greet(name) {
  console.log(`Hello, ${name}`);
}
const result = greet("Alice"); // Logs "Hello, Alice"
console.log(result); // undefined

// Early returns for validation
function divide(a, b) {
  if (b === 0) {
    return "Cannot divide by zero"; // Exit early
  }
  return a / b;
}

// Returning objects (common in Svelte)
function createUser(name, age) {
  return {
    name,
    age,
    id: Math.random().toString(36),
    createdAt: new Date()
  };
}

// Returning functions (higher-order functions)
function multiplier(factor) {
  return (number) => number * factor;
}

const double = multiplier(2);
const triple = multiplier(3);
console.log(double(5)); // 10
console.log(triple(5)); // 15
```

---

#### **6. Functions as Arguments (Callbacks)**

One of JavaScript's superpowers is treating functions as values you can pass around.

```js
// Function that takes another function as an argument
function operate(a, b, operation) {
  return operation(a, b);
}

// Pass functions as arguments
const result1 = operate(5, 3, (x, y) => x + y); // 8
const result2 = operate(5, 3, (x, y) => x * y); // 15
const result3 = operate(10, 2, (x, y) => x / y); // 5

// Real-world example: array transformations
const numbers = [1, 2, 3, 4, 5];

function transform(arr, transformFn) {
  const result = [];
  for (const item of arr) {
    result.push(transformFn(item));
  }
  return result;
}

const squared = transform(numbers, x => x ** 2);
// [1, 4, 9, 16, 25]

// Event handlers are callbacks!
function handleClick(callback) {
  console.log("Button clicked!");
  callback();
}

handleClick(() => console.log("Callback executed!"));
```

**In Svelte:** Callbacks are used for event handling and component communication:
```svelte
<script>
  let items = ["Apple", "Banana", "Cherry"];
  
  function handleItemClick(item) {
    console.log(`Clicked: ${item}`);
  }
</script>

{#each items as item}
  <button on:click={() => handleItemClick(item)}>
    {item}
  </button>
{/each}
```

---

#### **7. Scope and Closures**

**Scope** determines where variables are accessible.

```js
// Global scope
const globalVar = "I'm global";

function outer() {
  // Function scope
  const outerVar = "I'm in outer";
  
  function inner() {
    // Nested function scope
    const innerVar = "I'm in inner";
    
    // Can access all parent scopes
    console.log(globalVar); // ✅ Works
    console.log(outerVar); // ✅ Works
    console.log(innerVar); // ✅ Works
  }
  
  inner();
  // console.log(innerVar); // ❌ Error: innerVar not defined
}

outer();
// console.log(outerVar); // ❌ Error: outerVar not defined
```

**Closures** — functions "remember" their surrounding scope:
```js
function createCounter() {
  let count = 0; // Private variable
  
  return {
    increment() {
      count++;
      return count;
    },
    decrement() {
      count--;
      return count;
    },
    getCount() {
      return count;
    }
  };
}

const counter = createCounter();
console.log(counter.increment()); // 1
console.log(counter.increment()); // 2
console.log(counter.decrement()); // 1
console.log(counter.getCount()); // 1

// count is private — can't access directly!
// console.log(counter.count); // undefined
```

**Practical closure example — event handlers:**
```js
function setupButtons() {
  for (let i = 0; i < 3; i++) {
    const button = document.createElement('button');
    button.textContent = `Button ${i}`;
    
    // Each button "remembers" its own 'i'
    button.addEventListener('click', () => {
      console.log(`Clicked button ${i}`);
    });
    
    document.body.appendChild(button);
  }
}

// Why 'let' matters here: try changing to 'var' — all buttons will log the same number!
```

---

#### **8. Practice Exercises**

**Exercise 1: Temperature Converter**
```js
// Create functions to convert between Celsius and Fahrenheit

function celsiusToFahrenheit(celsius) {
  return (celsius * 9/5) + 32;
}

function fahrenheitToCelsius(fahrenheit) {
  return (fahrenheit - 32) * 5/9;
}

// With default and rounding
const convertTemp = (temp, toUnit = 'F') => {
  if (toUnit === 'F') {
    return Math.round(temp * 9/5 + 32);
  } else {
    return Math.round((temp - 32) * 5/9);
  }
};

console.log(convertTemp(25)); // 77
console.log(convertTemp(77, 'C')); // 25
```

**Exercise 2: Array Calculator**
```js
// Create a function that takes an array and a callback
function calculate(numbers, operation) {
  return numbers.map(operation);
}

const nums = [1, 2, 3, 4, 5];

console.log(calculate(nums, n => n * 2)); // [2, 4, 6, 8, 10]
console.log(calculate(nums, n => n ** 2)); // [1, 4, 9, 16, 25]
console.log(calculate(nums, n => n + 10)); // [11, 12, 13, 14, 15]
```

**Exercise 3: Person Factory**
```js
// Function that creates person objects with methods
function createPerson(firstName, lastName, age) {
  return {
    firstName,
    lastName,
    age,
    
    getFullName() {
      return `${this.firstName} ${this.lastName}`;
    },
    
    greet(otherName) {
      return `Hi ${otherName}, I'm ${this.getFullName()}`;
    },
    
    haveBirthday() {
      this.age++;
      return `Happy birthday! Now ${this.age} years old`;
    }
  };
}

const alice = createPerson("Alice", "Smith", 25);
console.log(alice.greet("Bob")); // "Hi Bob, I'm Alice Smith"
console.log(alice.haveBirthday()); // "Happy birthday! Now 26 years old"
```

**Exercise 4: Counter with Closure**
```js
// Create a private counter that can only be modified through methods
function makeCounter(initialValue = 0) {
  let count = initialValue;
  
  return {
    inc: () => ++count,
    dec: () => --count,
    get: () => count,
    reset: () => count = initialValue
  };
}

const counter1 = makeCounter(0);
const counter2 = makeCounter(100);

console.log(counter1.inc()); // 1
console.log(counter1.inc()); // 2
console.log(counter2.inc()); // 101
console.log(counter1.get()); // 2 (separate from counter2!)
```

**Exercise 5: Function Composition**
```js
// Create functions that work together
const addTax = price => price * 1.25; // 25% tax
const addShipping = price => price + 10;
const applyDiscount = (price, percent) => price * (1 - percent / 100);

// Manual composition
function calculateTotal(basePrice, discountPercent) {
  let price = basePrice;
  price = applyDiscount(price, discountPercent);
  price = addTax(price);
  price = addShipping(price);
  return Math.round(price * 100) / 100; // Round to 2 decimals
}

console.log(calculateTotal(100, 10)); // 100 → 90 → 112.5 → 122.5

// Advanced: Create a compose function (preview of functional programming)
const compose = (...fns) => (value) => fns.reduce((acc, fn) => fn(acc), value);

const calculatePrice = compose(
  (price) => applyDiscount(price, 10),
  addTax,
  addShipping
);

console.log(calculatePrice(100)); // Same result: 122.5
```

---

#### **9. Common Function Patterns in Svelte**

Now let's see how these function concepts apply directly to Svelte development:

**Event Handler Functions**
```svelte
<script>
  let count = 0;
  let name = '';
  
  // Simple handler
  function increment() {
    count += 1;
  }
  
  // Handler with parameter from event
  function handleInput(event) {
    name = event.target.value;
  }
  
  // Handler that prevents default behavior
  function handleSubmit(event) {
    event.preventDefault();
    console.log('Form submitted with:', name);
  }
  
  // Handler that needs arguments (use arrow function in template)
  function addAmount(amount) {
    count += amount;
  }
</script>

<button on:click={increment}>Count: {count}</button>

<input type="text" on:input={handleInput} value={name} />

<form on:submit={handleSubmit}>
  <button>Submit</button>
</form>

<!-- Passing arguments requires arrow function wrapper -->
<button on:click={() => addAmount(5)}>Add 5</button>
<button on:click={() => addAmount(10)}>Add 10</button>
```

**Computed/Derived Values (Reactive Declarations)**
```svelte
<script>
  let firstName = 'Alice';
  let lastName = 'Smith';
  
  // Svelte's reactive declaration - reruns when dependencies change
  $: fullName = `${firstName} ${lastName}`;
  
  // Can also be a function call
  $: displayName = getDisplayName(firstName, lastName);
  
  function getDisplayName(first, last) {
    return `${first.toUpperCase()} ${last.toUpperCase()}`;
  }
  
  // Complex computed values
  let items = [
    { name: 'Apple', price: 1.5, quantity: 3 },
    { name: 'Banana', price: 0.8, quantity: 5 }
  ];
  
  $: total = items.reduce((sum, item) => {
    return sum + (item.price * item.quantity);
  }, 0);
  
  $: formattedTotal = `${total.toFixed(2)}`;
</script>

<p>Full Name: {fullName}</p>
<p>Display: {displayName}</p>
<p>Cart Total: {formattedTotal}</p>
```

**Component Helper Functions**
```svelte
<script>
  let users = [
    { id: 1, name: 'Alice', age: 25, active: true },
    { id: 2, name: 'Bob', age: 30, active: false },
    { id: 3, name: 'Charlie', age: 22, active: true }
  ];
  
  // Filter function
  function getActiveUsers(userList) {
    return userList.filter(user => user.active);
  }
  
  // Find function
  function getUserById(id) {
    return users.find(user => user.id === id);
  }
  
  // Transform function
  function formatUser(user) {
    return {
      ...user,
      displayName: `${user.name} (${user.age})`
    };
  }
  
  // Action function (modifies state)
  function toggleUserStatus(userId) {
    users = users.map(user => 
      user.id === userId 
        ? { ...user, active: !user.active }
        : user
    );
  }
  
  // Reactive computation using helper
  $: activeUsers = getActiveUsers(users);
  $: formattedUsers = users.map(formatUser);
</script>

<h3>Active Users: {activeUsers.length}</h3>

{#each formattedUsers as user}
  <div>
    {user.displayName}
    <button on:click={() => toggleUserStatus(user.id)}>
      {user.active ? 'Deactivate' : 'Activate'}
    </button>
  </div>
{/each}
```

**Validation Functions**
```svelte
<script>
  let email = '';
  let password = '';
  let errors = {};
  
  // Validation helper functions
  function validateEmail(value) {
    const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
    if (!value) return 'Email is required';
    if (!emailRegex.test(value)) return 'Invalid email format';
    return null;
  }
  
  function validatePassword(value) {
    if (!value) return 'Password is required';
    if (value.length < 8) return 'Password must be at least 8 characters';
    if (!/[A-Z]/.test(value)) return 'Password must contain uppercase letter';
    if (!/[0-9]/.test(value)) return 'Password must contain a number';
    return null;
  }
  
  function validateForm() {
    const newErrors = {};
    
    const emailError = validateEmail(email);
    if (emailError) newErrors.email = emailError;
    
    const passwordError = validatePassword(password);
    if (passwordError) newErrors.password = passwordError;
    
    errors = newErrors;
    return Object.keys(newErrors).length === 0;
  }
  
  function handleSubmit(event) {
    event.preventDefault();
    
    if (validateForm()) {
      console.log('Form is valid!', { email, password });
      // Submit to API
    } else {
      console.log('Form has errors:', errors);
    }
  }
  
  // Real-time validation (reactive)
  $: emailError = email ? validateEmail(email) : null;
  $: passwordError = password ? validatePassword(password) : null;
</script>

<form on:submit={handleSubmit}>
  <div>
    <input 
      type="email" 
      bind:value={email}
      placeholder="Email"
    />
    {#if emailError}
      <span class="error">{emailError}</span>
    {/if}
  </div>
  
  <div>
    <input 
      type="password" 
      bind:value={password}
      placeholder="Password"
    />
    {#if passwordError}
      <span class="error">{passwordError}</span>
    {/if}
  </div>
  
  <button type="submit">Sign Up</button>
</form>

<style>
  .error {
    color: red;
    font-size: 0.875rem;
  }
</style>
```

**Debouncing and Throttling Functions**
```svelte
<script>
  let searchQuery = '';
  let searchResults = [];
  let isSearching = false;
  
  // Debounce: wait for user to stop typing
  function debounce(fn, delay) {
    let timeoutId;
    return function(...args) {
      clearTimeout(timeoutId);
      timeoutId = setTimeout(() => fn(...args), delay);
    };
  }
  
  // Actual search function
  async function performSearch(query) {
    if (!query.trim()) {
      searchResults = [];
      return;
    }
    
    isSearching = true;
    try {
      // Simulate API call
      await new Promise(resolve => setTimeout(resolve, 500));
      searchResults = [
        `Result 1 for "${query}"`,
        `Result 2 for "${query}"`,
        `Result 3 for "${query}"`
      ];
    } catch (error) {
      console.error('Search failed:', error);
    } finally {
      isSearching = false;
    }
  }
  
  // Debounced version - only searches after 300ms of no typing
  const debouncedSearch = debounce(performSearch, 300);
  
  function handleSearchInput(event) {
    searchQuery = event.target.value;
    debouncedSearch(searchQuery);
  }
</script>

<input 
  type="search" 
  value={searchQuery}
  on:input={handleSearchInput}
  placeholder="Search..."
/>

{#if isSearching}
  <p>Searching...</p>
{:else if searchResults.length > 0}
  <ul>
    {#each searchResults as result}
      <li>{result}</li>
    {/each}
  </ul>
{:else if searchQuery}
  <p>No results found</p>
{/if}
```

**API Fetching Functions**
```svelte
<script>
  let users = [];
  let loading = false;
  let error = null;
  
  // Generic fetch wrapper with error handling
  async function fetchData(url, options = {}) {
    try {
      const response = await fetch(url, options);
      
      if (!response.ok) {
        throw new Error(`HTTP error! status: ${response.status}`);
      }
      
      return await response.json();
    } catch (err) {
      console.error('Fetch error:', err);
      throw err;
    }
  }
  
  // Specific API function
  async function loadUsers() {
    loading = true;
    error = null;
    
    try {
      users = await fetchData('https://jsonplaceholder.typicode.com/users');
    } catch (err) {
      error = err.message;
    } finally {
      loading = false;
    }
  }
  
  // Create user function
  async function createUser(userData) {
    loading = true;
    error = null;
    
    try {
      const newUser = await fetchData(
        'https://jsonplaceholder.typicode.com/users',
        {
          method: 'POST',
          headers: { 'Content-Type': 'application/json' },
          body: JSON.stringify(userData)
        }
      );
      
      users = [...users, newUser]; // Immutable update
      return newUser;
    } catch (err) {
      error = err.message;
      return null;
    } finally {
      loading = false;
    }
  }
  
  // Load on component mount (Svelte 5 uses $effect)
  import { onMount } from 'svelte';
  onMount(() => {
    loadUsers();
  });
</script>

{#if loading}
  <p>Loading users...</p>
{:else if error}
  <p class="error">Error: {error}</p>
  <button on:click={loadUsers}>Retry</button>
{:else}
  <ul>
    {#each users as user (user.id)}
      <li>{user.name} - {user.email}</li>
    {/each}
  </ul>
{/if}

<button on:click={() => createUser({ name: 'New User', email: 'new@example.com' })}>
  Add User
</button>
```

---

#### **10. Key Takeaways for Day 2**

**What you've learned:**

1. **Three ways to define functions** - declarations, expressions, and arrow functions
2. **Parameters and return values** - how data flows in and out of functions
3. **Default parameters and rest parameters** - handling flexible inputs
4. **Functions as values** - passing functions as arguments (callbacks)
5. **Scope and closures** - how functions "remember" their environment
6. **Real Svelte patterns** - event handlers, computed values, validation, API calls

**When to use each function type:**

| Type | Use When | Example |
|------|----------|---------|
| Declaration | Top-level utilities, need hoisting | `function calculateTotal(items) {...}` |
| Expression | Assigning to variable, no hoisting needed | `const handler = function() {...}` |
| Arrow Function | Callbacks, array methods, event handlers | `numbers.map(n => n * 2)` |
| Regular Method | Object methods that use `this` | `{ increment() { this.count++; } }` |

**Common beginner mistakes to avoid:**

```js
// ❌ Arrow function in object method
const obj = {
  count: 0,
  increment: () => {
    this.count++; // 'this' doesn't work here!
  }
};

// ✅ Use regular method syntax
const obj = {
  count: 0,
  increment() {
    this.count++;
  }
};

// ❌ Forgetting to return in multi-line arrow function
const double = n => {
  n * 2; // Missing return!
};

// ✅ Add explicit return
const double = n => {
  return n * 2;
};

// ❌ Modifying parameters (can cause bugs)
function addItem(array, item) {
  array.push(item); // Mutates original array!
  return array;
}

// ✅ Return new array (immutable)
function addItem(array, item) {
  return [...array, item];
}
```

**Practice challenge for Svelte:**

Build a simple todo app component that uses:
- Functions for adding/removing/toggling todos
- Event handlers for user interactions
- Helper functions for filtering (all/active/completed)
- Validation function to prevent empty todos

This will prepare you perfectly for Day 3 (Control Flow) where you'll add conditional rendering and loops!

---

### **Ready for Day 3?**

Day 2 complete! You now understand:
- ✅ How to write functions three different ways
- ✅ How to pass data in and out of functions
- ✅ How to use functions as values (callbacks)
- ✅ How closures work and why they matter
- ✅ Real-world Svelte function patterns

**Next up:** Day 3 covers Control Flow (if/else, loops, switch statements) - the logic that makes your applications dynamic!
    