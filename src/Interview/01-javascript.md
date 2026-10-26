# 01 · JavaScript & Browser

Matrix: **JavaScript** (Core, Intermediate) · **JS in Browser** · **Browser APIs** · **JS Development Tools** (Required, Intermediate).
For A2 the level matches A1, but a Middle is expected to answer *why*, debug quickly and know the edge cases.
Old notes: [Core js 1](../../Evernote/Core%20js%201/Core%20js%201.md), [Core js 2](../../Evernote/Core%20js%202/Core%20js%202.md).

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
- [ ] Objects: property descriptors, `freeze`/`seal`, getters/setters, shallow vs deep copy (`structuredClone`)
- [ ] Arrays: mutating vs non-mutating methods (`toSorted`, `toSpliced`, `with`), `reduce`, `flatMap`, `at`, `Object.groupBy` (+)
- [ ] `Map`/`Set`/`WeakMap`/`WeakSet`: when to use which (+ WeakMap for private data / caching)
- [ ] Iterators & generators, `Symbol.iterator`, `for…of` vs `for…in` (+)
- [ ] Modules: ESM vs CommonJS, named vs default export, live bindings, dynamic `import()`, tree shaking
- [ ] Errors: `try/catch/finally`, custom `Error` subclasses, `error.cause` (+)
- [ ] Functional: pure functions, immutability, currying, composition, debounce/throttle, memoization

### Async
- [ ] **Event loop**: call stack → microtasks (promises, `queueMicrotask`) → render (rAF) → macrotask (timers, I/O, events). Be able to predict output order
- [ ] Promise states, chaining, error propagation, `then` returns a new promise
- [ ] `Promise.all` / `allSettled` / `race` / `any`: differences and failure behaviour
- [ ] `async/await`: sequential vs parallel awaits, error handling, `for await…of`
- [ ] `AbortController` for fetch cancellation and timeouts (`AbortSignal.timeout`) (+)
- [ ] Timers: `setTimeout(fn, 0)` is not 0, `setInterval` drift, `requestAnimationFrame`, `requestIdleCallback`

### Browser & DOM (A2)
- [ ] DOM tree, node vs element, query & traversal, `DocumentFragment`, `innerHTML` vs `textContent` (XSS!)
- [ ] Events: capturing → target → bubbling, `stopPropagation` vs `preventDefault`, **delegation**, `{ once, passive, capture }`, `CustomEvent`
- [ ] Page lifecycle: `DOMContentLoaded`, `load`, `beforeunload`, `visibilitychange`; `<script defer|async|type=module>`
- [ ] Forms: `submit` event, `FormData`, constraint validation API

### Browser APIs (A2)
- [ ] `fetch`: `response.ok` (fetch does **not** reject on 4xx/5xx), headers, `credentials`, JSON/Blob, abort, retry
- [ ] Storage: cookies (`HttpOnly`, `Secure`, `SameSite`, `Max-Age`) vs `localStorage` vs `sessionStorage` vs IndexedDB: size, lifetime, who can read it, when to use which
- [ ] Graphics/media: Canvas 2D basics (draw, animate with rAF), `<video>`/`<audio>` API, SVG vs Canvas
- [ ] Observers: `IntersectionObserver` (lazy load, infinite scroll), `ResizeObserver`, `MutationObserver` (+)
- [ ] `URL` / `URLSearchParams`, History API (`pushState`, `popstate`): the basis of client routing
- [ ] Web Workers, `BroadcastChannel`, `postMessage` (+); Clipboard, Geolocation, Notifications (awareness)

### Dev tools (A2)
- [ ] DevTools: Elements (computed styles, event listeners), Console (`$0`, `console.table`, `console.time`), **Sources** (breakpoints: line, conditional, DOM, XHR/fetch, logpoints, call stack, scope, watch), Network (throttling, initiator, HAR), Application (storage), Performance & Memory (heap snapshot, detached nodes)
- [ ] VS Code debugger for Vite/Vitest; `debugger` statement; source maps
- [ ] npm: `package.json` fields, scripts, `dependencies` vs `devDependencies` vs `peerDependencies`, semver `^ ~`, lock files, `npm ci`, `npx`
- [ ] ESLint (flat config `eslint.config.js`, rules, plugins, `--fix`), Prettier, `.editorconfig`, husky + lint-staged
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
