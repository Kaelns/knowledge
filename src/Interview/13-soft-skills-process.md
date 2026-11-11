# 13 · Soft skills, processes, general questions

Matrix (A2): SE Processes **Intermediate** · SE Practices **Intermediate** · Software Design (principles) **Intermediate**.
These questions are asked in **every** assessment, and good answers are cheap to prepare. Answers below are short and in simple English: read them, then **say them in my own words** with Gemini ([10-speaking.md](./10-speaking.md)).
Fill every `<…>` with a real example from my project; a real example beats a perfect definition.

Rule for every situational question: **1) understand / ask, 2) talk early and openly, 3) offer options with trade-offs, 4) agree and document, 5) learn (retro)**. Never "I just do overtime silently" and never "I just refuse".

---

## Code review

### 🔴 CORE How do you prevent conflicts during code review?
- Agree on rules **before** the review: a style guide, ESLint + Prettier + TypeScript in CI, so robots argue about formatting, not people.
- Small PRs with a clear description (what, why, how to test, screenshots). Big PRs make reviewers angry and miss bugs.
- PR template and checklist; link the ticket.
- Comment on the **code, not the person**: "This function may run twice because…", not "You did it wrong".
- Use labels: `nit:` (small, optional), `question:`, `blocker:`. Explain *why* and suggest a fix.
- Also praise good parts.
- If a discussion has more than ~3 comments, **switch to a call**: 5 minutes of talking saves a day of comments.
- In my project: `<we have ESLint + TS checks in CI, 1–2 approvals, PR template…>`

### 🔴 CORE Someone disagrees with your comment. How do you convince them?
- First **listen**: maybe they know something I don't. I ask "Why did you choose this?"
- Then use **facts, not opinion**: docs, a team convention, a benchmark, a small code example or a failing test.
- Focus on the shared goal: fewer bugs, easier to support.
- If it's a matter of taste (`nit`), I let it go: not every hill is worth it.
- If we still disagree on something important: ask a third person (tech lead / team), or discuss it on a team sync and **write the decision** into the guidelines, so next time we don't argue.
- I'm ready to accept that I was wrong.

### 🟡 SHALLOW What do you check in a code review?
Correctness and edge cases → readability and naming → architecture (right place, no duplication) → performance (extra re-renders, heavy loops) → security (XSS, secrets) → a11y → types → tests exist and test behaviour.

---

## Clean code and quality
📚 Old notes: [[Core js 2#SOLID|SOLID (RU notes)]] · [[Core js 1#KISS, DRY and YAGNI|KISS / DRY / YAGNI]] · [[Core js 2#Test pyramid|Test pyramid]]

### 🔴 CORE What is clean code? Code complexity?
- Clean code is code that **another developer can read, change and test easily**. It works, it's simple, it has no surprises.
- Signs: good names, small functions that do one thing, no duplication, no magic numbers, early returns instead of deep nesting, no hidden side effects, consistent style, tests.
- **Cyclomatic complexity** = number of independent paths through code (each `if`, `case`, `&&`, loop adds one). High → hard to test. Keep it ~<10 per function.
- **Cognitive complexity** (SonarQube) = how hard it is for a human to read; nesting costs more.
- Reduce complexity: split functions, early returns, lookup objects instead of long `switch`, extract custom hooks.

### 🔴 CORE DRY / KISS / YAGNI / SOLID
- **DRY**: Don't Repeat Yourself: one source of truth for one piece of knowledge. Pitfall: wrong abstraction is worse than small duplication ("rule of three").
- **KISS**: Keep It Simple: choose the simplest solution that works.
- **YAGNI**: You Aren't Gonna Need It: don't build features "for the future".
- **SOLID** with React examples:
  - **S**: one reason to change: data logic in a custom hook, UI in a component.
  - **O**: open for extension, closed for modification: `variant` props / composition (`children`) instead of editing the component for every case.
  - **L**: a subtype can replace the parent: my `<Button>` wrapper accepts all native button props and behaves like a button.
  - **I**: small interfaces: a component gets only the props it needs, not the whole `user` object.
  - **D**: depend on abstractions: components use an `api` service / React Query hook, not `fetch` directly; easy to mock in tests.

### 🔴 CORE What does high code quality mean to you? How do you measure it?
- Quality = **correct + readable + maintainable + tested + secure + fast enough**.
- How to measure:
  - Static analysis: ESLint errors, TypeScript strict, SonarQube (bugs, code smells, duplication %, complexity)
  - Test coverage (a signal, not a goal; 100% coverage can still test nothing)
  - Bugs found in production / escaped defects, reopened tickets
  - Time to review a PR, time for a new developer to understand a module
  - Bundle size, Lighthouse / Core Web Vitals for performance
  - DORA metrics (team level): deploy frequency, lead time, change failure rate, time to restore
- In my project: `<what we have: CI checks, coverage, Sonar?>`

---

## SDLC and methodologies
📚 Old notes: [[Core js 2#Waterfall|Waterfall]] · [[Core js 2#Agile|Agile]] · [[Core js 2#Scrum|Scrum]] · [[Core js 2#Kanban|Kanban]] · [[Core js 2#User stories|User stories]]

### 🟡 SHALLOW What is SDLC? Secure SDLC?
- SDLC = Software Development Life Cycle: **requirements → design → implementation → testing → deployment → maintenance**.
- Roles: BA / PO (requirements), architect, developers, QA, DevOps, PM / Scrum Master, stakeholders.
- **Secure SDLC** = add security at every stage, not only at the end: threat modeling at design, secure coding rules, SAST (static scan) and dependency scan (`npm audit`, Snyk) in CI, DAST (scan the running app), pentest, security review before release. Bugs found early are much cheaper.

### 🟡 SHALLOW Waterfall vs Agile
- **Waterfall**: stages one after another, full requirements at the start, release at the end. Good when requirements are fixed and change is expensive (government, medical, fixed-price contract).
- **Agile**: small iterations, working software often, feedback, change is expected. Good when requirements are unclear or change.

### 🔴 CORE What is Agile? Main concept
- Agile is a **mindset** (a set of values and principles), not a process. Scrum and Kanban are frameworks that implement it.
- **Agile Manifesto**, 4 values:
  1. Individuals and interactions **over** processes and tools
  2. Working software **over** comprehensive documentation
  3. Customer collaboration **over** contract negotiation
  4. Responding to change **over** following a plan
  ("over" = the right side has value too, the left side has more.)
- Main idea: deliver small pieces of value often, get feedback, adapt. Short feedback loops reduce risk.

### 🔴 CORE Why do we need processes?
- Predictability: the business knows what and when.
- Transparency: everyone sees the status and the problems.
- Quality: reviews, tests, DoD.
- Fewer conflicts and less chaos: clear roles and rules.
- Continuous improvement (retro).
- But process should help the team, not be bureaucracy: if a meeting gives no value, change it.

### 🔴 CORE What is Scrum? Roles, events, artifacts
- Scrum = an Agile **framework** with fixed-length iterations, **sprints** (1–4 weeks, usually 2), with a goal and a potentially shippable increment at the end.
- **3 roles (accountabilities)**:
  - **Product Owner**: owns and prioritizes the Product Backlog, maximizes value, talks to stakeholders.
  - **Scrum Master**: coach of the process, removes blockers, protects the team, helps the team improve. Not a manager.
  - **Developers**: everyone who builds the increment (devs, QA, designers); self-managing, decide *how* to do the work.
- **5 events (ceremonies)**:
  - **Sprint**: the container for all other events.
  - **Sprint Planning**: what we can do (Sprint Goal + selected items) and how.
  - **Daily Scrum** (15 min): progress to the goal, plan for today, blockers. Not a status report for a manager.
  - **Sprint Review**: demo the increment to stakeholders, get feedback, update the backlog.
  - **Sprint Retrospective**: how we worked: what went well, what to improve, action items.
  - (+ **Backlog Refinement**: not an official event but common: clarify and estimate future items.)
- **3 artifacts**: Product Backlog (→ Product Goal), Sprint Backlog (→ Sprint Goal), Increment (→ Definition of Done).
- In my project: `<sprint length, which events we have, who is PO/SM>`

### 🔴 CORE What is Kanban? How does it work?
- Kanban = a method to **visualize and manage flow** of work. No sprints, no fixed roles.
- Practices:
  - **Board** with columns (To Do → In Progress → Review → Testing → Done).
  - **WIP limits**: max number of tasks in a column. "Stop starting, start finishing." Shows bottlenecks.
  - **Pull system**: take a new task only when you have capacity.
  - Explicit rules (when a task can move), measure flow, improve continuously.
- Metrics: **lead time** (request → done), **cycle time** (work started → done), throughput, cumulative flow diagram.
- Release whenever ready (continuous delivery).

### 🔴 CORE Scrum vs Kanban. When to use which?
| | Scrum | Kanban |
|---|---|---|
| Cadence | fixed sprints | continuous flow |
| Roles | PO, SM, Developers | no required roles |
| Changes | not inside a sprint (protect the Sprint Goal) | any time, if WIP allows |
| Planning | sprint planning, estimates in story points | per item, often no estimates |
| Metric | velocity | lead / cycle time |
| Release | end of sprint (or more often) | any time |

- **Scrum** fits product development with a roadmap, new features, a stable team, when you need a rhythm and predictability.
- **Kanban** fits support / maintenance / bug fixing / DevOps: work arrives unpredictably and priorities change daily.
- Many teams use **Scrumban**: sprints + board with WIP limits.

### 🔴 CORE DoR vs DoD, acceptance criteria
- **Definition of Ready**: when a ticket can be taken into a sprint: clear description, acceptance criteria, designs, estimated, no blockers, dependencies known.
- **Definition of Done**: when work is really done, the same for all tickets: code reviewed, tests written and green, CI passes, deployed to test/stage, QA checked, docs updated, PO accepted.
- **Acceptance criteria**: conditions for *one* story (often Given / When / Then). DoD is for *all* stories.
- Functional requirements = *what* the system does (user can reset password). Non-functional = *how well*: performance, security, a11y, availability, browser support.
- User story: "As a `<role>`, I want `<goal>`, so that `<benefit>`"; good stories are **INVEST**: Independent, Negotiable, Valuable, Estimable, Small, Testable.

---

## Estimation
📚 Old notes: [[Core js 2#Estimation techniques|Estimation techniques (RU notes)]]

### 🔴 CORE How do you estimate tasks? Techniques and approaches
- **Relative** units (story points, t-shirt sizes): compare tasks with each other; measure complexity + effort + risk/uncertainty. Fast, team-level, don't depend on who does it.
- **Absolute** units (hours, days): needed for deadlines and fixed-price, but people are bad at them.
- Techniques:
  - **Planning Poker**: everyone shows a Fibonacci card (1, 2, 3, 5, 8, 13…) at the same time; highest and lowest explain; vote again. Avoids anchoring.
  - **T-shirt sizes** (XS–XL): quick, rough, good for a big backlog or roadmap.
  - **By analogy**: compare with a similar task done before.
  - **Expert judgment**: ask the person who knows this area best.
  - **Decomposition (WBS)**: split the task into small parts (≤1 day), estimate each, sum + buffer.
  - **Bucket system**: put many items into buckets of sizes quickly.
  - **Three-point / PERT**: `(Optimistic + 4 × Most likely + Pessimistic) / 6`: gives a range and shows risk.
- Always include: tests, code review, fixes after review/QA, meetings, deployment.

### 🔴 CORE Compare 2 estimation techniques
- **Planning Poker vs T-shirt sizing**: Poker is more precise and gives discussion (finds hidden work), but slower; good for sprint planning. T-shirt is very fast but rough; good for early roadmap / big backlog.
- **Story points vs hours**: points are relative and stable (velocity is used for planning), not tied to one person; hours are clear for business but often wrong and create pressure.

### 🔴 CORE Describe the estimation technique in your current project
Template to fill: `<We estimate on refinement / planning. We use story points (Fibonacci) with Planning Poker in <tool, e.g. Jira / planningpoker>. 1 point ≈ <simple reference task>. Big tasks (>8 / 13) are split. Unknown tasks get a spike first. Velocity is about <N> points per sprint. Or: we estimate in hours / days by decomposition, because…>`
Add one real example: a task where the estimate was wrong and what we changed.

### 🔴 CORE The team disagrees with your estimation (the task is assigned to you)
- In poker, a big difference is **useful**: someone sees risk or a shortcut that others don't.
- I explain my reasons (what work I see, what risks); I listen to theirs. Maybe I missed existing code or they missed a hidden part.
- We break the task down together and re-vote.
- If I'll do it and I'm less experienced in this area, my estimate can be higher, and that's fine: estimates are for the team. We can agree on the team number and I ask for help / pairing, or keep my number with clear reasons.
- If it's still unclear: do a **spike** first or give a range.

### 🔴 CORE You're in the middle of the sprint and understand you failed your estimation. What do you do?
1. **Tell early**: at the daily or directly to the lead / SM / PO, as soon as I see it. Bad news early is good news.
2. Explain why (hidden complexity, a dependency, a bug) and give a **new estimate**.
3. Offer **options**: ask for help or pairing; cut scope (MVP now, the rest in a new ticket); move a lower-priority item out of the sprint; move the task to the next sprint.
4. PO decides based on priority; update the ticket.
5. At the retro: why we missed it and what to change (smaller tasks, spikes, buffers).
- Don't silently work nights: it hides the problem and it will repeat.

### 🔴 CORE What is a spike? What's the result?
- A **spike** = a **time-boxed research task** (e.g. 1–2 days) to reduce uncertainty before estimating or implementing: try a new library, check an API, make a prototype / proof of concept.
- Result is **knowledge, not production code**: a decision or recommendation, a doc / ADR, a prototype, a list of risks, and **estimated, split tickets** for the real work.
- Term comes from XP (Extreme Programming).

---

## Customer and situations

### 🔴 CORE The customer is angry. What do you do?
1. Stay calm, **listen** to the end, don't argue, don't blame the team or the customer.
2. Show I understand: "I understand this is a problem for you."
3. Find the **real cause** with questions (what exactly, impact, since when).
4. Give facts and a plan: what we'll do now (workaround / hotfix), when they'll hear back. Promise only what is realistic.
5. Escalate to my lead / PM if it's outside my responsibility, and keep them in the loop.
6. Follow up, then do a post-mortem / retro so it doesn't repeat.

### 🔴 CORE The customer adds tasks to the current sprint and it means overtime for you
- In Scrum, the Sprint Backlog belongs to the Developers and the **Sprint Goal is protected**. New requests go to the **Product Owner / Product Backlog**.
- If it's really urgent (production bug, deadline): it's a **trade-off, not an addition**: "We can take it, but then X goes out of the sprint. Which is more important?" The PO and customer decide.
- I show the impact openly (capacity, risk to quality), not just "no".
- Overtime can happen once for a real emergency, but not as a regular plan: it hides the real velocity, lowers quality and burns the team. If it happens often, raise it at the retro / with the manager.

### 🟡 SHALLOW Other common situations (prepare one sentence each)
- **Requirements are unclear**: ask the BA/PO with concrete questions, write down assumptions, confirm in the ticket before coding; don't guess.
- **Conflict in the team**: talk 1-on-1 first, focus on facts and the shared goal, find a compromise; escalate to the lead only if it doesn't help.
- **You can't finish by the deadline**: same as "failed estimation": tell early + options.
- **You found a bug in production**: inform the team, check the impact, hotfix or rollback, write a test so it doesn't come back, post-mortem without blame.
- **You don't know the answer / technology**: say it honestly, explain how I'd find out (docs, spike, ask a colleague).
- **Junior asks for help** (Middle expectation): help them find the answer themselves, pair, review their code kindly, share docs.
- **Hard feedback to you**: thank them, ask for examples, agree on what to change.
- **How do you learn new things**: docs, small pet project / lab repo, my knowledge repo, articles, AI tools to explain + check myself.

---

## Git branching strategies

### 🔴 CORE What Git branching strategies do you know?
- **GitFlow**: long-lived `main` (production) and `develop`; short `feature/*` from develop; `release/*` for stabilization; `hotfix/*` from main. Good for versioned releases (mobile apps, boxed software), but heavy, many merges, slow.
- **GitHub Flow**: only `main` + short feature branches → PR → review → merge → deploy. Simple, good for web apps with continuous deployment.
- **GitLab Flow**: GitHub Flow + environment branches (`staging`, `production`) or release branches.
- **Trunk-based development**: everyone merges small changes into `main` at least daily; unfinished features hidden behind **feature flags**. Fast, few conflicts, needs strong CI and tests.
- Also: release branches (`release/1.2`) for support of old versions.
- In my project: `<which one, branch naming, who merges, how we deploy>`

---

## AI and related things

### 🔴 CORE How do you use AI in development? (prepare 2 real stories)
- Tools: `<Claude Code / Copilot / Cursor / ChatGPT>`; modes: inline completion, chat, agent (CLI) mode.
- Use cases: explain unknown code, explore a codebase, generate tests and boilerplate, investigate bugs from logs, write docs, review my own code, learn (this prep!).
- How I work with an agent: plan mode first (decompose the task), give good context (project instruction file like `CLAUDE.md`, relevant files), small steps, check every change.
- **MCP** (Model Context Protocol): a standard way to connect tools and data to an AI agent (e.g. Figma, docs, Jira).
- **Risks and how I handle them**: hallucinations (wrong APIs) → check docs, run tests; security (no secrets or customer data in prompts, follow company policy); over-engineering → keep it simple; licenses of generated code; I am responsible for the code, not the AI. I always review and understand it before commit.
- Story 1 (`<task where AI saved me X hours>`), story 2 (`<suggestion I rejected and why>`).

### 🟡 SHALLOW LLM basics in one breath
LLM predicts the next token; it has a **context window** (limited memory per session); **temperature** controls randomness; it can **hallucinate**, so the output needs verification; a better prompt and context give better results.

---

## Beyond the example: other common questions

The screenshot is only a sample. These are the usual neighbours of those questions at a Junior → Middle assessment.

### Scrum deeper (follow-up questions)

- 🔴 CORE **Why Fibonacci for story points?** Bigger tasks have more uncertainty, so the gaps between numbers grow. It stops fake precision ("is it 7 or 8?").
- 🔴 CORE **Story points vs hours: can you convert them?** No fixed conversion. Points are relative and team-specific. Over time, **velocity** (points done per sprint) is used to plan how much fits into a sprint.
- 🔴 CORE **What is velocity? How can it be misused?** Average points done per sprint, used for planning *inside* the team. Misuse: comparing teams, using it as a KPI, pushing to "increase velocity". The result is point inflation.
- 🟡 SHALLOW **Burndown vs burnup chart**: burndown = work left in the sprint going down to zero; burnup = work done going up plus a scope line, so scope changes are visible.
- 🔴 CORE **Who estimates? Who decides priority? Who decides how to implement?** Developers estimate and decide *how*; PO decides *what* and the order; SM helps the process. A manager doesn't estimate for the team.
- 🟡 SHALLOW **Scrum Master vs Project Manager**: SM is a servant leader and coach (no authority over people, focus on process and blockers); PM plans, controls budget/scope/deadline, reports to the business.
- 🟡 SHALLOW **Can one person be PO and SM?** Not recommended: conflict of interest (PO pushes scope, SM protects the team).
- 🔴 CORE **The Sprint Goal can't be reached. What happens?** Tell the PO early and agree to cut scope while keeping the goal if possible. Only the PO can **cancel a sprint**, when the goal becomes obsolete. Discuss it at the retro.
- 🔴 CORE **Daily takes 40 minutes. What's wrong and how to fix?** It became a status report or a problem-solving session. Fix: 15-minute time-box, focus on the Sprint Goal and blockers, details "after the daily" with only the people involved.
- 🔴 CORE **Retro is boring / nothing changes. What would you do?** Change the format (Start/Stop/Continue, Mad/Sad/Glad, 4Ls, Sailboat). Pick 1–2 concrete action items with an owner. Check them at the next retro.
- 🟡 SHALLOW **Sprint length: how to choose?** Shorter (1–2 weeks) = faster feedback, good for changing requirements; longer = less overhead, but higher risk.
- 🟡 SHALLOW **What is an increment?** A working, tested piece of the product that meets the DoD and could be released.
- 🟡 SHALLOW **Product Goal / Sprint Goal**: Product Goal = long-term target for the product; Sprint Goal = one objective for this sprint that gives the team focus and flexibility in *how*.

### Jira and work items

- 🟡 SHALLOW **Epic → Story → Task / Sub-task, Bug**: epic = big feature split over sprints; story = user value; task = technical work; sub-task = part of a story; bug = defect.
- 🔴 CORE **Severity vs priority**: severity = how bad the impact is (technical); priority = how soon to fix (business). Example: a typo in the logo on the home page = low severity, high priority. A crash in a rare admin report = high severity, may be lower priority.
- 🟡 SHALLOW **What makes a good bug report?** Title, steps to reproduce, expected vs actual result, environment (browser, version, user role), screenshots / video / logs, severity.
- 🟡 SHALLOW **Ticket workflow** in my project: `<To Do → In Progress → Code Review → QA → Done>`.
- 🟡 SHALLOW **Prioritization techniques**: MoSCoW (Must / Should / Could / Won't), value vs effort matrix, RICE (Reach × Impact × Confidence / Effort).

### Estimation deeper

- 🔴 CORE **How do you estimate a task you've never done?** Split it into known and unknown parts. Estimate the known parts; for the unknown parts do a **spike** or give a **range** (best / likely / worst). Add a buffer for risk. Re-estimate after the spike. Say the uncertainty out loud.
- 🟡 SHALLOW **Cone of uncertainty**: estimates at the start of a project can be wrong by ×4. They get more accurate as we learn, so re-estimate often.
- 🟡 SHALLOW **Why are estimates often wrong?** Optimism, hidden work (tests, review, fixes, meetings), unclear requirements, dependencies, interruptions.
- 🟡 SHALLOW **Estimate vs commitment vs target**: estimate = a forecast with uncertainty; target = what the business wants; commitment = what we agree to deliver. Don't let a manager turn an estimate into a promise silently.
- 🟡 SHALLOW **#NoEstimates**: some teams skip estimating; they split all stories to a similar small size and count items (throughput). Common in Kanban.

### Technical debt and improvements (A2: "propose improvements")

- 🔴 CORE **What is technical debt? How do you manage it?** Shortcuts that make future changes slower (no tests, copy-paste, old libraries, bad structure). Manage it: make it visible (tickets in the backlog, a TODO with a ticket link), explain the *business cost* to the PO, reserve ~10–20% of each sprint, follow the **boy scout rule** (leave code a bit cleaner), refactor together with the feature work in the same area.
- 🔴 CORE **How do you convince the business to spend time on refactoring?** Speak in business terms: "Features in this module take 2× longer and have most bugs. 3 days of refactoring will save X hours per sprint and reduce bugs." Use data: bug count, lead time, incidents. Offer a small step first.
- 🔴 CORE **What did you improve in your project?** (STAR, prepare one) `<e.g. added a PR template, sped up CI with caching, introduced Storybook, added tests for a critical flow, wrote a guideline>`
- 🟡 SHALLOW **Legacy code: you need to change it. How?** Understand it first (read, debug, ask the author), add tests around the current behaviour (characterization tests), then change in small steps; don't rewrite everything.
- 🟡 SHALLOW **Rewrite vs refactor**: refactor in small steps is almost always safer; rewrite only when the old code can't support new needs, and then do it step by step (strangler pattern).

### Release, quality and environments

- 🔴 CORE **Describe your CI/CD and release process.** `<PR → lint, typecheck, unit tests, build in CI → review → merge → deploy to dev/stage → QA → prod; how often; who presses the button; rollback>`
- 🟡 SHALLOW **Environments**: local → dev → QA/test → staging (copy of prod) → production. Why staging: test with prod-like data and config before users see it.
- 🟡 SHALLOW **Feature flags**: deploy code switched off, enable for some users (canary / A/B), turn off quickly without a redeploy. Remove old flags (they are tech debt).
- 🟡 SHALLOW **Semantic versioning**: `MAJOR.MINOR.PATCH`: breaking change / new feature / bug fix.
- 🔴 CORE **A critical bug in production after your release. Your actions?** Inform the team and stakeholders, check the impact, **rollback or hotfix** (whichever is faster and safer), verify, write a regression test, **blameless post-mortem**: root cause plus how to prevent it (missing test? missing review step?).
- 🟡 SHALLOW **Quality gates**: rules that block a merge or release: CI green, coverage threshold, no new Sonar issues, approvals.
- 🟡 SHALLOW **Pair / mob programming**: two (or the whole team) at one task; better knowledge sharing and quality, but costs time. Good for hard tasks and onboarding.
- 🟡 SHALLOW **Bus factor**: how many people can leave before the project stops. Increase it: code review, docs, pairing, rotation of tasks.

### Communication and teamwork (behavioural)

Answer these with **STAR** (Situation, Task, Action, Result), 1–2 min, a real story.
- 🔴 CORE **Tell me about a conflict with a colleague and how you solved it.**
- 🔴 CORE **Tell me about a mistake you made.** Show ownership: what happened → I told the team → fixed → what I changed so it doesn't repeat.
- 🔴 CORE **What are your strengths and weaknesses?** Strength with proof (example). Weakness = a real but non-critical one + what I do about it ("I sometimes go too deep into details; now I time-box research and ask earlier").
- 🔴 CORE **Why do you think you are ready for Middle?** Work independently on features end-to-end, estimate and deliver, review others' code, help juniors, propose improvements: give 1 example for each.
- 🟡 SHALLOW **How do you give feedback?** **SBI**: Situation, Behaviour, Impact. "In yesterday's PR (S), the API errors were not handled (B), so users see a blank screen (I). What do you think about adding…?" Private, specific, timely.
- 🟡 SHALLOW **Disagree and commit**: I can disagree in the discussion, but after the team decides, I support the decision fully.
- 🟡 SHALLOW **How do you say no to a manager / customer?** Not "no", but the trade-off: "Yes, if we move X" / "Yes, by date Y" / "Here is a smaller version for now".
- 🟡 SHALLOW **Two urgent tasks from two people at once?** Don't choose alone: bring both to the PO / lead, show the impact, they set the priority; inform both people.
- 🟡 SHALLOW **You are blocked by another team / backend not ready.** Tell at the daily, agree on an API contract (OpenAPI), mock it (MSW / Orval mocks) and continue the frontend; escalate if the deadline is at risk.
- 🟡 SHALLOW **Communication in a distributed team**: written over verbal for decisions, short clear messages, status visible in Jira, overlap hours for calls, record decisions.
- 🟡 SHALLOW **Onboarding a new developer**: README / setup guide, a buddy, a small first task, pair on the first PR, explain architecture and processes.
- 🟡 SHALLOW **You notice a colleague's code has a security issue / bad practice.** Tell them privately and kindly with a suggested fix; if it's serious (security, data), make sure it's fixed before release, involve the lead if needed.
- 🟡 SHALLOW **How do you manage your time / stay focused?** Plan the day from the sprint board, split big tasks, time-box research, block focus time, say early when stuck (my rule: stuck > 1–2h → ask).
- 🟡 SHALLOW **Where do you see yourself in 1–2 years?** Strong Middle → deeper in architecture / testing / performance, mentoring juniors, `<a goal of mine>`.

### Documentation (A2: "maintain / create simple docs")

- 🟡 SHALLOW **What documentation do you write?** README (setup, scripts), how-tos, coding guidelines, ADRs (Architecture Decision Records: context → decision → consequences), Storybook docs for components, JSDoc for tricky functions, comments for *why*, not *what*.
- 🟡 SHALLOW **Code comments: when?** When the code can't explain *why* (business rule, workaround with a link to a bug). Prefer clear names over comments.

## Quick self-check (answer aloud, ~1 min each)

- [ ] 🔴 How do you prevent conflicts in code review / convince someone?
- [ ] 🔴 Clean code, complexity, DRY/KISS/YAGNI, SOLID with examples
- [ ] 🔴 Agile values; why processes
- [ ] 🔴 Scrum roles + events + artifacts; Kanban practices; Scrum vs Kanban + when
- [ ] 🔴 DoR vs DoD
- [ ] 🔴 Estimation techniques; compare 2; my project; team disagrees
- [ ] 🔴 Failed estimation mid-sprint; spike
- [ ] 🔴 Angry customer; customer adds tasks → overtime
- [ ] 🔴 Branching strategies + mine
- [ ] 🔴 Code quality + how to measure
- [ ] 🔴 AI in my work: 2 stories + risks
- [ ] 🔴 Velocity + its misuse; Fibonacci; who estimates / prioritizes / decides how
- [ ] 🔴 Sprint goal at risk; long daily; boring retro
- [ ] 🔴 Severity vs priority; estimating a task I've never done
- [ ] 🔴 Tech debt: manage it + convince the business; what I improved (STAR)
- [ ] 🔴 My CI/CD and release process; production bug after my release
- [ ] 🔴 STAR: conflict, mistake; strengths/weaknesses; why I'm ready for Middle

## Links
- [Scrum Guide 2020](https://scrumguides.org/scrum-guide.html) (13 pages, read the roles/events/artifacts parts)
- [Agile Manifesto](https://agilemanifesto.org/) · [Atlassian: Kanban](https://www.atlassian.com/agile/kanban) · [Atlassian: Scrum vs Kanban](https://www.atlassian.com/agile/kanban/kanban-vs-scrum)
- [Atlassian: Git workflows](https://www.atlassian.com/git/tutorials/comparing-workflows)
