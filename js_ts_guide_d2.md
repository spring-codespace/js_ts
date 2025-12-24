# **Day 2 — Functions: Reusable Code Blocks**

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
    