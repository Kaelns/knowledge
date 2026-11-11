# 04 · Testing

Matrix: "Ability to write unit tests" (Frameworks, Intermediate) · "Ability to create tests on different levels of Test Pyramid" (SE Practices, **A2**) · Node.js Testing.
My stack (checked in the project): **Vitest 4 Browser Mode** (`@vitest/browser-playwright`: Playwright is only the engine that runs Chromium) + **`vitest-browser-react`** for components, API mocked with `vi.mock` (axios instance or hooks), **Cypress** for e2e. No React Testing Library, no jsdom, no MSW in tests (Orval generates MSW handlers, but tests don't use them).
Interviewers usually ask in **RTL terms** (`getBy`/`queryBy`/`findBy`, `userEvent`, `waitFor`), so know the RTL answer and how it maps to what we use (table below).
This is my weakest area: **reading tests ≠ writing tests**. Fri Oct 30 is mostly hands-on.

**Pass 1 priority.** P1 (all hands-on): pyramid, RTL query priority, `getBy`/`queryBy`/`findBy`, user-event, MSW, testing Redux/React Query components, mocks vs stubs. P2: fake timers, `renderHook`, Playwright basics, flaky tests. P3: Storybook `play`, performance-testing tools.

## Setup: lab repo (Thu Oct 29, 30 min)

```bash
npm create vite@latest interview-lab -- --template react-ts
cd interview-lab
npm i @reduxjs/toolkit react-redux @tanstack/react-query react-router
npm i -D vitest @vitest/browser-playwright vitest-browser-react playwright
npx playwright install chromium
```

Same setup as the project (`vitest.browser.config.ts`):

```ts
import { playwright } from '@vitest/browser-playwright';
import { defineConfig, mergeConfig } from 'vitest/config';
import viteConfig from './vite.config.ts';

export default mergeConfig(viteConfig, defineConfig({
  test: {
    browser: { enabled: true, headless: true, provider: playwright(), instances: [{ browser: 'chromium' }] },
  },
}));
```

No `environment: 'jsdom'` and no jest-dom setup file: the DOM is a real browser, and `expect.element` matchers are built in.

## Checklist

### Theory
📚 Old notes: [[Core js 2#Types of testing|Types of testing]] · [[Core js 2#Test pyramid|Test pyramid]] · [[Core js 2#TDD and BDD|TDD & BDD]]
- [x] 🔴 CORE Test pyramid (unit → integration → e2e) vs **testing trophy** (Kent C. Dodds: most value in integration); cost/speed/confidence trade-offs

**Testing trophy:**

- **Integration (The Bulk):** Render features with child components, simulate real user clicks/typing, mock only the network (RTL + MSW).
- **E2E (Peak):** Run a real browser for the 3–5 critical business flows that can’t fail (Playwright/Cypress).
- 
**The One Rule to Remember:**
> _"Test behavior as a user sees it, not internal implementation details."_

| Layer / Type | Scope                         | Primary Purpose                                     | Cost / Speed      | Modern JS Tooling                |
| :----------- | :---------------------------- | :-------------------------------------------------- | :---------------- | :------------------------------- |
| Static       | Entire codebase (100%)        | Catch syntax, types, and logic bugs at compile-time | Instant / Free    | TypeScript, ESLint, Biome        |
| Visual       | UI pixels / styles            | Catch CSS regressions, layout breaks, clipping      | Medium            | Chromatic, Storybook, Playwright |
| Contract     | API interface (FE ↔ BE)       | Prevent API drift between frontend and backend      | Fast / Cheap      | Pact, Zod / OpenAPI              |


- [x] 🟡 SHALLOW Unit vs integration vs e2e vs visual/screenshot vs contract vs smoke vs regression; static analysis (TS, ESLint) as the base layer
- smoke - shallow fast test if app works - you plug in a new piece of electronic equipment, **if you see smoke, you immediately unplug it**
- [x] 🔴 CORE AAA (Arrange-Act-Assert), one behaviour per test, test **behaviour not implementation**
- [x] 🔴 CORE Test doubles: dummy, stub, spy, mock, fake; when mocking hurts
- **When mocking hurts:**
    - You mock your own hooks or modules, so tests pass while the integration is broken (the `fullName` vs `name` drift).
    - You assert on internals (which function was called) instead of what the user sees, so every refactor breaks tests.
    - Hand-written mock data copies your assumption, not the real contract.
    - Mocks leak between tests because they aren't reset.
    - Rule: **mock at boundaries** (network via MSW, time, random, third-party widgets, callback props), and keep your own code real.
- [ ] 🟡 SHALLOW TDD (red → green → refactor), BDD (Given/When/Then); coverage metrics and why 100% is not the goal
- [ ] 🟡 SHALLOW Flaky tests: causes (timing, shared state, network, order) and fixes

### Vitest
- [x] 🔴 CORE `describe`/`it`/`test`, `beforeEach`/`afterEach`, `it.each`, `.only`/`.skip`/`.todo`
- [x] 🔴 CORE Matchers: `toBe` vs `toEqual` vs `toStrictEqual`, `toThrow`, `resolves`/`rejects`, `toHaveBeenCalledWith`, snapshot / inline snapshot
- snapshot - create a file with data `toMatchSnapshot` while inline puts data (like object) to backticks string `toMatchInlineSnapshot(`{...}`)`
- `toThrow` needs a wrapper: `expect(() => fn())`
- `await expect(p).rejects.toThrow()`. Without `await` the test can pass falsely.
- Floats use `toBeCloseTo`.
- `toEqual` is deep (without prototypes and shallow arrays) and ignores `undefined` props and types, `toStrictEqual` checks those too
- [x] 🔴 CORE `vi.fn()`, `vi.spyOn()`, `vi.mock('module')` (hoisted!), `vi.mocked()`, `mockResolvedValue`, `mockImplementation`, restore/reset/clear differences
- `vi.hoisted`, create the mock inline, or automock (no factory) and use `vi.mocked(...)` in tests.
- Cleanup: **clear** = wipe call history, **reset** = also wipe the implementation (version-dependent, so check your version), **restore** = put back the original of a `spyOn`. Set `restoreMocks`/`clearMocks` in the config. that
- [ ] 🟡 SHALLOW Fake timers: `vi.useFakeTimers()`, `vi.advanceTimersByTime()`, `vi.setSystemTime()` (debounce tests)
- [ ] 🟡 SHALLOW Config: environment (`node` / `jsdom` / `happy-dom` = fake DOM in Node) vs **Browser Mode** (real browser), coverage (`v8`), watch mode, UI mode; Vitest vs Jest differences (ESM-native, Vite config reuse, API mostly compatible)

### Component tests: Vitest Browser Mode + `vitest-browser-react` (what my project uses)
- [x] 🔴 CORE **What it is**: tests run in a **real browser** (Chromium, started by Playwright through `@vitest/browser-playwright`), not in jsdom. Real CSS, layout, focus and events → fewer "passes in jsdom, breaks in the browser" bugs. Cost: slower start, needs a browser installed (also in CI).
- [x] 🔴 CORE **`render` is async**: `const screen = await render(<C />, { wrapper })`. Also `await rerender(...)`, `await unmount()`, `await cleanup()` (the project calls `cleanup` in `afterEach`).
- [x] 🔴 CORE **Locators, not elements**: `screen.getByRole('button', { name: 'Save' })` returns a lazy `Locator`. It doesn't search the DOM yet and doesn't throw. Query names are almost the same as RTL: `getByRole`, `getByLabelText`, `getByPlaceholder` (not `…PlaceholderText`), `getByText`, `getByAltText`, `getByTitle`, `getByTestId`. Same priority rule: role first, test id last.
- [x] 🔴 CORE **Assertions retry**: `await expect.element(locator).toBeVisible()` / `.toBeInTheDocument()` / `.toHaveTextContent('…')` / `.not.toBeInTheDocument()`. It waits until the check passes or times out, so it replaces `findBy` and `waitFor`. Without `await`, the test can pass without checking anything.
- [x] 🔴 CORE **Interactions** are real browser events (sent by Playwright): `await locator.click()`, `.fill('text')`, `.clear()`, `.selectOptions('a')`, `.hover()`; or `userEvent` from `vitest/browser`. No `fireEvent`, no `userEvent.setup()`.
- [ ] 🟡 SHALLOW Non-retrying reads: `.element()` (exactly one match or throws), `.query()` (element or `null`), `.elements()` (array); `.nth()`, `.first()`, `.filter()`. For non-DOM checks (e.g. a mock was called): `await vi.waitFor(() => expect(fn).toHaveBeenCalled())` or `await expect.poll(() => value).toBe(…)`.
- [ ] 🟡 SHALLOW `renderHook` exists and is async too: `const { result, act } = await renderHook(() => useX(), { wrapper })`
- [ ] 🟡 SHALLOW Gotchas: `vi.mock` works (the project uses it in ~90 files), but `vi.spyOn` on an export of an imported module doesn't, because ESM exports are read-only in a real browser → use `vi.mock(path, { spy: true })` or a mock factory. `vi.hoisted(() => ({ requestMock: vi.fn() }))` for mock variables used inside `vi.mock` (project pattern). No Node APIs (`fs`, `path`) in test code.

**RTL → what we use** (for answering RTL questions):

| RTL (jsdom)                                                   | Vitest Browser Mode + `vitest-browser-react`                                       |
| ------------------------------------------------------------- | ---------------------------------------------------------------------------------- |
| `render(<C />)` (sync)                                        | `const screen = await render(<C />)`                                               |
| `screen.getByRole(...)` returns an element, throws if missing | `screen.getByRole(...)` returns a lazy locator                                     |
| `getBy` / `queryBy` / `findBy`                                | one locator + `await expect.element(loc)` (retries); `.query()` if you need `null` |
| `expect(el).toBeInTheDocument()` (jest-dom)                   | `await expect.element(loc).toBeInTheDocument()` (built in)                         |
| `await waitFor(() => …)`                                      | `expect.element` for DOM; `vi.waitFor` / `expect.poll` for anything else           |
| `fireEvent.click` / `await user.click(el)`                    | `await loc.click()` (real browser event)                                           |
| `act()` warnings                                              | not a thing in practice: `render` and interactions are async and wait for React    |

Say it like this: *"We use Vitest Browser Mode with vitest-browser-react. The API is very close to Testing Library and follows the same idea, query by role like a user, but tests run in real Chromium. Queries return locators, and `expect.element` retries, so we don't need `findBy` or `waitFor`."*


```Typescript
import { render, screen, waitFor } from '@testing-library/react'
import userEvent from '@testing-library/user-event'

test('saves the form', async () => {
  const user = userEvent.setup()
  render(<Form />, { wrapper: Providers })

  await user.type(screen.getByLabelText('Email'), 'a@b.com')
  await user.click(screen.getByRole('button', { name: 'Save' }))

  expect(await screen.findByText('Saved')).toBeInTheDocument()
  await waitFor(() =>
    expect(screen.queryByText('Saving...')).not.toBeInTheDocument()
  )
})

import { render } from 'vitest-browser-react'
import { expect, test } from 'vitest'

test('saves the form', async () => {
  const screen = await render(<Form />, { wrapper: Providers })

  await screen.getByLabelText('Email').fill('a@b.com')
  await screen.getByRole('button', { name: 'Save' }).click()

  await expect.element(screen.getByText('Saved')).toBeInTheDocument()
  await expect.element(screen.getByText('Saving...')).not.toBeInTheDocument()
})
```
### React Testing Library (interviewers still ask about it)
- [x] 🔴 CORE Philosophy: "The more your tests resemble the way your software is used…" (same idea in both libraries)
- [ ] 🟡 SHALLOW **Query priority**: `getByRole` (with `name`) → `getByLabelText` → `getByPlaceholderText` → `getByText` → … → `getByTestId` last
- [x] 🔴 CORE `getBy` (throws) vs `queryBy` (null: for asserting absence) vs `findBy` (async, waits). Know it: this is the most common testing question
- [ ] 🟡 SHALLOW `userEvent.setup()` + `await user.click/type/keyboard` vs `fireEvent`; why user-event is preferred (full event sequence like a real user)
- [ ] 🟡 SHALLOW Async: `findBy*`, `waitFor`, `waitForElementToBeRemoved`; the `act()` warning ("a state update happened that the test didn't wait for")
- [ ] 🟡 SHALLOW `screen`, `within`, `render` options (`wrapper`), custom `renderWithProviders` (Redux store + QueryClient + Router)

### Testing state & data
- [x] 🔴 CORE **Redux**: test reducers as pure functions; test slices with a real store; test connected components with a real store (`renderWithProviders`, preloaded state), don't mock `useSelector`
- [x] 🔴 CORE **React Query**: new `QueryClient` per test (or `client.clear()` in `afterEach`), `retry: false`, mock the network layer, not the hook. Project way: `vi.mock('@/api/config/axiosInstance.api')` with a `requestMock` that answers by `config.url`, so the real Orval hooks and React Query still run
- [ ] 🟡 SHALLOW **Ways to mock the API**: `vi.mock` the HTTP client (our way: simple, no extra tool) vs **MSW** (intercepts real requests: `http.get(...)` + `HttpResponse.json()`, `server.use()` per-test overrides; in a browser it runs as a Service Worker via `setupWorker` from `msw/browser`) vs mocking the hook (fastest, but skips the query logic, so it tests less)
- [ ] 🟡 SHALLOW Router: `MemoryRouter` with `initialEntries` (used in ~35 project tests), render a small `useLocation()` component to assert the URL

### E2E & beyond
- [ ] 🟡 SHALLOW **Playwright**: `test`, `page`, locators (`getByRole`), auto-waiting, web-first assertions `await expect(locator).toBeVisible()`, fixtures, `page.route()` for network mocking, trace viewer, codegen, parallelism/projects (browsers)
- [ ] 🟡 SHALLOW **Cypress** (our e2e): `cy.visit`, `cy.get` / `cy.contains`, chained commands with built-in retry, `cy.intercept()` for network mocking, runs inside the browser; project scripts `test:e2e`, `test:e2e:smoke`
- [ ] 🟡 SHALLOW Playwright vs Cypress: architecture (Playwright controls the browser from outside vs Cypress runs inside it), multi-tab/browser support, speed, API style (`await` vs command chains)
- [ ] ⚪ SKIP Storybook: stories as test cases, `play` functions (interaction tests), Vitest addon, visual regression (Chromatic) (+)
- [x] 🔴 CORE What to test where: logic → unit, component + data → integration (Browser Mode + mocked API), critical user journeys → e2e
- [ ] 🟡 SHALLOW Performance testing types (A2): load, stress, spike, soak/endurance; tools: k6, JMeter, Lighthouse CI for frontend

## Key questions

1. 🔴 CORE Explain the test pyramid. Where does your project put most effort and why?
2. 🔴 CORE `getBy` vs `queryBy` vs `findBy`: when do you use each? How does your project do the same? (locator + `expect.element`)
3. 🔴 CORE Why `userEvent` over `fireEvent`? (and why Browser Mode events are even closer to a real user)
4. 🔴 CORE How do you test a component that fetches data? (mock the HTTP client like we do vs MSW vs mocking the hook)
5. 🟡 SHALLOW How do you test a debounced input? (fake timers + `vi.advanceTimersByTime`, or just let `expect.element` wait if the delay is short)
6. 🔴 CORE Mock vs stub vs spy. What's the danger of over-mocking?
7. 🔴 CORE How do you test a component connected to Redux / React Query?
8. 🟡 SHALLOW What causes flaky tests and how do you fix them?
9. 🟡 SHALLOW Playwright vs Cypress: what does your team use and why? (Cypress for e2e; Playwright only as the Vitest browser engine)
10. 🟡 SHALLOW What's the `act()` warning? Why don't you see it with Vitest Browser Mode?
11. 🔴 CORE jsdom vs a real browser for component tests: trade-offs?
12. 🟡 SHALLOW What would you **not** test?

## Hands-on kata (kata 1–6 Fri Oct 30 ~3.5h, kata 7 ~1h, 8–9 in Pass 2)

1. [ ] 🟡 SHALLOW Pure function TDD: `formatPrice`, `slugify`, `groupBy` with `it.each` (30m)
2. [ ] ⚪ SKIP `useDebounce` / `useCounter` hook with `renderHook` + fake timers (30m)
3. [ ] 🔴 CORE `LoginForm` with `vitest-browser-react`: `await render`, `.fill()` + `.click()`, `await expect.element(...)` for validation errors, submit calls `onSubmit` with values; queries by role/label only (45m)
4. [ ] 🔴 CORE `UserList` with React Query: mock the axios instance with `vi.hoisted` + `vi.mock` (project pattern), test loading, success, empty and error (`requestMock.mockRejectedValueOnce`) (1h)
5. [ ] ⚪ SKIP Redux slice tests + component with `renderWithProviders` and preloaded state (45m)
6. [ ] ⚪ SKIP Route test with `createMemoryRouter`: protected route redirects to `/login` (30m)
7. [ ] ⚪ SKIP Cypress: login → list → open details; one test with `cy.intercept` mocking (1h)
8. [ ] ⚪ SKIP Storybook `play` function for `LoginForm` (optional, 30m)
9. [ ] 🟡 SHALLOW Write in this file: 5 rules I'll follow when writing tests (my own words)

## Links

- [Testing Library: queries & priority](https://testing-library.com/docs/queries/about#priority) · [user-event](https://testing-library.com/docs/user-event/intro)
- [Kent C. Dodds: Common mistakes with RTL](https://kentcdodds.com/blog/common-mistakes-with-react-testing-library)
- [Vitest docs](https://vitest.dev/guide/) · [Vitest mocking](https://vitest.dev/guide/mocking)
- [Vitest Browser Mode](https://vitest.dev/guide/browser/) · [Component testing](https://vitest.dev/guide/browser/component-testing) · [vitest-browser-react API](https://vitest.dev/api/browser/react) · [Locators](https://vitest.dev/api/browser/locators) · [Assertions (`expect.element`)](https://vitest.dev/api/browser/assertions)
- [MSW docs](https://mswjs.io/docs/) · [Redux: Writing tests](https://redux.js.org/usage/writing-tests) · [TkDodo: Testing React Query](https://tkdodo.eu/blog/testing-react-query)
- [Playwright best practices](https://playwright.dev/docs/best-practices)
