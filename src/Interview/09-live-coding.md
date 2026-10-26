# 09 · Live coding practice

Rules: 20–30 min per task, **talk aloud while coding** (that's what the interviewer evaluates), start with clarifying questions + edge cases, then code, then test with examples.
Old solutions: [Core js 2 → Coding Task Examples](../../Evernote/Core%20js%202/Core%20js%202.md#coding-task-examples), so redo them from scratch, don't re-read.

## Set A: JavaScript (Phase 1 evenings + Mon Nov 2)

- [ ] `debounce(fn, ms)` and `throttle(fn, ms)` (keep `this` and args; bonus: `cancel()`, leading/trailing)
- [ ] `memoize(fn)` with a custom key resolver
- [ ] `curry(fn)` → `curry(sum)(1)(2)(3)` and `curry(sum)(1, 2)(3)`
- [ ] Polyfills: `Array.prototype.map/filter/reduce`, `Function.prototype.bind`, `Object.create`
- [ ] `Promise.all`, `Promise.allSettled`, `Promise.race`, `Promise.any`
- [ ] `promisify(fn)`; `sleep(ms)`; `retry(fn, attempts, delay)` with exponential backoff
- [ ] `promisePool(tasks, limit)`: run async tasks with a concurrency limit
- [ ] `deepClone(obj)` (handle arrays, Date, Map, circular refs) and `deepEqual(a, b)`
- [ ] `flatten(arr, depth)`, `groupBy(arr, fn)`, `chunk(arr, size)`, `uniqueBy`
- [ ] `EventEmitter` class: `on`, `off`, `once`, `emit`
- [ ] `get(obj, 'a.b[0].c', default)`
- [ ] Predict output: 5 tricky `this` / closure / event-loop snippets

## Set B: TypeScript + React (Sat Nov 7)

- [ ] Type utils: `DeepPartial<T>`, `PickByValue<T, V>`, `ElementType<T>`, typed `groupBy<T, K extends PropertyKey>`
- [ ] `useDebounce(value, ms)`, `useFetch(url)` with abort + loading/error state, `useLocalStorage<T>(key, initial)`, `usePrevious`
- [ ] Autocomplete: debounced input, fetch suggestions, keyboard navigation, cancel stale requests
- [ ] Paginated / infinite list with React Query (`useInfiniteQuery`) + `IntersectionObserver`
- [ ] Form with validation (controlled), show errors on blur, disable submit while submitting
- [ ] Modal via portal: focus trap, close on Esc / backdrop click
- [ ] Generic typed `<Table<T> columns={…} rows={…} />`
- [ ] Find & fix bugs in a given component (stale closure in `useEffect`, missing deps, mutating state, index keys)
- [ ] Write tests for one of the above with RTL + user-event (ties to [04](./04-testing.md))

## Set C: Algorithms (pick 1/day in Phase 2)

See the LeetCode list in [08](./08-engineering.md#hands-on). Always state the Big-O at the end.

## Code review drill (+)

Interviews sometimes give a PR to review. Practice: take any old component of mine and list issues by category: correctness, performance, a11y, readability, typing, tests.
