# 04 · Testing

Matrix: "Ability to write unit tests" (Frameworks, Intermediate) · "Ability to create tests on different levels of Test Pyramid" (SE Practices, **A2**) · Node.js Testing.
My stack: **Vitest**, React Testing Library, MSW (+ Orval mocks), Playwright, a bit of Cypress, Storybook.
This is my weakest area: **reading tests ≠ writing tests**. Friday is fully hands-on.

## Setup: lab repo (do it Monday, 30 min)

```bash
npm create vite@latest interview-lab -- --template react-ts
cd interview-lab
npm i @reduxjs/toolkit react-redux @tanstack/react-query react-router
npm i -D vitest jsdom @testing-library/react @testing-library/user-event @testing-library/jest-dom msw @playwright/test
npx playwright install chromium
```

`vite.config.ts` → `test: { environment: 'jsdom', globals: true, setupFiles: './src/test/setup.ts' }`
`setup.ts` → `import '@testing-library/jest-dom/vitest'` + MSW `server.listen()` / `resetHandlers()` / `close()` hooks.

## Checklist

### Theory
- [ ] Test pyramid (unit → integration → e2e) vs **testing trophy** (Kent C. Dodds: most value in integration); cost/speed/confidence trade-offs
- [ ] Unit vs integration vs e2e vs visual/screenshot vs contract vs smoke vs regression; static analysis (TS, ESLint) as the base layer
- [ ] AAA (Arrange-Act-Assert), one behaviour per test, test **behaviour not implementation**
- [ ] Test doubles: dummy, stub, spy, mock, fake; when mocking hurts
- [ ] TDD (red → green → refactor), BDD (Given/When/Then); coverage metrics and why 100% is not the goal
- [ ] Flaky tests: causes (timing, shared state, network, order) and fixes

### Vitest
- [ ] `describe`/`it`/`test`, `beforeEach`/`afterEach`, `it.each`, `.only`/`.skip`/`.todo`
- [ ] Matchers: `toBe` vs `toEqual` vs `toStrictEqual`, `toThrow`, `resolves`/`rejects`, `toHaveBeenCalledWith`, snapshot / inline snapshot
- [ ] `vi.fn()`, `vi.spyOn()`, `vi.mock('module')` (hoisted!), `vi.mocked()`, `mockResolvedValue`, `mockImplementation`, restore/reset/clear differences
- [ ] Fake timers: `vi.useFakeTimers()`, `vi.advanceTimersByTime()`, `vi.setSystemTime()` (debounce tests)
- [ ] Config: environment (`jsdom`/`happy-dom`/`node`), coverage (`v8`), watch mode, UI mode; Vitest vs Jest differences (ESM-native, Vite config reuse, API mostly compatible)

### React Testing Library
- [ ] Philosophy: "The more your tests resemble the way your software is used…"
- [ ] **Query priority**: `getByRole` (with `name`) → `getByLabelText` → `getByPlaceholderText` → `getByText` → … → `getByTestId` last
- [ ] `getBy` (throws) vs `queryBy` (null: for asserting absence) vs `findBy` (async, waits)
- [ ] `userEvent.setup()` + `await user.click/type/keyboard` vs `fireEvent`; why user-event is preferred
- [ ] Async: `findBy*`, `waitFor`, `waitForElementToBeRemoved`; the `act()` warning and what it means
- [ ] `screen`, `within`, `render` options (`wrapper`), custom `renderWithProviders` (Redux store + QueryClient + Router)
- [ ] `renderHook` for custom hooks; testing a hook that uses React Query/Redux via wrapper
- [ ] jest-dom matchers: `toBeInTheDocument`, `toBeVisible`, `toHaveValue`, `toBeDisabled`, `toHaveAccessibleName`

### Testing state & data
- [ ] **Redux**: test reducers as pure functions; test slices with a real store; test connected components with a real store (`renderWithProviders`, preloaded state), don't mock `useSelector`
- [ ] **React Query**: new `QueryClient` per test, `retry: false`, `gcTime: Infinity` in tests, mock the network not the hook
- [ ] **MSW**: `http.get(...)` handlers + `HttpResponse.json()`, `server.use()` per-test overrides (error/empty/slow states), `onUnhandledRequest: 'error'`; Orval-generated MSW handlers
- [ ] Router: `MemoryRouter` / `createMemoryRouter` with `initialEntries`

### E2E & beyond
- [ ] **Playwright**: `test`, `page`, locators (`getByRole`), auto-waiting, web-first assertions `await expect(locator).toBeVisible()`, fixtures, `page.route()` for network mocking, trace viewer, codegen, parallelism/projects (browsers)
- [ ] Playwright vs Cypress: architecture (out-of-process CDP vs in-browser), multi-tab/browser support, speed, API style
- [ ] Storybook: stories as test cases, `play` functions (interaction tests), Vitest addon, visual regression (Chromatic) (+)
- [ ] What to test where: logic → unit, component + data → integration (RTL + MSW), critical user journeys → e2e
- [ ] Performance testing types (A2): load, stress, spike, soak/endurance; tools: k6, JMeter, Lighthouse CI for frontend

## Key questions

1. Explain the test pyramid. Where does your project put most effort and why?
2. `getBy` vs `queryBy` vs `findBy`: when do you use each?
3. Why `userEvent` over `fireEvent`?
4. How do you test a component that fetches data? (MSW vs mocking fetch vs mocking the hook)
5. How do you test a debounced input? (fake timers + `userEvent.setup({ advanceTimers })`)
6. Mock vs stub vs spy. What's the danger of over-mocking?
7. How do you test a component connected to Redux / React Query?
8. What causes flaky tests and how do you fix them?
9. Playwright vs Cypress: why did your team pick Playwright?
10. What's the `act()` warning?
11. What would you **not** test?

## Hands-on kata (Friday, in this order, ~6h)

1. [ ] Pure function TDD: `formatPrice`, `slugify`, `groupBy` with `it.each` (30m)
2. [ ] `useDebounce` / `useCounter` hook with `renderHook` + fake timers (30m)
3. [ ] `LoginForm`: labels, validation errors, submit calls `onSubmit` with values; queries by role/label only (45m)
4. [ ] `UserList` fetching `/api/users` with React Query + MSW: loading, success, empty, error (`server.use` override) (1h)
5. [ ] Redux slice tests + component with `renderWithProviders` and preloaded state (45m)
6. [ ] Route test with `createMemoryRouter`: protected route redirects to `/login` (30m)
7. [ ] Playwright: login → list → open details; one test with `page.route` mocking; open the trace viewer (1h)
8. [ ] Storybook `play` function for `LoginForm` (optional, 30m)
9. [ ] Write in this file: 5 rules I'll follow when writing tests (my own words)

## Links

- [Testing Library: queries & priority](https://testing-library.com/docs/queries/about#priority) · [user-event](https://testing-library.com/docs/user-event/intro)
- [Kent C. Dodds: Common mistakes with RTL](https://kentcdodds.com/blog/common-mistakes-with-react-testing-library)
- [Vitest docs](https://vitest.dev/guide/) · [Vitest mocking](https://vitest.dev/guide/mocking)
- [MSW docs](https://mswjs.io/docs/) · [Redux: Writing tests](https://redux.js.org/usage/writing-tests) · [TkDodo: Testing React Query](https://tkdodo.eu/blog/testing-react-query)
- [Playwright best practices](https://playwright.dev/docs/best-practices)
