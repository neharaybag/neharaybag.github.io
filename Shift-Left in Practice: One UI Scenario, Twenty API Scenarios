# Shift-Left in Practice: One UI Scenario, Twenty API Scenarios

*Part of [Beyond the Green Checkmark](https://neharaybag.github.io/)*

## The pattern I see in every legacy suite

Open almost any mature UI automation suite and you'll find the same anti-pattern: a signup or login form with 15–20 UI test cases, each one driving a full browser to check a single validation rule.

- Empty username shows an error
- Empty password shows an error
- Invalid email format shows an error
- Password under 8 characters shows an error
- Username with special characters is rejected
- Duplicate email is rejected
- ...and so on

Every one of these spins up a browser, loads the page, fills a form, and asserts an error message. At 5–8 seconds per test with browser overhead, that's 2 minutes of CI time spent re-verifying the *same* form-rendering and error-display logic, over and over, with only the input data changing.

That's not what UI tests are for. The UI's job here is to prove the **integration** works — that the field exists, that it's wired to a validator, that an error renders correctly on screen. It is not the right layer to enumerate every validation *rule*. That enumeration belongs at the API (or unit) level, where it runs in milliseconds and doesn't depend on a browser at all.

This is Shift-Left in its most concrete, defensible form: not a slogan, but a specific decision about which layer should own which assertion.

## The principle

> **One UI scenario proves the wiring works. Every other input variation belongs at the API layer.**

Concretely:

| Layer | What it should verify |
|---|---|
| **UI (1 happy path + 1 error-display test)** | The form renders, accepts input, calls the right endpoint, and displays *some* error correctly when the backend rejects it |
| **API (everything else)** | Every validation rule: required fields, format rules, length boundaries, duplicate checks, special characters, business rules |

The UI doesn't need to know *which* validation rule fired — it just needs to prove that when the backend says "invalid," the UI shows it. That single contract test buys you the right to delete dozens of redundant UI cases.

## Example: a signup form

Say you have a signup form with: `username` (required, 3–20 chars, alphanumeric), `email` (required, valid format, unique), `password` (required, min 8 chars, must include a number).

### Before — the old way (UI-heavy)

```ts
// 8+ separate UI tests, each driving a full browser
test('empty username shows error', async ({ page }) => { /* ...fill form, submit, assert... */ });
test('username too short shows error', async ({ page }) => { /* ... */ });
test('username with special chars shows error', async ({ page }) => { /* ... */ });
test('empty email shows error', async ({ page }) => { /* ... */ });
test('invalid email format shows error', async ({ page }) => { /* ... */ });
test('duplicate email shows error', async ({ page }) => { /* ... */ });
test('password too short shows error', async ({ page }) => { /* ... */ });
test('password without number shows error', async ({ page }) => { /* ... */ });
```

Eight browser launches. Eight page loads. Eight times re-proving that the form renders and displays errors — when only the *input data* and *expected message* actually differ.

### After — shifted left

**One UI test** proves the end-to-end wiring:

```ts
import { test, expect } from '@playwright/test';

test('signup form: valid input succeeds, invalid input shows an error @smoke', async ({ page }) => {
  await page.goto('/signup');

  // Happy path: proves the form submits and navigates on success
  await page.fill('#username', 'validuser123');
  await page.fill('#email', 'valid@example.com');
  await page.fill('#password', 'Password123');
  await page.click('#submit');
  await expect(page).toHaveURL(/dashboard/);
});

test('signup form: backend rejection renders an error message', async ({ page }) => {
  await page.goto('/signup');

  // Doesn't matter *which* rule fires — proves the UI displays whatever the API returns
  await page.fill('#username', ''); // any invalid input triggers this path
  await page.fill('#email', 'valid@example.com');
  await page.fill('#password', 'Password123');
  await page.click('#submit');

  await expect(page.locator('[data-test="error"]')).toBeVisible();
});
```

**The other 18 scenarios** move to the API layer, where they run in milliseconds with no browser at all:

```ts
import { test, expect } from '@playwright/test';

test.describe('Signup validation rules', () => {
  test('rejects empty username', async ({ request }) => {
    const res = await request.post('/api/signup', {
      data: { username: '', email: 'a@b.com', password: 'Password123' },
    });
    expect(res.status()).toBe(400);
    expect((await res.json()).error).toMatch(/username/i);
  });

  test('rejects username under 3 characters', async ({ request }) => {
    const res = await request.post('/api/signup', {
      data: { username: 'ab', email: 'a@b.com', password: 'Password123' },
    });
    expect(res.status()).toBe(400);
  });

  test('rejects username with special characters', async ({ request }) => {
    const res = await request.post('/api/signup', {
      data: { username: 'inv@lid!', email: 'a@b.com', password: 'Password123' },
    });
    expect(res.status()).toBe(400);
  });

  test('rejects invalid email format', async ({ request }) => {
    const res = await request.post('/api/signup', {
      data: { username: 'validuser', email: 'not-an-email', password: 'Password123' },
    });
    expect(res.status()).toBe(400);
  });

  test('rejects duplicate email', async ({ request }) => {
    await request.post('/api/signup', {
      data: { username: 'firstuser', email: 'dup@example.com', password: 'Password123' },
    });
    const res = await request.post('/api/signup', {
      data: { username: 'seconduser', email: 'dup@example.com', password: 'Password123' },
    });
    expect(res.status()).toBe(409);
  });

  test('rejects password under 8 characters', async ({ request }) => {
    const res = await request.post('/api/signup', {
      data: { username: 'validuser', email: 'a@b.com', password: 'Pas1' },
    });
    expect(res.status()).toBe(400);
  });

  test('rejects password without a number', async ({ request }) => {
    const res = await request.post('/api/signup', {
      data: { username: 'validuser', email: 'a@b.com', password: 'Password' },
    });
    expect(res.status()).toBe(400);
  });

  // ...continue for every boundary and rule. Each one runs in well under 100ms.
});
```

## What this actually saves

Rough numbers from a suite I migrated this way:

| | Before (all UI) | After (1 UI + N API) |
|---|---|---|
| Scenarios for one form | 8 | 2 UI + 8 API |
| Approx. runtime | ~50–60s (8 × ~6–7s incl. browser overhead) | ~12s UI + ~1s API ≈ 13s |
| Flakiness surface | High — every test touches DOM timing, animations, network | Low — API tests have no DOM, no rendering, no waits |
| Parallelization | Limited by browser instances | Trivial — API tests are cheap to fan out |

Multiply this across every form, filter, and validated input in a real application, and the regression suite that used to take 25 minutes can often drop to single digits — which is where a meaningful chunk of the 20–30% cycle-time reduction I've driven with Shift-Left strategies actually comes from. It's not one big trick; it's this decision, repeated across dozens of forms.

## The caveat — when you still need it at the UI

This isn't "delete all UI validation tests." Keep a UI-level test when:

- **Validation logic lives only in the frontend** (e.g., a JS-only character counter or input mask with no backend equivalent) — there's no API to shift it to.
- **The visual presentation of the error matters** — placement, styling, accessibility (screen reader announcement) — those are UI-only concerns by definition.
- **Cross-field UI behavior** that the API doesn't model, like a field that disables/greys out based on another field's state.

The rule isn't "no validation at the UI ever." It's: **don't pay browser overhead to re-verify a rule the API already enforces.**

## Takeaway

Shift-Left isn't a maturity-model buzzword — it's a series of small, defensible decisions about which layer owns which assertion. "One UI scenario, everything else at the API" is one of the highest-leverage versions of that decision, because form validation is exactly the kind of test case teams instinctively write at the UI layer by default, when the API was the right place all along.

