# **Day 1 — Variables, Data Types & Basic Operations**

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
