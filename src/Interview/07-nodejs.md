# 07 · Node.js

Matrix (T-shape, new at A2): **Node.js** Novice · **Node.js Core** Novice. Not required at A1, so expect basic but broad questions.
Old notes: [nodejs.md](../nodeJS/nodejs.md) (event loop) · [Programming → Node](../../Evernote/Programming/Programming.md#node).

**Pass 1 priority.** P1: runtime vs browser, event loop phases + `nextTick`, CJS vs ESM, npm/package.json, async error handling. P2: core modules (fs/path/process), EventEmitter, JWT middleware, debugging. P3: streams, JSDoc, loggers, `node:test`.

## Checklist
📚 Old notes: [[Core js 2#Module types AMD CommonJS ES6 UMD|CommonJS vs ESM]] · [[Core js 2#Export and import syntax|import / export]] · [[Core js 2#Promises, async await and the event loop|Event loop (browser)]] · [[Core js 2#Callback hell|Callbacks]] · [[Core js 2#Custom errors|Errors]] · [[Core js 2#Authentication JWT and OAuth|JWT]] · [[Programming#Node|Node (old notes)]] · [[nodejs|nodejs.md]]

- [ ] 🔴 CORE **Runtime**: V8 + libuv, single JS thread + thread pool (fs, crypto, dns), what differs from browser JS (no DOM/`window`, `global`/`globalThis`, `process`, `Buffer`, fs/network access)
- [ ] 🔴 CORE **Event loop phases**: timers → pending callbacks → poll → check (`setImmediate`) → close; `process.nextTick` vs microtasks vs `setImmediate` order
- [ ] ⚪ SKIP **CLI**: `node file.js`, REPL, `--watch`, `--inspect`/`--inspect-brk`, `--env-file=.env`, `--test`, `-e`/`-p`, `node --version`
- [ ] 🟡 SHALLOW **Modules**: CommonJS (`require`, `module.exports`, module wrapper → module scope, `__dirname`), ESM (`import`, `"type": "module"`, `.mjs`/`.cjs`, `import.meta.url`/`import.meta.dirname`), interop
- [ ] 🟡 SHALLOW **Package managers**: npm/pnpm/yarn, `npm init/install/ci/run/outdated/audit/link`, package structure (`main`, `exports`, `bin`, `type`, `engines`, `scripts`), semver, lock file
- [ ] 🟡 SHALLOW **Async**: error-first callbacks → promisified APIs (`fs/promises`, `util.promisify`), async/await
- [ ] 🟡 SHALLOW **Events**: `EventEmitter` (`on`, `once`, `emit`, `off`), event-driven architecture
- [ ] 🟡 SHALLOW **Core APIs**: `console`, `fs` (sync vs async vs promises), `path` (`join` vs `resolve`), `os`, `process` (`argv`, `env`, `exit`, `cwd`), `string_decoder`, timers, `url` (`URL`, `fileURLToPath`), `crypto` (+), streams basics (+: readable/writable/pipe, why streams for big files)
- [ ] 🟡 SHALLOW **Error handling**: `Error` class, custom errors, sync `try/catch` vs async rejections, `process.on('unhandledRejection' | 'uncaughtException')`, graceful shutdown
- [ ] 🟡 SHALLOW **Debugging**: `node --inspect` + `chrome://inspect`, VS Code debugger
- [ ] 🟡 SHALLOW **Auth**: JWT sign/verify (`jsonwebtoken`), middleware that checks `Authorization: Bearer`, expiration, refresh
- [ ] ⚪ SKIP **Testing**: built-in `node:test` + `node:assert`, or Vitest; testing an Express route with supertest
- [ ] ⚪ SKIP **Docs**: JSDoc (`@param`, `@returns`, `@typedef`), type checking JS via JSDoc + `// @ts-check`
- [ ] ⚪ SKIP **Logging**: levels (error/warn/info/debug/trace), structured JSON logs, pino/winston, no secrets in logs
- [ ] ⚪ SKIP **Deployment**: npm scripts (`start`, `build`, `pre`/`post` hooks), env vars, `NODE_ENV`, PM2/Docker basics
- [ ] ⚪ SKIP Express basics (+): routing, middleware chain, error middleware, static files

## Key questions

1. 🔴 CORE How does Node handle many connections on one thread?
2. 🔴 CORE Order of `process.nextTick`, `Promise.then`, `setTimeout(0)`, `setImmediate`.
3. 🟡 SHALLOW CommonJS vs ESM in Node: how do you choose and what are the gotchas?
4. 🟡 SHALLOW `dependencies` vs `devDependencies` vs `peerDependencies`; what does `npm ci` do?
5. 🟡 SHALLOW How do you handle errors in async code and avoid crashing the process?
6. 🟡 SHALLOW Why use streams?
7. 🟡 SHALLOW How does JWT authentication work end-to-end?
8. 🟡 SHALLOW How would you debug a Node script?

## Hands-on (Pass 2)

- [ ] ⚪ SKIP CLI script: read a JSON file from `process.argv[2]`, transform it, write the output with `fs/promises`; handle missing file errors
- [ ] ⚪ SKIP Tiny Express (or `node:http`) API: `POST /login` returns JWT, `GET /me` protected by middleware, pino logs
- [ ] ⚪ SKIP One `node:test` test for the transform function; run with `node --test`
- [ ] ⚪ SKIP Debug it with `--inspect-brk` in Chrome

## Links

- [Node.js Learn](https://nodejs.org/en/learn/getting-started/introduction-to-nodejs)
- [Event loop, timers, nextTick](https://nodejs.org/en/learn/asynchronous-work/event-loop-timers-and-nexttick)
- [Node API docs](https://nodejs.org/api/)
- [jwt.io intro](https://jwt.io/introduction)
