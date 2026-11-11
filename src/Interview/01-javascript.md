# 01 · JavaScript & Browser

Matrix: **JavaScript** (Core, Intermediate) · **JS in Browser** · **Browser APIs** · **JS Development Tools** (Required, Intermediate).
For A2 the level matches A1, but a Middle is expected to answer *why*, debug quickly and know the edge cases.
Old notes: [Core js 1](../../Evernote/Core%20js%201/Core%20js%201.md), [Core js 2](../../Evernote/Core%20js%202/Core%20js%202.md).

**Pass 1 priority.** P1: closures, `this`, prototypes, event loop, promises/async, events & delegation, fetch, storage. P2: modules, errors, DOM lifecycle, `defer`/`async`, DevTools, npm/ESLint. P3: generators, Canvas/media, Workers, observers.

## Checklist

### Language core
📚 Old notes: [[Core js 1#Coercion|Coercion]] · [[Core js 1#let, var and const|var / let / const]] · [[Core js 1#Temporal Dead Zone|TDZ]] · [[Core js 1#Hoisting|Hoisting]] · [[Core js 1#Execution context and lexical environment|Closures & lexical env]] · [[Core js 1#this in functions|this]] · [[Core js 1#Losing this (Reference Type)|Losing this]] · [[Core js 1#call, apply, bind|call, apply, bind]] · [[Core js 2#proto vs prototype|Prototypes]] · [[Core js 2#Classes vs constructor functions|Classes]] · [[Core js 2#super|super]] · [[Core js 1#Property flags and descriptors|Descriptors]] · [[Core js 1#Arrays Built-in Methods|Arrays]] · [[Core js 2#Map|Map]] · [[Core js 2#Set|Set]] · [[Core js 2#WeakMap and WeakSet|WeakMap / WeakSet]] · [[Core js 1#Iterables and Symbol.iterator|Iterators]] · [[Core js 2#Module types AMD CommonJS ES6 UMD|Modules]] · [[Core js 2#try catch finally|try / catch]] · [[Core js 2#Custom errors|Custom errors]] · [[Core js 2#Currying|Currying]] · [[Core js 2#Partial functions|Partial]] · [[Core js 2#Garbage collection|Garbage collection]]
- [x] 🔴 CORE Types, coercion, `==` vs `===`, `Object.is`, `typeof null`, falsy values, `??` vs `||`, `?.`
- coercion either number or string
	- `+` - If there is not string - **number**, if yes - **string**
	- `other math operators` - **number**
	- `comparison operators` - to `primitives` -> both `strings` -> compare by symbols / both `numbers` -> `numbers`
	- `==` - both strings -> compare by symbols / converts to numbers / `null` & `undefined` to each other and nothing else except themself
	- `===` - everything by type and value, but not `NaN` and `-0` and `0`
	- objects - either **valueOf** or **toString**. Has tree **hints** - `number`, `string` and `default`. Default one is in case when `+` and `==` involved, then tries `valueOf`
- [x] 🔴 CORE Scope: lexical, block vs function, **TDZ**, hoisting of `var`/`let`/function declarations/classes
- `variable scope` - just a concept describing `lexical env`
- `block scope` - temporal lex env in the same context with outer as context orig env that is poped when executions comes to the end of the block
- `var` - **declared** - **initialized** with `undefined`, **assigned** later. As it is written to var env and redeclaring doesn't cause error, for loop writes the value to context env
- `class` behaves as `let`/`const`
- [x] 🔴 CORE Closures: private state, factory functions, the classic `for (var i…)` + `setTimeout` bug
- [x] 🔴 CORE `this`: default / implicit / explicit (`call`/`apply`/`bind`) / `new`, arrows have lexical `this`, losing `this` in callbacks
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
- [x] 🔴 CORE Prototypes: `__proto__` vs `prototype`, `Object.create`, prototype chain lookup, `class` is sugar; `#private`, `static`, class fields
- `Object.create` second object with default props are not **writable**, **configurable** and **enumerable**. We have to pass `Record<string,PropertyDescriptor>`
- `super` called in constructor, `super.method` optionally in method
- [x] 🟡 SHALLOW Objects: property descriptors, `freeze`/`seal`, getters/setters, shallow vs deep copy (`structuredClone`)
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
- [x] 🟡 SHALLOW Arrays: mutating vs non-mutating methods (`toSorted`, `toSpliced`, `with`), `reduce`, `flatMap`, `at`, `Object.groupBy` (+)
- groupBy takes iterable, callback gives the object and returns a key where the object will be pushed in array
  ```Typescript
  Object.groupBy(arr, ({key} => key === smth ? "key1": "key2"))
  ```
- [x] 🟡 SHALLOW `Map`/`Set`/`WeakMap`/`WeakSet`: when to use which (+ WeakMap for private data / caching)
- Map
	- `Object.fromEntries(map.entries())`
- `WeakMap` and `WeekSet` only work with objects, attach data to dom, cache...
- [ ] 🟡 SHALLOW Iterators & generators, `Symbol.iterator`, `for…of` vs `for…in` (+)
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
- [ ] 🟡 SHALLOW Modules: ESM vs CommonJS, named vs default export, live bindings, dynamic `import()`, tree shaking
	- [ ] Esm vs CommonJs 

| Feature | CommonJS (CJS) | ESM (ECMAScript Modules) |
| ------------ | ------------------------------------------ | ----------------------------------- |
| Syntax | `const x = require('x')module.exports = x` | `import x from 'x'export default x` |
| Loading | Synchronous (blocks execution) | Asynchronous (non-blocking) |
| Timing | Evaluated at runtime | Evaluated before code runs (static) |
| Tree-shaking | Difficult / Not supported | Native (removes unused code) |
| Environment | Node.js only | Browsers & Node.js |
- [x] 🟡 SHALLOW Errors: `try/catch/finally`, custom `Error` subclasses, `error.cause` (+)
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
- [ ] 🔴 CORE Functional: pure functions, immutability, currying, composition, debounce/throttle, memoization

### Async
📚 Old notes: [[Core js 2#Promises, async await and the event loop|Event loop]] · [[Core js 2#Callback hell|Callback hell]] · [[Core js 1#Timers|Timers]] · [[Core js 2#Timers and requestAnimationFrame|requestAnimationFrame]]
- [x] 🔴 CORE **Event loop**: call stack → microtasks (promises, `queueMicrotask`) → render (rAF) → macrotask (timers, I/O, events). Be able to predict output order
	- [ ] train output order
	- [ ] events
- [x] 🔴 CORE Promise states, chaining, error propagation, `then` returns a new promise
- [x] 🔴 CORE `Promise.all` / `allSettled` / `race` / `any`: differences and failure behaviour
- [x] 🔴 CORE `async/await`: sequential vs parallel awaits, error handling, `for await…of`
- `for await…of` is not like Promise.all. It goes sequentially through the items and wait lement before starting next. `ForEach` won't work as it is synchronous and only other loops might be used here  
- [ ] 🟡 SHALLOW `AbortController` for fetch cancellation and timeouts (`AbortSignal.timeout`) (+)
- [x] 🟡 SHALLOW Timers: `setTimeout(fn, 0)` is not 0, `setInterval` drift, `requestAnimationFrame`, `requestIdleCallback`

### Browser & DOM (A2)
📚 Old notes: [[Core js 1#Global Object Window|window & DOM]] · [[Core js 1#Types of DOM events|Event types]] · [[Core js 1#Event listeners|Event listeners]] · [[Core js 1#Event phases|Event phases]] · [[Core js 1#Event delegation|Delegation]] · [[Core js 1#Preventing default behaviour|preventDefault]] · [[Core js 1#Custom events|Custom events]] · [[Core js 2#Critical rendering path|Critical rendering path]] · [[Core js 2#Dynamic imports|Dynamic imports]]
- [x] 🟡 SHALLOW DOM tree, node vs element, query & traversal, `DocumentFragment`, `innerHTML` vs `textContent` (XSS!)
- DocumentFragment lightweight container that allows to manipulated nodes and append them at once. After that disappears
- DOM tree is build from nodes (element, comments, text, doc node and fragment). Elements - nodes corresponding to html tags
- [x] 🔴 CORE Events: capturing → target → bubbling, `stopPropagation` vs `preventDefault`, **delegation**, `{ once, passive, capture }`, `CustomEvent`
- `dispatchEvent(new Event('yourEvent'))`
- [x] 🟡 SHALLOW Page lifecycle: `DOMContentLoaded`, `load`, `beforeunload`, `visibilitychange`; `<script defer|async|type=module>`
	- [ ] type=module
- `visibilitychange` - when the page visibility changes (like tab in the background)
- [x] ⚪ SKIP Forms: `submit` event, `FormData`, constraint validation API
- constraint validation API - connects native validation rules to JS allows to check the through methods and props. Allows to check everything and `setCustomValidity` before sending the data

### Browser APIs (A2)
📚 Old notes: [[Core js 2#Fetch|Fetch]] · [[Core js 2#XMLHttpRequest|XMLHttpRequest]] · [[Core js 1#Web Storage API and Cookies|Storage & cookies]] · [[Core js 2#History API|History API]] · [[Core js 2#Location|Location]] · [[Core js 2#Navigator|Navigator]] · [[Core js 1#Date and Time|Date and Time]]
- [x] 🔴 CORE `fetch`: `response.ok` (fetch does **not** reject on 4xx/5xx), headers, `credentials`, JSON/Blob, abort, retry
	- [ ] headers
	- [ ] Blob
	- [ ] retry
- [x] 🔴 CORE Storage: cookies (`HttpOnly`, `Secure`, `SameSite`, `Max-Age`) vs `localStorage` vs `sessionStorage` vs IndexedDB: size, lifetime, who can read it, when to use which
- [ ] ⚪ SKIP Graphics/media: Canvas 2D basics (draw, animate with rAF), `<video>`/`<audio>` API, SVG vs Canvas
- [ ] 🟡 SHALLOW Observers: `IntersectionObserver` (lazy load, infinite scroll), `ResizeObserver`, `MutationObserver` (+)
- [ ] 🟡 SHALLOW `URL` / `URLSearchParams`, History API (`pushState`, `popstate`): the basis of client routing
- [ ] ⚪ SKIP Web Workers, `BroadcastChannel`, `postMessage` (+); Clipboard, Geolocation, Notifications (awareness)

### Dev tools (A2)
- [x] 🟡 SHALLOW DevTools: Elements (computed styles, event listeners), Console (`$0`, `console.table`, `console.time`), **Sources** (breakpoints: line, conditional, DOM, XHR/fetch, logpoints, call stack, scope, watch), Network (throttling, initiator, HAR), Application (storage), Performance & Memory (heap snapshot, detached nodes)
	- [ ] console.table and time
	- [ ] sources
	- [ ] network initiator and har
	- [ ] detached nodes
- [x] ⚪ SKIP VS Code debugger for Vite/Vitest; `debugger` statement; source maps
- [ ] 🟡 SHALLOW npm: `package.json` fields, scripts, `dependencies` vs `devDependencies` vs `peerDependencies`, semver `^ ~`, lock files, `npm ci`, `npx`
- [x] 🟡 SHALLOW ESLint (flat config `eslint.config.js`, rules, plugins, `--fix`), Prettier, `.editorconfig`, husky + lint-staged
- [ ] 🟡 SHALLOW Bundlers: what Vite does in dev (native ESM + esbuild) vs build (Rollup), HMR (+)

## Key questions (answer aloud)

1. 🔴 CORE What's a closure? Show a real use case and a memory-leak risk.
2. 🔴 CORE Explain the event loop. Output of: `setTimeout(log1); Promise.resolve().then(log2); queueMicrotask(log3); log4`.
3. 🔴 CORE How does `this` get determined? Why does `obj.method` lose `this` when passed as a callback?
4. 🔴 CORE Prototype chain: what happens on `obj.foo` lookup? How do classes map to prototypes?
5. 🔴 CORE `Promise.all` vs `allSettled`: when would you pick each? How do you cancel a fetch?
6. 🔴 CORE Event delegation: what is it, why use it, and how do you handle `event.target` vs `currentTarget`?
7. 🟡 SHALLOW `defer` vs `async` scripts.
8. 🔴 CORE Where would you store an auth token and why? (cookies vs localStorage; ties into 06 security)
9. 🔴 CORE `var`/`let`/`const` and the TDZ.
10. 🟡 SHALLOW Deep copy options and their limitations (`JSON`, `structuredClone`, recursion).
11. 🟡 SHALLOW How do you find a memory leak in an SPA?
12. 🟡 SHALLOW ESM vs CommonJS: differences and why tree shaking needs ESM.

## Hands-on

- [ ] 🔴 CORE Predict-the-output: 10 event-loop snippets (search "event loop quiz") without running them, then check
- [ ] 🔴 CORE Implement `debounce`, `throttle`, `memoize`, `curry`, `Promise.all` (see [09](./09-live-coding.md))
- [ ] ⚪ SKIP Debug session: set a conditional breakpoint + XHR breakpoint on any real site, inspect the call stack
- [ ] ⚪ SKIP Lazy-load images with `IntersectionObserver`; cancel stale search requests with `AbortController`
- [ ] ⚪ SKIP Take a heap snapshot, find a detached DOM node you created on purpose

## Links

- [javascript.info](https://javascript.info/): Parts 1–2 cover everything above
- [MDN: Event loop / Using promises](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Event_loop)
- [Jake Archibald: In The Loop (talk)](https://www.youtube.com/watch?v=cCOL7MC4Pl0)
- [Chrome DevTools docs](https://developer.chrome.com/docs/devtools)
