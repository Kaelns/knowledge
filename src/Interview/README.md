# Middle (A2) Frontend Interview Prep

Goal: pass the **A1 Junior → A2 Software Engineer** assessment (Primary skill: JavaScript Frontend).
Interview: **not earlier than Nov 9, 2026**. Phase 1 makes me interview-ready; Phase 2 closes gaps and covers optional / T-shape skills.

## How to use

1. Each day: open the day's sub-plan files, go through the **checklist → questions → hands-on** blocks.
2. Last 15–20 min of every session: **explain-back with Gemini Live** ([10-speaking.md](./10-speaking.md)).
3. Tick `- [ ]` boxes in the sub-plans; write anything I couldn't explain into the **Gaps log** at the bottom.
4. Write new knowledge into the sub-plan files (short) – they become my interview cheat sheets.

Legend in sub-plans: **(A2)** = explicitly in the matrix for Middle, **(+)** = my extra, interviewers often ask it.

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

## Phase 1: get interview-ready (Oct 26 – Nov 1)

Time: ~2h tonight, 3–4h Tue–Wed after work, ~6h on each day off (Thu–Sun). **≈ 33h**.

| Day | Time | Focus | Files |
|-----|------|-------|-------|
| **Mon 26** (evening) | 2h | Self-assessment against the tracker below (30m) · JS core refresh: scope, closures, `this`, prototypes · create the **lab repo** (see 04 → Setup) | 01, 04 |
| **Tue 27** | 3.5h | JS async + event loop, Browser APIs, DevTools · TS part 1 (A1 list as a quick check) | 01, 02 |
| **Wed 28** | 3.5h | TS A2 list (narrowing, mapped/template types, tsconfig) · React core: refs, forms, context, performance | 02, 03 |
| **Thu 29** | 6h | React ecosystem: Redux Toolkit + thunk, React Query, router, React 19/Compiler, patterns (4h) · HTML/CSS/Sass/BEM/Tailwind/a11y (2h) | 03, 05 |
| **Fri 30** | 6h | **Testing day**: Vitest + RTL + user-event + MSW + hooks/Redux/React Query tests + Playwright. Mostly hands-on in the lab repo | 04 |
| **Sat 31** | 6h | Web platform: HTTP, caching, auth/cookies, CORS, rendering strategies, security (4h) · Node.js core (2h) | 06, 07 |
| **Sun 1** | 6h | Engineering: patterns, SOLID/GRASP, architecture, Git/CI/CD/Docker, Agile/estimation, algorithms, SQL (4h) · **Mock interview #1** with Gemini (1h) · write the Gaps log (1h) | 08, 10 |

## Phase 2: deep dive + optional + loose ends (Nov 2 – Nov 8)

Time: ~3h on workdays, ~5h on the weekend. **≈ 25h**.

| Day | Focus | Files |
|-----|-------|-------|
| **Mon 2** | Fix top gaps from mock #1 · live coding set A (JS) | Gaps log, 09 |
| **Tue 3** | Optional skills: Web Performance deep dive (DevTools Performance panel, Lighthouse, Core Web Vitals, bundle analysis) · PWA & AMP (service worker, manifest) | 06 |
| **Wed 4** | T-shape: React Native basics (A2 = Intermediate!) · CMS (headless CMS concepts) | 11, 06 |
| **Thu 5** | Cloud & hosting basics, Docker hands-on, deployment strategies · Gen AI assisted dev (MCP, CLAUDE.md, agent workflows) | 11, 08 |
| **Fri 6** | Deep research: React internals (Fiber, reconciliation, RSC), TS compiler / module resolution, Node streams · CS: data structures implementation, symmetric/asymmetric crypto | 03, 02, 07, 08 |
| **Sat 7** | **Mock interview #2** (full, 1.5h) · live coding set B · fix gaps | 10, 09 |
| **Sun 8** | Light day: re-read all "Key questions", rehearse project pitch + STAR stories, rest | all |

## After Nov 9: maintenance (30–45 min/day until the interview)

- 1 random sub-plan → answer its "Key questions" aloud with Gemini (15m)
- 1 live coding task (15–20m)
- 1 item from the Gaps log (10m)

## Progress tracker

Rate 0–3 (0 = never heard, 1 = heard/read, 2 = can explain, 3 = can explain **and** code it). Target before the interview: **3** for Core, **2** for Required/Optional.

| Area | Start | After Ph.1 | After Ph.2 |
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
