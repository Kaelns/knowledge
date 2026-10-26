# 08 · Engineering: design, practices, processes, CS fundamentals

Matrix (A2): Design patterns **Novice** (new) · Software Design **Intermediate** · SE Practices **Intermediate** · SE Processes **Intermediate** · SE Knowledge & Experience **Intermediate**.
Old notes: [Core js 2 → Patterns, SOLID, Methodologies, Testing](../../Evernote/Core%20js%202/Core%20js%202.md#design-patterns) · [SQL](../sql/sql.md).

## Checklist

### Design patterns & software design
- [ ] GoF essentials with a **frontend example each**: Singleton (store, API client), Factory, Builder, Observer (events, Redux subscribe), Decorator (HOC, debounce wrapper), Adapter (API DTO → UI model), Facade (API service layer), Proxy (JS `Proxy`, Immer), Strategy (validation rules, payment methods), Command (Redux actions, undo), Composite (component tree), Mediator (event bus), State (state machines / XState) (+)
- [ ] **Dependency Injection / IoC**: constructor injection, React Context as DI, why it helps testing
- [ ] Principles: **SOLID** (with React examples: SRP → split hook/UI, OCP → composition/variants, LSP, ISP → small props interfaces, DIP → depend on an `api` abstraction), **GRASP** (Information Expert, Creator, Controller, Low Coupling, High Cohesion, Polymorphism, Pure Fabrication, Indirection, Protected Variations), KISS, DRY, YAGNI, Clean Code (naming, small functions, no side effects)
- [ ] Paradigms: OOP (encapsulation, inheritance, polymorphism, abstraction), FP (purity, immutability, composition), Reactive (streams, RxJS idea, signals)
- [ ] **Architecture patterns**: Layered, Client-Server, MVC/MVVM, SOA vs **microservices** vs monolith, **event-driven**, micro-frontends (+, Module Federation), BFF (+); Feature-Sliced Design / feature folders for frontend structure (+)
- [ ] "Explain patterns on different layers of your solution": prepare: UI (compound components, HOC), state (Observer/Flux), data (Adapter/Facade over Orval client), infra (Singleton QueryClient)
- [ ] Cross-cutting concerns: logging (Sentry, levels), error handling (error boundaries, global handlers, API error mapping), secure coding (sanitization, secrets)
- [ ] Docs: README, ADRs (+), how-tos, Storybook as living docs, JSDoc

### Practices (A2)
- [ ] Code quality: code review (what I look for, how to give feedback), static analysis (ESLint, TS, SonarQube), dynamic analysis; pros/cons of each
- [ ] **Git**: merge vs **rebase** (and interactive rebase, `--onto`), `stash`, `cherry-pick`, `reset` soft/mixed/hard vs `revert`, `reflog`, fork + PR flow, resolving conflicts, conventional commits
- [ ] Branching strategies: **GitFlow** vs **GitHub Flow** vs **trunk-based** (+ feature flags); centralized (SVN) vs distributed VCS
- [ ] **CI/CD**: continuous integration vs delivery vs deployment; pipeline stages (install → lint → typecheck → test → build → deploy), caching, artifacts, env secrets; write a simple GitHub Actions / GitLab CI YAML
- [ ] Testing on all pyramid levels → see [04](./04-testing.md); performance testing types (load, stress, spike, soak)
- [ ] "Identify gaps and propose improvements": prepare 2 real examples from my project (e.g. flaky e2e, slow CI, missing tests)

### Processes (A2)
- [ ] SDLC stages & roles; **Secure SDLC** (threat modeling, SAST/DAST, dependency scanning)
- [ ] **Scrum** (roles: PO/SM/Dev; events: planning, daily, review, retro, refinement; artifacts) vs **Kanban** (WIP limits, flow, lead/cycle time): pros/cons; Waterfall when it fits
- [ ] **DoR vs DoD**, acceptance criteria, functional vs non-functional requirements, user stories (INVEST)
- [ ] **Estimation**: by analogy, expert, planning poker, decomposition, bucket, t-shirt, story points; relative vs absolute units; estimating unknown tasks (spike, ranges, 3-point / PERT `(O + 4M + P) / 6`)
- [ ] Jira basics; how I'd suggest a process improvement (retro action items)

### CS & SE knowledge (A2)
- [ ] **Big-O**; data structures with implementation details: array vs linked list, stack, queue, hash table (collisions), set, tree/BST, heap, graph (BFS/DFS), trie (+)
- [ ] Algorithms: sorting (quick/merge, JS `sort` is TimSort), binary search, two pointers, sliding window, recursion/memoization
- [ ] **Networks**: OSI/TCP-IP layers, TCP vs UDP, IP, DNS, ports, HTTP over TCP/QUIC
- [ ] **Cryptography**: symmetric (AES: one shared key, fast) vs asymmetric (RSA/ECC: public/private keys, slow), hashing vs encryption, password hashing (bcrypt/argon2, salt), digital signatures (JWT RS256 vs HS256), TLS uses both
- [ ] **SQL** (A2 = complex queries): JOIN types, GROUP BY/HAVING, subqueries, CTEs, window functions (+), `CREATE/ALTER/DROP`, indexes, transactions; SQL vs NoSQL. Revisit [sql.md](../sql/sql.md)
- [ ] **Virtualization vs containerization**: VM (hypervisor, full OS, snapshots, backups) vs **Docker** (image, container, layers, Dockerfile, volumes, compose); multi-stage Dockerfile for a Vite app served by Nginx
- [ ] **Deployment strategies**: blue-green, canary, rolling, feature flags, rollback
- [ ] **Licenses**: permissive (MIT, Apache 2.0, BSD) vs copyleft (GPL, LGPL, AGPL); implications for commercial use
- [ ] Troubleshooting / debugging / profiling (DevTools, Node inspector), IDE power features (multi-cursor, refactor rename, go to definition, debug configs); multithreading awareness (Web Workers, race conditions in async UI)

## Key questions

1. Name 5 patterns you've used in frontend code and where.
2. Explain SOLID with React examples. What's DIP in a React app?
3. What's GRASP? Explain High Cohesion / Low Coupling.
4. Monolith vs microservices vs micro-frontends: when would you choose each?
5. Merge vs rebase. When must you NOT rebase?
6. GitFlow vs trunk-based development.
7. Describe your project's CI/CD pipeline. What would you improve?
8. DoR vs DoD. How do you estimate a task you've never done?
9. Symmetric vs asymmetric encryption. How does TLS use both?
10. Docker container vs VM. What's in a Dockerfile for a SPA?
11. Blue-green vs canary deployment.
12. MIT vs GPL: can we use a GPL library in a commercial product?
13. Write a SQL query: top 3 customers by total order amount in 2025 (JOIN + GROUP BY + ORDER BY + LIMIT).

## Hands-on

- [ ] Implement in JS: `Stack`, `Queue`, `LinkedList`, `LRUCache` (Map), BFS on a graph
- [ ] 6–8 LeetCode easy/medium: Two Sum, Valid Parentheses, Group Anagrams, Merge Intervals, Binary Search, Longest Substring Without Repeating, Top K Frequent, Flatten nested array
- [ ] `git rebase -i` to squash 3 commits in a scratch repo; recover with `reflog`
- [ ] Multi-stage Dockerfile (node build → nginx) for the lab repo; `docker run -p 8080:80`
- [ ] GitHub Actions workflow: lint + typecheck + test + build on PR
- [ ] Write 3 SQL queries with JOINs on the DB in `src/sql`

## Links

- [refactoring.guru: patterns](https://refactoring.guru/design-patterns) · [patterns.dev](https://www.patterns.dev/)
- [Atlassian Git tutorials: merging vs rebasing](https://www.atlassian.com/git/tutorials/merging-vs-rebasing)
- [Scrum Guide](https://scrumguides.org/scrum-guide.html)
- [NeetCode roadmap](https://neetcode.io/roadmap) · [Docker get started](https://docs.docker.com/get-started/)
- [choosealicense.com](https://choosealicense.com/licenses/)
