# Beyond the Green Checkmark

**Notes from a Senior SDET on building test automation that actually holds up in production.**

I'm Neha Raybagkar — a Senior SDET / QA Automation Architect with 14+ years building test frameworks for enterprise-scale applications. This is where I write about the decisions behind the frameworks, not just the syntax: why a framework was structured a certain way, what broke, and what I'd do differently next time.

A green checkmark tells you a test passed. It doesn't tell you whether the test was worth writing, whether it'll still be useful in six months, or whether it's quietly hiding a flaky retry. That's what this blog is actually about.

🔗 [Resume](#) · [LinkedIn](https://linkedin.com/in/neha-raybagkar) · [GitHub](https://github.com/neharaybag)

---

## 📚 Latest Articles

| Article | Topic | Link |
|---|---|---|
| Shift-Left in Practice: One UI Scenario, Twenty API Scenarios  | Framework Migration | [Read](https://neharaybag.github.io/shift-left-one-ui-many-api) |
| Why I Migrated 200 Flaky Selenium Tests to Playwright | Framework Migration | Coming soon |
| Building a Self-Healing Locator with Playwright and an LLM | AI-Assisted Testing | Coming soon |
| Cypress vs Playwright: What I Learned Running Both in Production | Framework Comparison | Coming soon |
| Catching Breaking API Changes Before They Ship | Contract Testing | Coming soon |
| TypeScript: var, let, and const | TypeScript Fundamentals | [Read](./typescript-explained.md) |
| TypeScript: Sync vs Async | TypeScript Fundamentals | [Read](./typescript_sync_async.md) |

---

## 🗂 Browse by Topic

### Framework Architecture
Design decisions behind real test frameworks — why a structure was chosen, what tradeoffs it made, what broke at scale.
- *Why I Migrated 200 Flaky Selenium Tests to Playwright* — coming soon
- *Cypress vs Playwright: What I Learned Running Both in Production* — coming soon

### AI-Assisted Testing
Where I'm experimenting with LLMs in the test pipeline — self-healing locators, AI-generated test cases, and where the hype doesn't hold up.
- *Building a Self-Healing Locator with Playwright and an LLM* — coming soon

### API & Contract Testing
Keeping backend changes from silently breaking consumers.
- *Catching Breaking API Changes Before They Ship: Contract Testing in CI* — coming soon

### CI/CD & Test Infrastructure
Making suites fast, parallel, and reliable enough that people trust the results.
- *Cutting Regression Time 30% with Parallel Dockerized Test Execution* — coming soon

### TypeScript Fundamentals
- [TypeScript: var, let, and const](./typescript-explained.md)
- [TypeScript: Sync vs Async](./typescript_sync_async.md)

---

## 🧰 Companion Repositories

Each post above is paired with a real, runnable repository — code first, write-up second.

| Repo | What it covers |
|---|---|
| [playwright-ts-framework](https://github.com/neharaybag/playwright-ts-framework) | Playwright + TypeScript, Page Object Model, custom fixtures, API testing, CI |
| cypress-ts-framework | Coming soon |
| ai-self-healing-locators | Coming soon |
| api-contract-testing | Coming soon |

---

## 🎯 What This Blog Is For

Mostly written for engineers solving the same problems I have: flaky suites, slow regressions, frameworks that worked fine at 50 tests and fell apart at 500. 

---

⭐ If something here is useful, a star on the repo helps it reach more people.
