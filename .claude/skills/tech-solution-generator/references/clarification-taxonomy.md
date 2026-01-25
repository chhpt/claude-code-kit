# Clarification taxonomy (frontend technical solution)

Use this taxonomy to scan a spec/brief and identify high-impact ambiguities. Mark each as: Clear / Partial / Missing.

## Functional scope & behavior

- Core user goals & success criteria
- Explicit out-of-scope
- User roles/personas (if role-based behavior differs)

## Domain & data model

- Entities, attributes, relationships (and where state lives: server vs client)
- Identity/uniqueness rules (IDs, keys, slug rules)
- Lifecycle/state transitions (draft/published, enabled/disabled, etc.)
- Data volume/scale assumptions (list sizes, pagination)

## Interaction & UX flow

- Critical journeys (entry → completion)
- Empty/loading/error states (and retry behavior)
- Accessibility or localization notes (keyboard, screen reader, i18n)

## Non-functional quality attributes

- Performance budgets (LCP/TTI, route-level budgets, list perf)
- Reliability expectations (offline-ish behavior? retry/backoff?)
- Observability needs (frontend logging, error reporting)
- Security & privacy (authN/Z assumptions, PII handling, threat model)
- Compliance constraints (if any)

## Integration & external dependencies

- APIs/services + versioning
- Failure modes + fallback UX
- Data import/export formats (if any)

## Edge cases & failure handling

- Negative scenarios (validation, permissions, unsupported states)
- Rate limiting / throttling behaviors
- Conflict resolution (concurrent edits, optimistic updates)

## Constraints & tradeoffs

- Technical constraints (framework, browser support, SSR/CSR)
- Explicit tradeoffs or rejected alternatives

## Terminology & consistency

- Canonical glossary terms
- Avoid synonyms that cause confusion

## Completion signals

- Acceptance criteria testability
- Definition-of-done indicators (analytics added, docs updated, etc.)

## Question selection rules (max 5)

- Choose the top 5 by (Impact × Uncertainty).
- Don’t ask plan-level “how to code it” questions unless it blocks correctness.
- Each question must be multiple-choice (2–5) or short answer (<=5 words).
- Ask one question at a time; don’t reveal the future queue.

