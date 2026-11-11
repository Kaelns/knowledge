# 09 · Live coding practice

Rules: 20–30 min per task, **talk aloud while coding** (that's what the interviewer evaluates), start with clarifying questions + edge cases, then code, then test with examples.
Old solutions: [Core js 2 → Coding Task Examples](../../Evernote/Core%20js%202/Core%20js%202.md#coding-task-examples), so redo them from scratch, don't re-read.

## Set A: JavaScript (4 tasks Sun Nov 1, rest in Pass 2)
📚 Old notes: [[Core js 2#bind polyfill|bind]] · [[Core js 2#Object.create polyfill|Object.create]] · [[Core js 2#Array.flat polyfill|flat]] · [[Core js 2#Array.reduce polyfill|reduce]] · [[Core js 2#Currying|curry]] · [[Core js 2#Partial functions|partial]] · [[Core js 2#Sequential promise execution|sequential promises]] · [[Core js 2#Functional expressions|five(plus(…))]] · [[Core js 1#Custom sorting|sort comparator]] · [[Core js 1#Binding a function twice|bind twice]]

- [ ] 🔴 CORE `debounce(fn, ms)` and `throttle(fn, ms)` (keep `this` and args; bonus: `cancel()`, leading/trailing)
- [ ] 🔴 CORE `memoize(fn)` with a custom key resolver
- [ ] 🟡 SHALLOW `curry(fn)` → `curry(sum)(1)(2)(3)` and `curry(sum)(1, 2)(3)`
- [ ] 🔴 CORE Polyfills: `Array.prototype.map/filter/reduce`, `Function.prototype.bind`, `Object.create`
- [ ] 🔴 CORE `Promise.all`, `Promise.allSettled`, `Promise.race`, `Promise.any`
- [ ] 🟡 SHALLOW `promisify(fn)`; `sleep(ms)`; `retry(fn, attempts, delay)` with exponential backoff
- [ ] 🟡 SHALLOW `promisePool(tasks, limit)`: run async tasks with a concurrency limit
- [ ] 🟡 SHALLOW `deepClone(obj)` (handle arrays, Date, Map, circular refs) and `deepEqual(a, b)`
- [ ] 🔴 CORE `flatten(arr, depth)`, `groupBy(arr, fn)`, `chunk(arr, size)`, `uniqueBy`
- [ ] 🟡 SHALLOW `EventEmitter` class: `on`, `off`, `once`, `emit`
- [ ] ⚪ SKIP `get(obj, 'a.b[0].c', default)`
- [ ] 🔴 CORE Predict output: 5 tricky `this` / closure / event-loop snippets

## Set B: TypeScript + React (Pass 2, day off 2)

- [ ] ⚪ SKIP Type utils: `DeepPartial<T>`, `PickByValue<T, V>`, `ElementType<T>`, typed `groupBy<T, K extends PropertyKey>`
- [ ] 🔴 CORE `useDebounce(value, ms)`, `useFetch(url)` with abort + loading/error state, `useLocalStorage<T>(key, initial)`, `usePrevious`
- [ ] 🟡 SHALLOW Autocomplete: debounced input, fetch suggestions, keyboard navigation, cancel stale requests
- [ ] ⚪ SKIP Paginated / infinite list with React Query (`useInfiniteQuery`) + `IntersectionObserver`
- [ ] 🟡 SHALLOW Form with validation (controlled), show errors on blur, disable submit while submitting
- [ ] ⚪ SKIP Modal via portal: focus trap, close on Esc / backdrop click
- [ ] ⚪ SKIP Generic typed `<Table<T> columns={…} rows={…} />`
- [ ] 🔴 CORE Find & fix bugs in a given component (stale closure in `useEffect`, missing deps, mutating state, index keys)
- [ ] 🟡 SHALLOW Write tests for one of the above with vitest-browser-react: locators + `expect.element` (ties to [04](./04-testing.md))

## Set C: Algorithms (pick 1/day in Pass 2)

See the LeetCode list in [08](./08-engineering.md#hands-on). Always state the Big-O at the end.

## Code review drill (+)

Interviews sometimes give a PR to review. Practice: take any old component of mine and list issues by category: correctness, performance, a11y, readability, typing, tests.
