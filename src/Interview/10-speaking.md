# 10 · Speaking practice with Gemini Live

Two drills: **explain-back** (daily, 15–20 min) and **mock interview** (Sun Nov 1, Sat Nov 7, then weekly).
Tip: copy the prompt into a Gemini chat first, then switch to Live (voice). Speak English the whole time.

## Drill 1: explain-back (end of every session)

Prompt:

```text
You are a senior frontend engineer helping me prepare for a Middle (A2) JavaScript/React interview.
Today's topic: <TOPIC, e.g. "JS event loop and promises">.
I will explain concepts out loud. After each of my answers:
1) rate it 1–5 for correctness and 1–5 for clarity,
2) name what I missed or got wrong,
3) give one short follow-up question that an interviewer would ask next.
Also correct my English if a phrase sounds unnatural, but keep it brief.
Start by asking me the first question about the topic. Ask one question at a time.
```

After the session: write missed points into the README **Gaps log**.

## Drill 2: mock interview (45–90 min)

```text
Act as a strict technical interviewer at a large IT company. You are assessing me for promotion
from Junior to Middle JavaScript Frontend Engineer (React, TypeScript, Redux, React Query, Vitest, Playwright).
Structure the interview:
1) 2 min: ask me to introduce myself and my current project,
2) 30 min: theory: JavaScript, TypeScript, React, testing, web/HTTP/security, CSS,
3) 10 min: engineering: patterns, SOLID, Git, CI/CD, Agile, estimation,
4) 5 min: one behavioural question,
5) 3 min: my questions to you.
Go deeper with follow-ups when my answer is shallow; move on if I clearly don't know.
Don't give feedback during the interview. At the end give a structured report:
strengths, weak topics (ranked), communication issues, and a verdict: would you promote me to Middle? Why?
```

Variant for a single area (e.g. testing): replace step 2 with "40 min only on testing React apps".

## Answer structure (use for every theory question)

1. **Definition** in one sentence
2. **How it works** (mechanism, 2–3 sentences)
3. **Example** from my project ("In our project we use … because …")
4. **Trade-offs / pitfalls**
Keep it to ~1 minute; let the interviewer pull more.

## Prepare these stories (write them here, then rehearse)

- [ ] **Project pitch** (60–90s): product, team size, my role, stack (React, Redux, React Query + Orval, shadcn/Tailwind, Storybook, Vitest/Playwright, React Compiler), what I own
- [ ] **STAR**: a hard bug I debugged (Situation, Task, Action, Result)
- [ ] **STAR**: a feature I delivered end-to-end, incl. estimation and testing
- [ ] **STAR**: a disagreement in code review / with a teammate and how it was resolved
- [ ] **STAR**: something I improved in the process or codebase (matches A2 "propose improvements")
- [ ] **STAR**: a mistake I made and what I learned
- [ ] How I use AI tools in my daily work (see [11](./11-t-shape.md#gen-ai-assisted-development-optional-a2-intermediate))
- [ ] Why I deserve Middle: 3 concrete arguments
- [ ] My questions to the interviewer (2–3)

## Useful phrases

- "Let me think about it for a second…" · "If I understand the question correctly, you're asking…"
- "I haven't used it in production, but as far as I know it works like…" (honest > guessing)
- "There's a trade-off here: on one hand…, on the other hand…"
- "In our project we solved this by…"
