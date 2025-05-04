![Thumbnail](./assets/speed-and-reliability.png)

# Speed and Reliability — Auto-Waits, Conditional Actions, and More

Writing stable test automation for web apps is like trying to hit a moving target — elements load at different times, networks lag, and dynamic UIs love to throw curveballs. Traditional tools make you slap on manual waits and retries, turning your test suite into a slow, flaky mess. Playwright steps in with **auto-waits** and **conditional actions** to keep your tests fast *and* reliable.

## Auto-Waits: No More Sleepy Tests

Playwright automatically waits for elements to be ready — clickable, visible, whatever — before taking action. Forget hardcoding `sleep(5)` and hoping it works. It waits for a button to be attached to the DOM, visible, and stable before clicking. Your tests hit the sweet spot every time, no wasted seconds. Think of it as a barista who knows exactly when your coffee’s brewed to perfection.

## Conditional Actions: Tests That Think

What if you don’t know the page’s state — like whether a menu is expanded or collapsed? Playwright’s got you covered with runtime checks:

```typescript
const expandButton = page.getByRole("button", { name: "Expand" });
if (await expandButton.isVisible()) await expandButton.click();
```

Your tests adapt on the fly, dodging flakiness like a ninja.

Here’s how Playwright compares:

- **Selenium**: Relies on explicit waits or retry hacks to handle dynamic content. This increases the likelihood of introducing flakiness, as arbitrary wait times can be too short or too long depending on external factors like network speed or browser load. And in my humble opinion, implicit waits—which Selenium can't function without—are one of the gravest mistakes in test automation, leading to unpredictable test behavior.
- **Cypress** offers built-in waits similar to Playwright but ***religiously*** rejects the idea of conditional behavior. Its development team is so convinced that you should always know the exact state of your app that they [removed the ability](https://docs.cypress.io/app/faq#Can-I-use-ES7-async--await-syntax) to use async functions in the `cy` domain, which previously allowed automation engineers to workaround the lack of conditional statements. This rigidity limits flexibility in dynamic scenarios and creates a lot of obstacles for typical OOP-driven test development.  
For us, it was a dealbreaker: we had async functions in every method of every page object across multiple frameworks. When that change hit, we faced a brutal choice—refactor our entire test architecture or lock ourselves into the last version supporting async/await and never update. We chose the latter, and it was our final Cypress project. Playwright’s adaptability won us over.

## Reliability Boosters

Playwright piles on the goodness:

- **Automatic Retries**: [Retrying assertions](https://playwright.dev/docs/test-assertions#auto-retrying-assertions) and conditional actions wrapped in `expect.toPass()` or `expect.poll()` retry automatically, brushing off temporary hiccups. You can even retry entire tests via `playwright.config.ts`, and even tune it to retry only in CI.
- **Network Mocking**: Monitor or mock network requests to tame flaky APIs and control your test environment.
  
| Feature                 | Selenium | Cypress | Playwright |
| ----------------------- |:--------:|:-------:|:----------:|
| Auto-waits              |    ❌    |   ✅   |     ✅     |
| Automatic retries       |    ❌    |   ✅   |     ✅     |
| Network mocking         |    ❌    |   ✅   |     ✅     |
| Conditional actions     |    ✅    |   ❌   |     ✅     |
| OOP-friendly            |    ✅    |   ❌   |     ✅     |

Playwright’s auto-waits and conditional actions deliver tests that are fast, stable, and drama-free.  
Speed or reliability? You don’t have to pick anymore.

Want more? Stay tuned for our next piece on making your CI pipeline a breeze — and you’ll see how we ran a massive suite, like 1800 E2E UI tests, in just 4 minutes with Playwright.