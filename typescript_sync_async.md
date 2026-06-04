# TypeScript: Synchronous vs Asynchronous Code

## Overview

In TypeScript (and JavaScript), the key difference between **synchronous (sync)** and **asynchronous (async)** code is whether the program waits for an operation to finish before moving on to the next statement.

---

## Synchronous Code

Synchronous code executes **one statement at a time**. Each line must complete before the next line runs.

### Example

```typescript
function getData(): string {
  return "Hello";
}

console.log("Start");
const data = getData();
console.log(data);
console.log("End");
```

### Output

```text
Start
Hello
End
```

### Execution Flow

1. Print `"Start"`
2. Call `getData()`
3. Print the returned value
4. Print `"End"`

The program waits for each step to complete before continuing.

---

## Asynchronous Code

Asynchronous code allows operations to run in the background while the program continues executing other tasks.

### Example

```typescript
async function getData(): Promise<string> {
  return "Hello";
}

console.log("Start");

getData().then(data => {
  console.log(data);
});

console.log("End");
```

### Output

```text
Start
End
Hello
```

### Execution Flow

1. Print `"Start"`
2. Start the async operation
3. Continue immediately
4. Print `"End"`
5. Print `"Hello"` when the async operation completes

---

## Using `async` and `await`

TypeScript provides `async` and `await` to make asynchronous code easier to read and maintain.

### Example

```typescript
function delay(ms: number): Promise<void> {
  return new Promise(resolve => {
    setTimeout(resolve, ms);
  });
}

async function example() {
  console.log("Start");

  await delay(2000);

  console.log("After 2 seconds");
}

example();
```

### Output

```text
Start
After 2 seconds
```

### What Happens?

* `await` pauses execution **inside the async function**.
* The JavaScript runtime remains free to execute other tasks.
* Execution resumes when the Promise is resolved.

---

## Return Types

### Synchronous Function

```typescript
function add(a: number, b: number): number {
  return a + b;
}

const result = add(2, 3);
```

**Return Type**

```typescript
number
```

---

### Asynchronous Function

```typescript
async function add(a: number, b: number): Promise<number> {
  return a + b;
}

const result = await add(2, 3);
```

**Return Type**

```typescript
Promise<number>
```

> Any value returned from an `async` function is automatically wrapped in a `Promise`.

---

## Common Use Cases for Async Code

Use asynchronous code when working with operations that take time:

* API requests (`fetch`)
* Database queries
* Reading or writing files
* Network communication
* Timers (`setTimeout`)
* External services

### Example

```typescript
async function getUser(id: number) {
  const response = await fetch(`/api/users/${id}`);
  return response.json();
}
```

Without asynchronous programming, the application would block while waiting for the server response.

---

## Comparison Table

| Synchronous                      | Asynchronous                                                |
| -------------------------------- | ----------------------------------------------------------- |
| Executes one statement at a time | Allows waiting for long-running operations without blocking |
| Returns values directly          | Typically returns a `Promise`                               |
| Simpler execution flow           | Requires `async/await` or `.then()`                         |
| Can block execution              | Keeps applications responsive                               |
| Best for quick computations      | Best for I/O operations (API, DB, Files)                    |

---

## Mental Model

### Synchronous

```text
Start
  ↓
Do Work
  ↓
Finish
```

### Asynchronous

```text
Start
  ↓
Begin Long Task
  ↓
Continue Other Work
  ↓
Long Task Completes
  ↓
Handle Result
```

---

## Summary

* **Synchronous code** executes step-by-step and waits for each operation to finish.
* **Asynchronous code** allows long-running operations to complete without blocking the application.
* `async` functions always return a `Promise`.
* `await` makes asynchronous code easier to read by allowing asynchronous operations to be written in a sequential style.
* Most modern TypeScript applications rely heavily on asynchronous programming for APIs, databases, files, and network communication.
