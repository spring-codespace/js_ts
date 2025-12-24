# Day 3 — Control Flow: Making Decisions and Repeating Actions

**Why This Matters for Svelte:** Control flow is the backbone of dynamic UIs. Every conditional render (`{#if}`), every list (`{#each}`), every dynamic class or style uses the concepts you'll learn today. Mastering control flow means mastering how your app responds to user actions and data changes.

---

## Table of Contents
1. [Conditional Statements](#1-conditional-statements)
2. [Loops](#2-loops)
3. [Control Flow in Svelte](#3-control-flow-in-svelte)
4. [Practice Exercises](#4-practice-exercises)
5. [Real-World Svelte Components](#5-real-world-svelte-components)
6. [Key Takeaways](#6-key-takeaways)

---

## 1. Conditional Statements: Making Decisions

Conditional statements let your code make decisions based on conditions. They're fundamental to any interactive application.

### **If Statement: The Basic Decision Maker**

The `if` statement runs code only when a condition is true.

```js
let age = 20;

if (age >= 18) {
  console.log("You can vote!");
}

// Condition can be any expression that evaluates to true/false
let isLoggedIn = true;
if (isLoggedIn) {
  console.log("Welcome back!");
}

// Multiple conditions with && (AND) and || (OR)
let hasPermission = true;
let isActive = true;

if (isLoggedIn && hasPermission) {
  console.log("Access granted");
}

if (isLoggedIn || hasPermission) {
  console.log("At least one condition is true");
}
```

### **Understanding Truthy/Falsy in Conditions**

This is one of the most important concepts in JavaScript!

```js
// These all evaluate to FALSE in conditions:
if (false) { }        // obviously false
if (0) { }            // zero is falsy
if ("") { }           // empty string is falsy
if (null) { }         // null is falsy
if (undefined) { }    // undefined is falsy
if (NaN) { }          // NaN is falsy

// Everything else is TRUE, including:
if (true) { }         // obviously true
if (1) { }            // any non-zero number
if (-1) { }           // even negative numbers
if ("hello") { }      // any non-empty string
if ("0") { }          // string "0" is truthy!
if ([]) { }           // empty array is truthy!
if ({}) { }           // empty object is truthy!

// Practical example
let username = "";

if (username) {
  console.log(`Welcome, ${username}!`);
} else {
  console.log("Please log in");
}
```

### **If-Else: Two Alternative Paths**

```js
let temperature = 25;

if (temperature > 30) {
  console.log("It's hot! 🌞");
} else {
  console.log("It's comfortable 😊");
}

// With multiple statements
let score = 75;

if (score >= 60) {
  console.log("Passed!");
  console.log("Congratulations!");
} else {
  console.log("Failed");
  console.log("Try again next time");
}
```

### **If-Else If-Else: Multiple Conditions**

When you have more than two possibilities:

```js
let score = 85;

if (score >= 90) {
  console.log("Grade: A 🌟");
} else if (score >= 80) {
  console.log("Grade: B 📘");
} else if (score >= 70) {
  console.log("Grade: C 📗");
} else if (score >= 60) {
  console.log("Grade: D 📙");
} else {
  console.log("Grade: F ❌");
}

// Important: only the first matching condition runs!
let age = 25;

if (age >= 18) {
  console.log("Adult"); // This runs
} else if (age >= 21) {
  console.log("Can drink in US"); // This NEVER runs, even though it's true!
} else {
  console.log("Minor");
}

// To check multiple independent conditions, use separate if statements
if (age >= 18) {
  console.log("Adult");
}
if (age >= 21) {
  console.log("Can drink in US");
}
```

### **Nested If Statements**

```js
let isLoggedIn = true;
let hasSubscription = true;
let subscriptionType = "premium";

if (isLoggedIn) {
  console.log("User is logged in");
  
  if (hasSubscription) {
    console.log("User has subscription");
    
    if (subscriptionType === "premium") {
      console.log("Show premium content 💎");
    } else {
      console.log("Show basic content 📄");
    }
  } else {
    console.log("Show free content");
  }
} else {
  console.log("Please log in");
}

// Better: flatten with early returns (in functions)
function getContentType(isLoggedIn, hasSubscription, subscriptionType) {
  if (!isLoggedIn) return "login-required";
  if (!hasSubscription) return "free";
  if (subscriptionType === "premium") return "premium";
  return "basic";
}
```

### **Comparison Operators**

Understanding comparison operators is crucial for writing conditions:

```js
// Equality
console.log(5 == 5);     // true (loose equality, type coercion)
console.log(5 == "5");   // true (string "5" coerced to number)
console.log(5 === 5);    // true (strict equality, no coercion)
console.log(5 === "5");  // false (different types)

// ALWAYS use === (strict) in your code!
// The only exception: checking for null/undefined

// Inequality
console.log(5 != 6);     // true (loose inequality)
console.log(5 !== "5");  // true (strict inequality)

// Greater/Less than
console.log(10 > 5);     // true
console.log(10 >= 10);   // true (greater than or equal)
console.log(5 < 10);     // true
console.log(5 <= 5);     // true (less than or equal)

// Logical operators
let x = 5;
console.log(x > 3 && x < 10);   // true (both conditions must be true)
console.log(x > 10 || x < 3);   // false (at least one must be true)
console.log(!(x > 10));         // true (negation/NOT)

// Short-circuit evaluation
let user = null;
let name = user && user.name; // user is falsy, so returns null (no error!)

let defaultName = username || "Guest"; // If username is falsy, use "Guest"
```

**Common Pitfall: Assignment vs Comparison**

```js
let x = 5;

// ❌ WRONG - this assigns 10 to x, doesn't compare!
if (x = 10) {
  console.log("This always runs!"); // x is now 10
}

// ✅ CORRECT - this compares x to 10
if (x === 10) {
  console.log("x equals 10");
}
```

### **Switch Statement: Multiple Cases**

Use `switch` when you have many possible values for a single variable:

```js
let day = "Monday";

switch (day) {
  case "Monday":
    console.log("Start of work week 😫");
    break; // Important! Without break, it continues to next case
  case "Tuesday":
  case "Wednesday":
  case "Thursday":
    console.log("Middle of week 📅");
    break;
  case "Friday":
    console.log("Almost weekend! 🎉");
    break;
  case "Saturday":
  case "Sunday":
    console.log("Weekend! 🏖️");
    break;
  default:
    console.log("Invalid day");
}

// Without break, it "falls through" to next case
let grade = "B";

switch (grade) {
  case "A":
    console.log("Excellent!");
  case "B":
    console.log("Good job!"); // This runs
  case "C":
    console.log("Passed"); // This also runs!
  case "D":
    console.log("Barely passed"); // This too!
    break; // Finally stops here
  default:
    console.log("Failed");
}
```

**When to use Switch vs If-Else:**

```js
// ✅ Good use of switch - checking one variable against many values
switch (color) {
  case "red":
  case "blue":
  case "green":
    // handle cases
}

// ❌ Bad use of switch - complex conditions
switch (true) {
  case age > 18 && hasPermission:
  case isAdmin:
    // This is confusing, use if-else instead
}

// ✅ Use if-else for complex conditions
if (age > 18 && hasPermission) {
  // ...
} else if (isAdmin) {
  // ...
}
```

### **Ternary Operator: Inline Conditionals**

The ternary operator is a shorthand for simple if-else statements:

```js
// Basic syntax: condition ? valueIfTrue : valueIfFalse
let age = 20;
let status = age >= 18 ? "Adult" : "Minor";
console.log(status); // "Adult"

// Traditional if-else equivalent:
let status;
if (age >= 18) {
  status = "Adult";
} else {
  status = "Minor";
}

// Practical examples
let isLoggedIn = true;
let greeting = isLoggedIn ? "Welcome back!" : "Please log in";

let score = 75;
let result = score >= 60 ? "Pass ✅" : "Fail ❌";

let items = 5;
let message = items === 0 ? "Cart is empty" 
            : items === 1 ? "1 item in cart"
            : `${items} items in cart`;

// In function returns
function getDiscount(isMember) {
  return isMember ? 0.15 : 0.05; // 15% or 5%
}

// In JSX/Svelte (very common!)
let showModal = true;
// In template: {showModal ? <Modal /> : <Button />}
```

**Nested Ternaries (Use Sparingly!)**

```js
// ❌ Hard to read
let message = age < 13 ? "child" 
            : age < 18 ? "teenager"
            : age < 65 ? "adult"
            : "senior";

// ✅ Better - use if-else for multiple conditions
let message;
if (age < 13) message = "child";
else if (age < 18) message = "teenager";
else if (age < 65) message = "adult";
else message = "senior";

// ✅ Ternary is great for simple cases
let access = isPremium ? "full" : "limited";
```

### **Nullish Coalescing (??): Default Values**

```js
// ?? returns right side only if left is null or undefined
let username = null;
let display = username ?? "Guest"; // "Guest"

let count = 0;
let amount = count ?? 10; // 0 (not 10! because 0 is not null/undefined)

// Compare with || (logical OR)
let amount2 = count || 10; // 10 (because 0 is falsy)

// When to use which:
let value1 = "" || "default";    // "default" (empty string is falsy)
let value2 = "" ?? "default";    // "" (empty string is not null/undefined)

let value3 = 0 || 100;           // 100 (0 is falsy)
let value4 = 0 ?? 100;           // 0 (0 is not null/undefined)

// Practical examples
let userSettings = {
  theme: null,
  notifications: false
};

let theme = userSettings.theme ?? "light"; // "light"
let notifications = userSettings.notifications ?? true; // false (not true!)
```

### **Optional Chaining (?.): Safe Property Access**

```js
let user = {
  name: "Alice",
  address: {
    city: "Stockholm"
  }
};

// Without optional chaining
let city;
if (user && user.address && user.address.city) {
  city = user.address.city;
}

// With optional chaining (much cleaner!)
let city = user?.address?.city; // "Stockholm"

// If any part is null/undefined, returns undefined (no error!)
let user2 = null;
let city2 = user2?.address?.city; // undefined (no error!)

// Works with arrays
let users = [{ name: "Alice" }, { name: "Bob" }];
let firstUserName = users?.[0]?.name; // "Alice"

// Works with function calls
let obj = {
  greet: function() { return "Hello!"; }
};
let greeting = obj.greet?.(); // "Hello!"
let missing = obj.goodbye?.(); // undefined (no error!)

// Combining ?? and ?.
let userName = user?.profile?.name ?? "Anonymous";
```

---

## 2. Loops: Repeating Actions

Loops let you execute code multiple times. They're essential for working with arrays and collections.

### **For Loop: Traditional Counter-Based Loop**

```js
// Basic syntax: for (initialization; condition; update)
for (let i = 0; i < 5; i++) {
  console.log(i); // 0, 1, 2, 3, 4
}

// How it works:
// 1. let i = 0 - runs once at start
// 2. i < 5 - checked before each iteration
// 3. console.log(i) - loop body runs
// 4. i++ - runs after each iteration
// 5. Back to step 2

// Loop through array by index
let fruits = ["apple", "banana", "orange"];
for (let i = 0; i < fruits.length; i++) {
  console.log(`${i}: ${fruits[i]}`);
}
// 0: apple
// 1: banana
// 2: orange

// Counting backwards
for (let i = 5; i > 0; i--) {
  console.log(i); // 5, 4, 3, 2, 1
}
console.log("Blast off! 🚀");

// Skip by 2
for (let i = 0; i < 10; i += 2) {
  console.log(i); // 0, 2, 4, 6, 8
}

// Nested loops (be careful - can be slow!)
for (let i = 0; i < 3; i++) {
  for (let j = 0; j < 3; j++) {
    console.log(`i=${i}, j=${j}`);
  }
}
// i=0, j=0
// i=0, j=1
// i=0, j=2
// i=1, j=0
// ...
```

**Common For Loop Patterns:**

```js
// Sum array of numbers
let numbers = [1, 2, 3, 4, 5];
let sum = 0;
for (let i = 0; i < numbers.length; i++) {
  sum += numbers[i];
}
console.log(sum); // 15

// Find maximum value
let max = numbers[0];
for (let i = 1; i < numbers.length; i++) {
  if (numbers[i] > max) {
    max = numbers[i];
  }
}
console.log(max); // 5

// Build a new array (modern way is .map() - Day 5)
let doubled = [];
for (let i = 0; i < numbers.length; i++) {
  doubled.push(numbers[i] * 2);
}
console.log(doubled); // [2, 4, 6, 8, 10]
```

### **For...Of Loop: Iterate Over Values**

Modern, cleaner way to loop through arrays:

```js
let fruits = ["apple", "banana", "orange"];

// For...of gives you the value directly
for (const fruit of fruits) {
  console.log(fruit);
}
// apple
// banana
// orange

// Works with strings (treats them as arrays of characters)
let word = "hello";
for (const char of word) {
  console.log(char); // h, e, l, l, o
}

// Getting index with entries()
for (const [index, fruit] of fruits.entries()) {
  console.log(`${index}: ${fruit}`);
}
// 0: apple
// 1: banana
// 2: orange

// Array of objects
let users = [
  { name: "Alice", age: 25 },
  { name: "Bob", age: 30 },
  { name: "Charlie", age: 22 }
];

for (const user of users) {
  console.log(`${user.name} is ${user.age} years old`);
}
```

**For vs For...Of:**

```js
let numbers = [10, 20, 30];

// Traditional for - when you need the index
for (let i = 0; i < numbers.length; i++) {
  console.log(`Index ${i}: ${numbers[i]}`);
}

// For...of - when you only need the value (cleaner!)
for (const num of numbers) {
  console.log(num);
}

// ❌ Can't modify array length safely in for...of
let items = [1, 2, 3];
for (const item of items) {
  items.push(item * 2); // ⚠️ Infinite loop!
}

// ✅ Traditional for is safer for modifying during iteration
for (let i = 0; i < 3; i++) {
  items.push(items[i] * 2);
}
```

### **For...In Loop: Iterate Over Object Keys**

Use for...in to loop through object properties:

```js
let person = {
  name: "Alice",
  age: 25,
  city: "Stockholm"
};

// For...in gives you the keys
for (const key in person) {
  console.log(`${key}: ${person[key]}`);
}
// name: Alice
// age: 25
// city: Stockholm

// Get all keys
let keys = [];
for (const key in person) {
  keys.push(key);
}
console.log(keys); // ["name", "age", "city"]

// Get all values
let values = [];
for (const key in person) {
  values.push(person[key]);
}
console.log(values); // ["Alice", 25, "Stockholm"]

// Modern alternative: Object methods (preferred!)
console.log(Object.keys(person));     // ["name", "age", "city"]
console.log(Object.values(person));   // ["Alice", 25, "Stockholm"]
console.log(Object.entries(person));  // [["name", "Alice"], ["age", 25], ...]

// Looping over Object.entries (best practice)
for (const [key, value] of Object.entries(person)) {
  console.log(`${key}: ${value}`);
}
```

**⚠️ Warning: Don't use for...in with arrays!**

```js
let arr = ["a", "b", "c"];

// ❌ Bad - for...in gives you indices as strings
for (const index in arr) {
  console.log(typeof index); // "string"
  console.log(index); // "0", "1", "2"
}

// ✅ Good - for...of gives you values
for (const value of arr) {
  console.log(value); // "a", "b", "c"
}

// ✅ Or traditional for if you need numeric index
for (let i = 0; i < arr.length; i++) {
  console.log(i); // 0, 1, 2 (numbers)
}
```

### **While Loop: Repeat While Condition is True**

```js
// Basic while loop
let count = 0;
while (count < 5) {
  console.log(count);
  count++; // Don't forget to increment!
}

// Reading from a list until empty
let tasks = ["task1", "task2", "task3"];
while (tasks.length > 0) {
  let task = tasks.pop();
  console.log(`Completed: ${task}`);
}

// User input simulation
let password = "";
while (password !== "secret") {
  // In browser: password = prompt("Enter password:");
  console.log("Access denied");
  break; // Prevent infinite loop in this example
}

// ⚠️ Be careful of infinite loops!
// while (true) {
//   console.log("This never stops!"); // ❌ Will freeze your program
// }
```

### **Do...While Loop: Run At Least Once**

```js
// Runs once before checking condition
let i = 0;
do {
  console.log(i);
  i++;
} while (i < 5);
// 0, 1, 2, 3, 4

// Difference from while: runs at least once even if condition is false
let x = 10;

// while - doesn't run at all
while (x < 5) {
  console.log("While: " + x);
}

// do-while - runs once
do {
  console.log("Do-while: " + x);
} while (x < 5);
// Output: "Do-while: 10"

// Practical use: menu systems
let choice;
do {
  // Show menu
  console.log("1. Start game");
  console.log("2. Load game");
  console.log("3. Quit");
  // choice = getUserInput();
  choice = 3; // Simulate user input
} while (choice !== 3);
```

### **Break and Continue: Loop Control**

```js
// break - exit loop completely
for (let i = 0; i < 10; i++) {
  if (i === 5) {
    break; // Stop the loop
  }
  console.log(i); // 0, 1, 2, 3, 4
}

// Practical: searching for an item
let users = [
  { id: 1, name: "Alice" },
  { id: 2, name: "Bob" },
  { id: 3, name: "Charlie" }
];

let foundUser;
for (const user of users) {
  if (user.id === 2) {
    foundUser = user;
    break; // Stop searching once found
  }
}
console.log(foundUser); // { id: 2, name: "Bob" }

// continue - skip to next iteration
for (let i = 0; i < 10; i++) {
  if (i % 2 === 0) {
    continue; // Skip even numbers
  }
  console.log(i); // 1, 3, 5, 7, 9
}

// Practical: filter during loop
let numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
let oddNumbers = [];
for (const num of numbers) {
  if (num % 2 === 0) continue; // Skip evens
  oddNumbers.push(num);
}
console.log(oddNumbers); // [1, 3, 5, 7, 9]

// Break with labels (rare, but useful for nested loops)
outer: for (let i = 0; i < 3; i++) {
  for (let j = 0; j < 3; j++) {
    if (i === 1 && j === 1) {
      break outer; // Break out of both loops
    }
    console.log(`i=${i}, j=${j}`);
  }
}
```

---

## 3. Control Flow in Svelte

Now let's see how these concepts translate to Svelte templates:

### **Conditional Rendering with {#if}**

```svelte
<script>
  let isLoggedIn = false;
  let user = { name: "Alice", role: "admin" };
  let score = 85;
</script>

<!-- Basic if -->
{#if isLoggedIn}
  <p>Welcome back!</p>
{/if}

<!-- If-else -->
{#if isLoggedIn}
  <button>Logout</button>
{:else}
  <button>Login</button>
{/if}

<!-- If-else if-else -->
{#if score >= 90}
  <div class="grade-a">Grade: A</div>
{:else if score >= 80}
  <div class="grade-b">Grade: B</div>
{:else if score >= 70}
  <div class="grade-c">Grade: C</div>
{:else}
  <div class="grade-f">Grade: F</div>
{/if}

<!-- Nested conditions -->
{#if isLoggedIn}
  <nav>
    {#if user.role === "admin"}
      <a href="/admin">Admin Panel</a>
    {/if}
    <a href="/profile">Profile</a>
  </nav>
{:else}
  <button>Please log in</button>
{/if}

<!-- Ternary in attributes -->
<button class={isLoggedIn ? "logout-btn" : "login-btn"}>
  {isLoggedIn ? "Logout" : "Login"}
</button>

<!-- Conditional classes (Svelte shorthand) -->
<div class:active={isLoggedIn} class:disabled={!isLoggedIn}>
  Status
</div>
```

### **Lists with {#each}**

```svelte
<script>
  let fruits = ["Apple", "Banana", "Orange"];
  
  let users = [
    { id: 1, name: "Alice", age: 25, active: true },
    { id: 2, name: "Bob", age: 30, active: false },
    { id: 3, name: "Charlie", age: 22, active: true }
  ];
  
  let emptyArray = [];
</script>

<!-- Basic each loop -->
<ul>
  {#each fruits as fruit}
    <li>{fruit}</li>
  {/each}
</ul>

<!-- With index -->
<ul>
  {#each fruits as fruit, index}
    <li>{index + 1}. {fruit}</li>
  {/each}
</ul>

<!-- Array of objects with key (important for performance!) -->
<ul>
  {#each users as user (user.id)}
    <li>
      {user.name} - {user.age} years old
      {#if user.active}
        <span class="badge">Active</span>
      {/if}
    </li>
  {/each}
</ul>

<!-- Empty state with {:else} -->
{#each users as user}
  <div class="user-card">{user.name}</div>
{:else}
  <p>No users found</p>
{/each}

<!-- Destructuring in each -->
{#each users as { id, name, age, active } (id)}
  <div>
    {name} ({age})
    <span class:active></span>
  </div>
{/each}

<!-- Nested loops -->
<div class="grid">
  {#each Array(3) as _, row}
    <div class="row">
      {#each Array(3) as _, col}
        <div class="cell">
          {row},{col}
        </div>
      {/each}
    </div>
  {/each}
</div>
```

### **Filtering and Mapping in Templates**

```svelte
<script>
  let todos = [
    { id: 1, text: "Learn JS", done: true },
    { id: 2, text: "Learn Svelte", done: false },
    { id: 3, text: "Build app", done: false }
  ];
  
  let filter = "all"; // "all", "active", "completed"
  
  // Reactive filtered list
  $: filteredTodos = todos.filter(todo => {
    if (filter === "active") return !todo.done;
    if (filter === "completed") return todo.done;
    return true;
  });
  
  $: completedCount = todos.filter(t => t.done).length;
  $: activeCount = todos.length - completedCount;
</script>

<!-- Filter buttons -->
<div class="filters">
  <button 
    class:active={filter === "all"}
    on:click={() => filter = "all"}
  >
    All ({todos.length})
  </button>
  <button 
    class:active={filter === "active"}
    on:click={() => filter = "active"}
  >
    Active ({activeCount})
  </button>
  <button 
    class:active={filter === "completed"}
    on:click={() => filter = "completed"}
  >
    Completed ({completedCount})
  </button>
</div>

<!-- Render filtered todos -->
{#each filteredTodos as todo (todo.id)}
  <div class="todo" class:done={todo.done}>
    <input 
      type="checkbox" 
      checked={todo.done}
      on:change={() => {
        todos = todos.map(t => 
          t.id === todo.id ? {...t, done: !t.done} : t
        );
      }}
    />
    <span>{todo.text}</span>
  </div>
{:else}
  <p>No {filter} todos</p>
{/each}

<style>
  .filters button.active {
    background: blue;
    color: white;
  }
  .todo.done {
    opacity: 0.5;
    text-decoration: line-through;
  }
</style>
```

---

## 4. Practice Exercises

### **Exercise 1: Grade Calculator**

```js
function getGrade(score) {
  if (score >= 90) return "A";
  if (score >= 80) return "B";
  if (score >= 70) return "C";
  if (score >= 60) return "D";
  return "F";
}

function getGradeMessage(score) {
  const grade = getGrade(score);
  
  switch (grade) {
    case "A":
      return "Excellent work! 🌟";
    case "B":
      return "Great job! 👍";
    case "C":
      return "Good effort! 📝";
    case "D":
      return "Passed, but needs improvement ⚠️";
    default:
      return "Failed. Study harder! 📚";
  }
}

console.log(getGradeMessage(85)); // "Great job! 👍"
```

### **Exercise 2: Filter Array**

```js
// Filter array to get only even numbers
const numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];

// Using for loop
const evens = [];
for (const num of numbers) {
  if (num % 2 === 0) {
    evens.push(num);
  }
}
console.log(evens); // [2, 4, 6, 8, 10]

// Filter students who passed
const students = [
  { name: "Alice", score: 85 },
  { name: "Bob", score: 55 },
  { name: "Charlie", score: 92 },
  { name: "David", score: 48 }
];

const passed = [];
for (const student of students) {
  if (student.score >= 60) {
    passed.push(student);
  }
}

### **Exercise 3: Loop Through Objects**

```js
const users = {
  Alice: { age: 25, active: true },
  Bob: { age: 30, active: false },
  Charlie: { age: 22, active: true }
};

// List all users with their status
for (const [name, info] of Object.entries(users)) {
  console.log(`${name} is ${info.age} years old and ${info.active ? "active" : "inactive"}`);
}

// Output:
// Alice is 25 years old and active
// Bob is 30 years old and inactive
// Charlie is 22 years old and active
```

### **Exercise 4: Svelte Conditional Rendering**

```svelte
<script>
  let loggedIn = false;
  let user = { name: "Alice", role: "admin" };
</script>

{#if loggedIn}
  <p>Welcome, {user.name}!</p>
  {#if user.role === "admin"}
    <p>You have admin access</p>
  {/if}
{:else}
  <p>Please log in to continue</p>
{/if}
```

### **Exercise 5: Svelte List Rendering**

```svelte
<script>
  let todos = [
    { id: 1, text: "Learn Svelte", done: false },
    { id: 2, text: "Build a project", done: true }
  ];
</script>

<ul>
  {#each todos as {id, text, done} (id)}
    <li class:done={done}>
      {text} {done ? "✅" : "❌"}
    </li>
  {/each}
</ul>

<style>
  .done {
    text-decoration: line-through;
    opacity: 0.6;
  }
</style>
```

---

## 5. Real-World Svelte Components

### **1. Dynamic Navigation Menu**

```svelte
<script>
  let user = { loggedIn: true, role: "editor" };
</script>

<nav>
  <a href="/">Home</a>
  {#if user.loggedIn}
    <a href="/dashboard">Dashboard</a>
    {#if user.role === "admin"}
      <a href="/admin">Admin</a>
    {/if}
    <button>Logout</button>
  {:else}
    <button>Login</button>
  {/if}
</nav>
```

### **2. Todo List with Filters**

```svelte
<script>
  let todos = [
    { id: 1, text: "Learn JS", done: true },
    { id: 2, text: "Learn Svelte", done: false }
  ];
  let filter = "all";

  $: filtered = todos.filter(t => 
    filter === "all" ? true : filter === "active" ? !t.done : t.done
  );
</script>

<div class="filters">
  <button on:click={() => filter = "all"} class:active={filter === "all"}>All</button>
  <button on:click={() => filter = "active"} class:active={filter === "active"}>Active</button>
  <button on:click={() => filter = "completed"} class:active={filter === "completed"}>Completed</button>
</div>

<ul>
  {#each filtered as {id, text, done} (id)}
    <li class:done={done}>{text}</li>
  {/each}
</ul>

<style>
  .filters button.active {
    font-weight: bold;
  }
  li.done {
    text-decoration: line-through;
  }
</style>
```

### **3. Nested Grids / Tables**

```svelte
<script>
  let rows = 3;
  let cols = 3;
</script>

<div class="grid">
  {#each Array(rows) as _, r}
    <div class="row">
      {#each Array(cols) as _, c}
        <div class="cell">{r},{c}</div>
      {/each}
    </div>
  {/each}
</div>

<style>
  .grid {
    display: flex;
    flex-direction: column;
  }
  .row {
    display: flex;
  }
  .cell {
    width: 50px;
    height: 50px;
    border: 1px solid #ccc;
    display: flex;
    justify-content: center;
    align-items: center;
  }
</style>
```

---

## 6. Key Takeaways

1. **Conditional statements** let your app make decisions (`if`, `else`, `else if`, `switch`, ternary).
2. **Truthy/falsy values** are essential to understand for proper conditions.
3. **Loops** (`for`, `for...of`, `for...in`, `while`, `do...while`) allow repeated execution and iterating collections.
4. **Break & continue** help control loop flow.
5. **Svelte control flow blocks** (`{#if}`, `{#each}`) map directly from JavaScript concepts.
6. **Nested and filtered rendering** in Svelte improves UX (e.g., nested menus, filtered lists).
7. **Ternary operators, optional chaining, and nullish coalescing** make code concise and safer.
8. **Performance tips**:

   * Use keys in `{#each}` loops.
   * Avoid deep nested loops when possible.
   * Flatten nested ifs with early returns in JS functions.

---