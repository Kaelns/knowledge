# 01 · JavaScript & Browser

Matrix: **JavaScript** (Core, Intermediate) · **JS in Browser** · **Browser APIs** · **JS Development Tools** (Required, Intermediate).
For A2 the level matches A1, but a Middle is expected to answer *why*, debug quickly and know the edge cases.
Old notes: [Core js 1](../../Evernote/Core%20js%201/Core%20js%201.md), [Core js 2](../../Evernote/Core%20js%202/Core%20js%202.md).

**Pass 1 priority.** P1: closures, `this`, prototypes, event loop, promises/async, events & delegation, fetch, storage. P2: modules, errors, DOM lifecycle, `defer`/`async`, DevTools, npm/ESLint. P3: generators, Canvas/media, Workers, observers.

## Checklist

### Language core
- [x] Types, coercion, `==` vs `===`, `Object.is`, `typeof null`, falsy values, `??` vs `||`, `?.`
- coercion either number or string
	- `+` - If there is not string - **number**, if yes - **string**
	- `other math operators` - **number**
	- `comparison operators` - to `primitives` -> both `strings` -> compare by symbols / both `numbers` -> `numbers`
	- `==` - both strings -> compare by symbols / converts to numbers / `null` & `undefined` to each other and nothing else except themself
	- `===` - everything by type and value, but not `NaN` and `-0` and `0`
	- objects - either **valueOf** or **toString**. Has tree **hints** - `number`, `string` and `default`. Default one is in case when `+` and `==` involved, then tries `valueOf`
- [x] Scope: lexical, block vs function, **TDZ**, hoisting of `var`/`let`/function declarations/classes
- `variable scope` - just a concept describing `lexical env`
- `block scope` - temporal lex env in the same context with outer as context orig env that is poped when executions comes to the end of the block
- `var` - **declared** - **initialized** with `undefined`, **assigned** later. As it is written to var env and redeclaring doesn't cause error, for loop writes the value to context env
- `class` behaves as `let`/`const`
- [x] Closures: private state, factory functions, the classic `for (var i…)` + `setTimeout` bug
- [x] `this`: default / implicit / explicit (`call`/`apply`/`bind`) / `new`, arrows have lexical `this`, losing `this` in callbacks
```Typescript
Child.prototype = Object.create(Parent.prototype);

// FIXING THE CONSTRUCTOR: 
// Object.create wipes out the constructor
Child.prototype.constructor = Child;

Child.prototype.drive = function() {
  // SIMULATING: super.drive()
  const parentDriveMessage = Parent.prototype.drive.call(this); 

  return `${parentDriveMessage} At speed: ${this.speed}mph!`;
};

// ==============================================
// SIMULATING: Static Method Inheritance
// In modern JS, Parent's static methods are also inherited by Child
Object.setPrototypeOf(Child, Parent);
```
- [x] Prototypes: `__proto__` vs `prototype`, `Object.create`, prototype chain lookup, `class` is sugar; `#private`, `static`, class fields
- `Object.create` second object with default props are not **writable**, **configurable** and **enumerable**. We have to pass `Record<string,PropertyDescriptor>`
- `super` called in constructor, `super.method` optionally in method
- [x] Objects: property descriptors, `freeze`/`seal`, getters/setters, shallow vs deep copy (`structuredClone`)
- Descriptors
	- Object.getOwnPropertyDescriptor(s)
	- Object.defineProperty(ies)
- `getter` and `setter` - **accessor descriptors**
- `structuredClone`
	- copies `Set`, `Map`, `Date`, nested obj
	- circular references
	- throws errors for **functions** and **dom nodes**
	- drops **prototypes**
	- drops **symbols**
	- drops **descriptors** (getters, setters). Values become standard, getters just evaluated
- [x] Arrays: mutating vs non-mutating methods (`toSorted`, `toSpliced`, `with`), `reduce`, `flatMap`, `at`, `Object.groupBy` (+)
- groupBy takes iterable, callback gives the object and returns a key where the object will be pushed in array
  ```Typescript
  Object.groupBy(arr, ({key} => key === smth ? "key1": "key2"))
  ```
- [x] `Map`/`Set`/`WeakMap`/`WeakSet`: when to use which (+ WeakMap for private data / caching)
- Map
	- `Object.fromEntries(map.entries())`
- `WeakMap` and `WeekSet` only work with objects, attach data to dom, cache...
- [ ] Iterators & generators, `Symbol.iterator`, `for…of` vs `for…in` (+)
- for in - by keys even in prototypes, for of by iterators
- Iterator
```Typescript
let range = {
	from: 1,
	to: 5
};

range[Symbol.iterator] = function() {
	return {
		current: this.from,
		last: this.to,
		next() {
			if (this.current <= this.last) {
				return { done: false, value: this.current++ };
			} else {
				return { done: true };
			}
		}
	};
};

// or 
const range = {
	from: 1,
	to: 5,

	*[Symbol.iterator]() {
		// a shorthand for [Symbol.iterator]: function*()
		for (let value = this.from; value <= this.to; value++) {
			yield value;
		}
	},
};
```
- [ ] Modules: ESM vs CommonJS, named vs default export, live bindings, dynamic `import()`, tree shaking
	- [ ] Esm vs CommonJs 

| Feature | CommonJS (CJS) | ESM (ECMAScript Modules) |
| ------------ | ------------------------------------------ | ----------------------------------- |
| Syntax | `const x = require('x')module.exports = x` | `import x from 'x'export default x` |
| Loading | Synchronous (blocks execution) | Asynchronous (non-blocking) |
| Timing | Evaluated at runtime | Evaluated before code runs (static) |
| Tree-shaking | Difficult / Not supported | Native (removes unused code) |
| Environment | Node.js only | Browsers & Node.js |
- [x] Errors: `try/catch/finally`, custom `Error` subclasses, `error.cause` (+)
- `error.cause`
``` Typescript
throw new Error('Something went wrong', { cause: err })

// Error: Something went wrong
//     at ...
// Caused by: SyntaxError: Unexpected token b in JSON at position 2 ...

console.error(err.message); // "Failed to fetch user data"
console.error(err.cause);   // [SyntaxError: Unexpected token b in JSON]
console.error(err.cause instanceof SyntaxError); // true
```
- [ ] Functional: pure functions, immutability, currying, composition, debounce/throttle, memoization

### Async
- [x] **Event loop**: call stack → microtasks (promises, `queueMicrotask`) → render (rAF) → macrotask (timers, I/O, events). Be able to predict output order
	- [ ] train output order
	- [ ] events
- [x] Promise states, chaining, error propagation, `then` returns a new promise
- [x] `Promise.all` / `allSettled` / `race` / `any`: differences and failure behaviour
- [x] `async/await`: sequential vs parallel awaits, error handling, `for await…of`
- `for await…of` is not like Promise.all. It goes sequentially through the items and wait lement before starting next. `ForEach` won't work as it is synchronous and only other loops might be used here  
- [ ] `AbortController` for fetch cancellation and timeouts (`AbortSignal.timeout`) (+)
- [x] Timers: `setTimeout(fn, 0)` is not 0, `setInterval` drift, `requestAnimationFrame`, `requestIdleCallback`

### Browser & DOM (A2)
- [x] DOM tree, node vs element, query & traversal, `DocumentFragment`, `innerHTML` vs `textContent` (XSS!)
- DocumentFragment lightweight container that allows to manipulated nodes and append them at once. After that disappears
- DOM tree is build from nodes (element, comments, text, doc node and fragment). Elements - nodes corresponding to html tags
- [x] Events: capturing → target → bubbling, `stopPropagation` vs `preventDefault`, **delegation**, `{ once, passive, capture }`, `CustomEvent`
- `dispatchEvent(new Event('yourEvent'))`
- [x] Page lifecycle: `DOMContentLoaded`, `load`, `beforeunload`, `visibilitychange`; `<script defer|async|type=module>`
	- [ ] type=module
- `visibilitychange` - when the page visibility changes (like tab in the background)
- [x] Forms: `submit` event, `FormData`, constraint validation API
- constraint validation API - connects native validation rules to JS allows to check the through methods and props. Allows to check everything and `setCustomValidity` before sending the data

### Browser APIs (A2)
- [x] `fetch`: `response.ok` (fetch does **not** reject on 4xx/5xx), headers, `credentials`, JSON/Blob, abort, retry
	- [ ] headers
	- [ ] Blob
	- [ ] retry
- [x] Storage: cookies (`HttpOnly`, `Secure`, `SameSite`, `Max-Age`) vs `localStorage` vs `sessionStorage` vs IndexedDB: size, lifetime, who can read it, when to use which
- [ ] Graphics/media: Canvas 2D basics (draw, animate with rAF), `<video>`/`<audio>` API, SVG vs Canvas
- [ ] Observers: `IntersectionObserver` (lazy load, infinite scroll), `ResizeObserver`, `MutationObserver` (+)
- [ ] `URL` / `URLSearchParams`, History API (`pushState`, `popstate`): the basis of client routing
- [ ] Web Workers, `BroadcastChannel`, `postMessage` (+); Clipboard, Geolocation, Notifications (awareness)

### Dev tools (A2)
- [x] DevTools: Elements (computed styles, event listeners), Console (`$0`, `console.table`, `console.time`), **Sources** (breakpoints: line, conditional, DOM, XHR/fetch, logpoints, call stack, scope, watch), Network (throttling, initiator, HAR), Application (storage), Performance & Memory (heap snapshot, detached nodes)
	- [ ] console.table and time
	- [ ] sources
	- [ ] network initiator and har
	- [ ] detached nodes
- [x] VS Code debugger for Vite/Vitest; `debugger` statement; source maps
- [ ] npm: `package.json` fields, scripts, `dependencies` vs `devDependencies` vs `peerDependencies`, semver `^ ~`, lock files, `npm ci`, `npx`
- [x] ESLint (flat config `eslint.config.js`, rules, plugins, `--fix`), Prettier, `.editorconfig`, husky + lint-staged
- [ ] Bundlers: what Vite does in dev (native ESM + esbuild) vs build (Rollup), HMR (+)

## Key questions (answer aloud)

1. What's a closure? Show a real use case and a memory-leak risk.
2. Explain the event loop. Output of: `setTimeout(log1); Promise.resolve().then(log2); queueMicrotask(log3); log4`.
3. How does `this` get determined? Why does `obj.method` lose `this` when passed as a callback?
4. Prototype chain: what happens on `obj.foo` lookup? How do classes map to prototypes?
5. `Promise.all` vs `allSettled`: when would you pick each? How do you cancel a fetch?
6. Event delegation: what is it, why use it, and how do you handle `event.target` vs `currentTarget`?
7. `defer` vs `async` scripts.
8. Where would you store an auth token and why? (cookies vs localStorage; ties into 06 security)
9. `var`/`let`/`const` and the TDZ.
10. Deep copy options and their limitations (`JSON`, `structuredClone`, recursion).
11. How do you find a memory leak in an SPA?
12. ESM vs CommonJS: differences and why tree shaking needs ESM.

## Hands-on

- [ ] Predict-the-output: 10 event-loop snippets (search "event loop quiz") without running them, then check
- [ ] Implement `debounce`, `throttle`, `memoize`, `curry`, `Promise.all` (see [09](./09-live-coding.md))
- [ ] Debug session: set a conditional breakpoint + XHR breakpoint on any real site, inspect the call stack
- [ ] Lazy-load images with `IntersectionObserver`; cancel stale search requests with `AbortController`
- [ ] Take a heap snapshot, find a detached DOM node you created on purpose

## Links

- [javascript.info](https://javascript.info/): Parts 1–2 cover everything above
- [MDN: Event loop / Using promises](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Event_loop)
- [Jake Archibald: In The Loop (talk)](https://www.youtube.com/watch?v=cCOL7MC4Pl0)
- [Chrome DevTools docs](https://developer.chrome.com/docs/devtools)
