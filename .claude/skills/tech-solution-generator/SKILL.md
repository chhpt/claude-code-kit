---
name: tech-solution-generator
description: Generate senior-frontend-style technical solutions / design docs (技术方案/设计方案/实现方案) from a feature brief, PRD, or existing spec, including an ambiguity scan + up to 5 high-impact clarifying questions, then produce a structured Markdown technical plan (user journeys, component & state design, data flow, API contracts, non-functional requirements, risks/tradeoffs, rollout, and test plan). Use when asked to “写技术方案/做前端方案/架构设计/实现方案/任务拆解”, or when converting requirements/specs into a frontend technical design.
---

# Tech Solution Generator (Frontend)

## Inputs to look for

- A feature brief in chat, or a doc path (Markdown preferred)
- Existing spec-driven docs (if present): `specs/<feature>/{requirements.md,design.md,tasks.md}`
- Repo constraints: framework (React/Vue/Next/Nuxt), build tooling, UI kit, API style, auth model

If the user did not provide a doc path, ask for the minimum: feature goal + target users + key UI flow(s) + existing page/route where it lives.

## Workflow

### 1) Locate the spec (preferred) and load it once

1. If `.specify/scripts/bash/check-prerequisites.sh` exists, run:
   - `.specify/scripts/bash/check-prerequisites.sh --json --paths-only`
   - Parse `FEATURE_SPEC` and load that file as the source of truth.
2. Otherwise, look for a best-effort spec candidate in this order:
   - `specs/**/requirements.md`
   - `docs/**/` feature docs
   - A user-provided file path

If no spec exists, do not create one; ask the user to provide a doc or run their spec bootstrap flow first.

### 2) Clarify ambiguities (senior FE behavior)

Run a structured ambiguity scan (taxonomy in `references/clarification-taxonomy.md`). Keep an internal coverage map, then ask **exactly one** question at a time:

- Ask **max 5** questions total.
- Each question must be answerable with:
  - multiple choice (2–5 options), or
  - short answer (<=5 words).
- For multiple-choice questions, recommend the best option (1–2 sentences why), then show a table, then instruct how to reply.
- Stop early if the remaining ambiguities are low-impact or the user says “done/stop/proceed”.

After each accepted answer, immediately integrate it into the spec:

- Ensure `## Clarifications` exists and add `### Session YYYY-MM-DD`.
- Append: `- Q: <question> → A: <final answer>`
- Apply the answer to the most relevant section (requirements/design/non-functional/edge cases) and remove contradictions.

### 3) Produce the technical solution doc

Use `assets/tech-solution-template.md` as the skeleton and fill it with concrete, testable content:

- Prefer repo-native patterns (routing, state, data fetching, UI kit, testing stack).
- Make tradeoffs explicit (alternatives, why rejected, risk).
- Provide a task breakdown that is implementable (small steps with acceptance checks).
- Add a simple diagram (Mermaid) when it reduces ambiguity (data flow, component layering, sequence).

Write output to the most natural place for the repo:

- If `specs/<feature>/design.md` exists: update it.
- Else: create/update `docs/tech/<feature>-design.md` (only if the repo already uses `docs/`).

### 4) Quality bar (before finalizing)

- Every “should” is testable (acceptance criteria, error states, empty/loading).
- Data contracts are explicit (request/response shape, caching/invalidation).
- Accessibility notes exist for interactive UI.
- Performance constraints are measurable (budgets, lazy loading, lists virtualization if needed).
- Security/privacy assumptions are stated (auth, PII handling, XSS/CSRF considerations).

## Output format

Follow the template headings; keep content concise but specific. Prefer bullet lists and tables over paragraphs.

If the user asks for “只要方案，不要代码”, stop after docs; if they ask to implement, transition into an implementation plan and tasks aligned to the doc.
