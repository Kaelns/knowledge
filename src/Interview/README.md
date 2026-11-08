# Middle (A2) Frontend Interview Prep

Goal: pass the **A1 Junior → A2 Software Engineer** assessment (Primary skill: JavaScript Frontend).
Interview: **not earlier than Nov 9, 2026**. **Pass 1** = breadth: touch every likely question once. **Pass 2** = depth only where the mock interview shows gaps + optional skills.

## How to use: speed rules (breadth first)

1. **Start from "Key questions"**, not the checklist. Answer each one aloud in ≤1 min.
   - Can answer → tick, move on.
   - Can't → look it up, **max 10 min**, write a ≤3-line note, move on.
2. **Checklist = scan only.** Read the line, ask "could I explain this in 2 sentences?" If yes, skip. Don't write notes for things I already know.
3. **Time-box every block** (times in the schedule). When the timer rings, move on and dump leftovers into the **Gaps log**. Unfinished ≠ failed; Pass 2 exists for that.
4. **Pass 1 skips** every `(+)` item and every **P3** topic (see the priority line at the top of each sub-plan). Hands-on in Pass 1 is only the testing kata and live coding set A.
5. Last 15 min of a session: **explain-back with Gemini Live** ([10-speaking.md](./10-speaking.md)) on what I just covered.

Legend in sub-plans: **(A2)** = explicitly in the matrix for Middle, **(+)** = my extra (Pass 2). **P1** = asked in almost every interview, **P2** = often asked, **P3** = rare / optional.

## Sub-plans

| #   | File                                                   | Matrix skills                                                                     | A2 level                                              |
| --- | ------------------------------------------------------ | --------------------------------------------------------------------------------- | ----------------------------------------------------- |
| 01  | [JavaScript & Browser](./01-javascript.md)             | JavaScript, JS in Browser, Browser APIs, JS Dev Tools                             | Intermediate (Core / Required)                        |
| 02  | [TypeScript](./02-typescript.md)                       | TypeScript                                                                        | Intermediate (Core)                                   |
| 03  | [React & ecosystem](./03-react.md)                     | JS Top Frameworks, ReactJS                                                        | Intermediate (Core)                                   |
| 04  | [Testing](./04-testing.md)                             | Unit tests (Frameworks), Test Pyramid (SE Practices), Node testing                | Intermediate                                          |
| 05  | [HTML, CSS, a11y](./05-html-css.md)                    | HTML, CSS, Preprocessors, Methodologies, Frameworks, Cross-browser, Accessibility | HTML/CSS **Advanced**, rest Intermediate, a11y Novice |
| 06  | [Web platform](./06-web-platform.md)                   | Protocols, Rendering strategies, Performance, Security, PWA & AMP, CMS            | Intermediate / Novice                                 |
| 07  | [Node.js](./07-nodejs.md)                              | Node.js, Node.js Core                                                             | Novice                                                |
| 08  | [Engineering](./08-engineering.md)                     | Design patterns, Software Design, SE Practices, SE Processes, SE Knowledge        | Intermediate (patterns Novice)                        |
| 09  | [Live coding](./09-live-coding.md)                     | — practice tasks for the coding round                                             | —                                                     |
| 10  | [Speaking (Gemini Live)](./10-speaking.md)             | — speech practice, mock interviews, STAR stories                                  | —                                                     |
| 11  | [T-shape: mobile, cloud, hosting, AI](./11-t-shape.md) | JS Cross-Mobile, Cloud, Hosting, Gen AI Assisted Dev                              | Cross-mobile Intermediate, AI Intermediate (optional) |

Old notes from Junior prep (reuse, don't rewrite): [Core js 1](../../Evernote/Core%20js%201/Core%20js%201.md) · [Core js 2](../../Evernote/Core%20js%202/Core%20js%202.md) (TS, patterns, SOLID, Agile, testing, HTTP, polyfills) · [Programming](../../Evernote/Programming/Programming.md) (Node, React, Redux) · [SQL](../sql/sql.md) · [Node](../nodeJS/nodejs.md)

## Pass 1: breadth (Tue Oct 27 – Sun Nov 1)

Workdays ~3h, days off ~7h. **≈ 34h.** Mon 26 done: JS language core (most of it).

| Day | Time-boxes | Files |
|-----|-----------|-------|
| **Tue 27** | Rest of JS core via key questions (1h) · Async + event loop (1h) · Browser APIs + DevTools: skim (45m) · Gemini (15m) | 01 |
| **Wed 28** | TypeScript: key questions + A2 checklist scan (1.5h) · 5 easy type-challenges (45m) · React fundamentals (45m) | 02, 03 |
| **Thu 29** | React: hooks, refs, forms, context, performance, Compiler (2h) · Redux + React Query + router (1.5h) · lab repo setup (30m) · Testing theory + Vitest + RTL queries (2h) · Gemini (30m) | 03, 04 |
| **Fri 30** | Testing kata 1–6 (3.5h) · HTML/CSS/Sass/BEM/Tailwind/a11y key questions (2h) · Playwright skim + kata 7 (1h) · Gemini (30m) | 04, 05 |
| **Sat 31** | Web: HTTP, caching, cookies/JWT, CORS, TLS (1.5h) · Security: XSS/CSRF/CSP (1h) · Rendering strategies (45m) · Node key questions (1h) · Engineering: patterns, SOLID, Git, CI/CD, Agile/estimation (2h) · Gemini (45m) | 06, 07, 08 |
| **Sun 1** | Engineering rest: Docker, deploy strategies, algorithms, SQL, crypto (1.5h) · Live coding set A, 4 tasks (2h) · Project pitch + 3 STAR stories (1h) · **Mock interview #1** (1h) · sort the Gaps log by priority (1h) | 08, 09, 10 |

## Pass 2: gaps + depth + optional (Mon Nov 2 – Sun Nov 8)

3 workdays ~3h + **2 extra days off** ~7h + weekend. **≈ 30h.** Put the 2 days off where the heavy blocks are (e.g. Thu–Fri), shift the rest.

| Day | Focus | Files |
|-----|-------|-------|
| **Workday 1** | Top 5 P1 gaps from mock #1 · live coding set A (rest) | Gaps log, 09 |
| **Workday 2** | P3 optional: Web Performance (Core Web Vitals, Lighthouse), PWA & AMP, CMS: key questions only | 06 |
| **Workday 3** | React Native (A2 Intermediate) + cloud/hosting + Gen AI: key questions only | 11 |
| **Day off 1** | Deep dives on the weakest P1 areas from mock #1 (testing / TS / React, whichever ranked lowest) · `(+)` items in 01–03 · type-challenges medium | 01–04 |
| **Day off 2** | Live coding set B (3h) · remaining hands-on in 05–08 (Docker, Git rebase, SQL joins) · **Mock interview #2** (1.5h) | 09, 10, 08 |
| **Sat 7** | Fix gaps from mock #2 · Pass-2 depth topics (React internals, Node streams, TS compiler) only if P1 is solid | Gaps log |
| **Sun 8** | Light: re-answer all "Key questions" aloud, rehearse pitch + STAR, rest | all |

## After Nov 9: maintenance (30–45 min/day until the interview)

- 1 random sub-plan → answer its "Key questions" aloud with Gemini (15m)
- 1 live coding task (15–20m)
- 1 item from the Gaps log (10m)

## Progress tracker

Rate 0–3 (0 = never heard, 1 = heard/read, 2 = can explain, 3 = can explain **and** code it). Target before the interview: **3** for Core, **2** for Required/Optional.

| Area | Start | After Pass 1 | After Pass 2 |
|------|-------|-----------|-----------|
| JS core + async | | | |
| Browser & Browser APIs | | | |
| TypeScript (A2 list) | | | |
| React + Redux + React Query | | | |
| Testing (unit / integration / e2e) | | | |
| HTML / CSS / Sass / methodologies | | | |
| Accessibility | | | |
| Web protocols + security | | | |
| Rendering strategies + performance | | | |
| Node.js | | | |
| Patterns / SOLID / architecture | | | |
| Git / CI/CD / Docker / processes | | | |
| Algorithms / DS / crypto / SQL | | | |
| React Native / cloud / hosting | | | |
| Gen AI assisted development | | | |
| Speaking (fluency, structure) | | | |

## Gaps log

Write down anything I couldn't explain or code, then cross it out when fixed.

- [ ] …
