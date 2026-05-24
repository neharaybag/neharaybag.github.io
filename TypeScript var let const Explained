# Understanding `var`, `let`, and `const` in TypeScript

When learning TypeScript or JavaScript, one of the most important concepts is understanding the difference between `var`, `let`, and `const`.

---

# 1. `var`

`var` allows:
- Redeclaration ✅
- Reassignment ✅

## Example

```ts
var count = 2
console.log(count)

var count = 4
console.log(count)

count = 6
console.log(count)
```

## Output

```ts
2
4
6
```

## Explanation

- `var count = 2` → variable created
- `var count = 4` → redeclared successfully
- `count = 6` → reassigned successfully

So, `var` supports both redeclaration and reassignment.

---

# 2. `let`

`let` allows:
- Redeclaration ❌
- Reassignment ✅

---

## Example 1 — Reassignment Allowed

```ts
let count = 2
console.log(count)

count = 4
console.log(count)
```

## Output

```ts
2
4
```

---

## Example 2 — Redeclaration Not Allowed

```ts
let count = 2
console.log(count)

let count = 4
```

## Error

```ts
Cannot redeclare block-scoped variable 'count'.
```

## Explanation

With `let`:
- You can change the value
- But you cannot declare the same variable again in the same scope

---

# 3. `const`

`const` allows:
- Redeclaration ❌
- Reassignment ❌

---

## Example 1 — Valid

```ts
const country = "India"
console.log(country)
```

## Output

```ts
India
```

---

## Example 2 — Reassignment Not Allowed

```ts
const country = "India"
console.log(country)

country = "Bharat"
```

## Error

```ts
Cannot assign to 'country' because it is a constant.
```

---

## Example 3 — Redeclaration Not Allowed

```ts
const country = "India"
console.log(country)

const country = "Bharat"
```

## Error

```ts
Cannot redeclare block-scoped variable 'country'.
```

---

# Quick Comparison Table

| Feature | var | let | const |
|---|---|---|---|
| Redeclare | ✅ Yes | ❌ No | ❌ No |
| Reassign | ✅ Yes | ✅ Yes | ❌ No |
| Scope | Function Scope | Block Scope | Block Scope |

---

# Best Practice

- Use `const` by default
- Use `let` when value needs to change
- Avoid `var` in modern TypeScript

---

# Interview One-Line Answer

- `var` → redeclare + reassign
- `let` → reassign only
- `const` → neither redeclare nor reassign

