# Coffee Atlas — Agent Working Agreement

This file applies to all coding agents and contributors working in this repository. **Default mode: Balanced.** The maintainer may override the mode for any individual task. These are modes of collaboration, not separate products, environments, branches, or required toolchains.

## Product mission

**Turn every cup into an experiment, and every experiment into knowledge.**

Coffee Atlas is an **AI-native, mobile-first coffee learning companion**. Its first user workflow is:

1. Discover a new bean from a packaging photo or natural-language description.
2. Learn about its origin, processing, roast, and uncertainty; get an **editable, explained** AI recipe.
3. Brew, describe actual parameters and tasting feedback, and discuss plausible adjustments.
4. **On explicit Save:** generate a structured record **draft** from the conversation.
5. Review/correct the draft, then **explicitly confirm** to persist the related records.
6. Continue learning from confirmed history and optional active Brew Journeys.

Keep user observations separate from AI hypotheses, label facts separate from inference, and planned recipes separate from actual brews. Unknown data stays unknown. Never invent farm coordinates, shop addresses, sensory measurements, or causal certainty. Confirmed structured records, not raw chat transcripts, are the source of truth for saved facts.

## Source of work

- [#1 Roadmap](https://github.com/BrandonTing/coffee_atlas/issues/1) defines product scope and sequencing.
- **GitHub Issues are feature specifications.** Read the relevant issue, acceptance criteria, blockers, and current code before changing anything.
- Build **one small, independently testable vertical slice** at a time; a slice should deliver observable behavior across the layers it needs, not a speculative horizontal framework.
- Existing ADRs and tests take precedence over unsupported assumptions. If a decision is important and durable, write a short ADR; do not turn every small edit into documentation.
- Don't repeatedly re-interview the maintainer about product decisions already recorded in issues or ADRs.

## Choose a collaboration mode

Use an explicit mode supplied in the request (e.g. `Mode: Ship`, `Mode: Balanced`, `Mode: Deep Learning`). **Otherwise use Balanced without asking**. Scope the choice to the current task; do not infer the maintainer's available time from the clock. If the request is explicitly read-only, do not edit code regardless of mode.

### Ship Mode — minimal maintainer time

**Use when:** the maintainer wants a small feature/fix delivered and has little time to write or study code.

- Agent owns scoped implementation, relevant tests, and direct verification.
- Avoid optional explorations, broad refactors, multiple design proposals, and tutorial output.
- Show a concise handoff: what changed, what works, what was actually verified, known risks, and any **decision requiring the maintainer**.
- A brief explanation of non-obvious design choices is welcome; an exercise is **not mandatory**.
- Do not skip checks, silently broaden scope, or claim tests passed unless they ran.

### Balanced Mode — **default**: ship and learn

**Use when:** the maintainer wants measurable product progress while building engineering judgment.

- Agent implements **one small vertical slice**, adds appropriate tests, and verifies behavior.
- Identify exactly **ONE Learning Gate**: the most important unfamiliar concept, architectural decision, or trade-off in that slice. Do not generate a tutorial for every changed file.
- In the PR or handoff, explain the Learning Gate using:
  1. **Problem** — what the design solves.
  2. **Choice** — why this approach was selected.
  3. **Alternative** — one plausible option and its trade-off.
  4. **Code** — relevant file/entry point.
  5. **Try it** — one small review question or practical exercise requiring independent thought.
- Keep the gate targeted to the actual code, preferably Effect 4, SvelteKit boundaries, data models, structured AI output, or XState where relevant.
- The maintainer does **not** need to write every line. AI-generated code is acceptable; comprehension and meaningful review are the learning outcomes.
- Surface important decisions for human review rather than silently committing irreversible architecture changes.

### Deep Learning Mode — maintainer-led exploration

**Use when:** the goal is understanding a technique rather than shipping as quickly as possible.

- Teach the relevant concept using authoritative sources and this repository's actual requirements.
- Compare one or two viable approaches, provide a small hands-on exercise or isolated experiment, and review the maintainer's attempt.
- **Do not implement production feature code on the maintainer's behalf** unless explicitly asked in this task.
- Small illustrative code snippets and throwaway spikes are fine when clearly identified and useful.
- Don't block teaching on a giant planning document or force every question into a multi-session course.

### Switching modes

A subsequent message can switch modes without rewriting existing issues, redoing completed work, or creating new GitHub workflow labels. User directions for a task override the default. For mixed tasks, keep the requested implementation scope fixed and apply the chosen mode to the explanation/review depth.

## Technology and architecture

- Current direction: TypeScript, SvelteKit (verify actual stable/version compatibility), Effect 4 for Schema, typed failures, services and async workflows. Treat these as learning and product directions, not permission to overengineer.
- Introduce abstractions only when a concrete feature needs them. Favor simple, testable interfaces and readable domain terminology.
- Keep AI provider calls behind a server/trusted boundary; never ship secret API credentials to the browser. API key vs ChatGPT Login and billing decisions are deferred; don't lock the project to an auth mechanism prematurely.
- XState v6 / experimental `@xstate/effect` is **optional research**, not a mandatory dependency. Adopt/defer/reject it based on demonstrated workflow complexity and Svelte compatibility.
- Preserve provenance and user confirmation for AI-extracted data. AI responses must pass schema validation at the boundary and may require user correction.
- The first release should not require Rust, 3D terrain, a general agent framework, or a login system.

## Token- and time-efficient execution

- **Read narrowly:** the task issue, AGENTS.md, relevant changed files, and matching ADRs/tests. Expand only when evidence requires it.
- Reuse existing specifications. Do not regenerate a large plan when the issue already explains the behavior.
- Prefer one agent and one implementation path. **Do not automatically fan out to multiple subagents, model panels, or exhaustive architecture reviews.** Use them only for genuinely complex/high-risk decisions or when explicitly requested, noting the cost.
- Select skills deliberately rather than chaining all available skills. Example: focused TDD for a feature, targeted code review for a risky diff, or a short teaching discussion for the Learning Gate. Never assume a named third-party skill is installed.
- Run the narrowest useful checks during development; run broader checks when the change is substantial or before claiming overall project health. Report which checks were and were not run.
- Avoid huge context dumps, repetitive explanations, and generating unused scaffolding. Summarize session context in the PR/issue when needed.
- If blocked by missing credentials or access, do not fake outputs; complete independently verifiable portions and state the blocker.

## Delivery and review

1. Restate the selected issue/slice and current **mode** briefly.
2. Implement only the scope required by that slice; don't quietly complete unrelated issues.
3. Verify against acceptance criteria through tests and, where feasible, the actual UI/API. A successful typecheck alone does not prove behavior.
4. Open a **small PR** explaining: what changed, acceptance criteria met, verification evidence, limitations, and any ADR.
5. In **Balanced Mode**, include the one Learning Gate in the PR description or handoff. In Ship Mode, keep a short decision/risk note. In Deep Learning Mode, provide the exercise/review unless implementation was explicitly requested.
6. Leave PR merge and issue completion to maintainer review. **Do not close an issue** merely because code was generated or a PR opened.
7. Do not mark the user-facing feature as complete if actual behavior was not verified.

### Suggested PR handoff template

```md
## Scope
Issue #N · Mode: Balanced / Ship / Deep Learning
User-visible behavior added:

## Verification
- [ ] Relevant tests (commands/results):
- [ ] Manual UI/API verification (steps/results):
- Not run / blocked:

## Design and risk
- Key decision or constraint:
- Known limitations:

## Learning Gate (Balanced)
- Problem:
- Choice and why:
- Alternative:
- Code reference:
- Try it (one question/exercise):
```

## First implementation milestones

- **#2 Foundation:** verify SvelteKit/Effect versions, scaffold the app, establish a minimal trusted AI-provider boundary; optional XState/Effect spike only.
- **#3 Discover:** picture or text → AI bean explanation → editable suggested recipe (no prior collection entry required).
- **#4 Brew/Reflect/Save:** continue AI conversation → capture actuals and taste → draft on Save → user review/correction → confirmed structured persistence.
- **#5 onward:** active/historical Brew Journeys, comparisons, origin map, and deeper multi-session recommendations.

These issues remain **feature-first** regardless of development mode. The Learning Gate is an implementation practice, not a replacement for acceptance criteria.
