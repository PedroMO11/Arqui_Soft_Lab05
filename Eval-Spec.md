## A. Overall criteria

### O1 — Sections present and on-vocabulary
- Checks: All nine §3 sections exist and are non-empty; every domain noun
  used matches the glossary in §5 (Incident, Customer/Engineering/Support
  Escalation, LLM, Harness, MCP, SLA, SPOF, Peak period).
- PASS: All nine sections present; no term used that is absent from or
  contradicts §5.
- FAIL: A section missing/empty, or a term invented that §5 does
  not define.
- Where: README §1–9, cross-checked against §5.

### O2 — Problem stands on its own   [CRITICAL]
- Checks: §2 states what hurts and why it is hard, with a concrete failure
  description traceable to a persona's stated pain.
- PASS: All five problems named in README §2 appear, each with a concrete
  failure description that matches (in substance, not necessarily wording)
  at least one persona's Current pain.
- FAIL: A problem is summarized so vaguely a reader cannot tell what
  actually went wrong, or a problem invents a failure no persona file
  states.
- Where: README §2, cross-checked against `Users/*.md` Current pains.

### O3 — Every objective traces to a pain   [CRITICAL]
- Checks: Each numbered §3 objective names the problem(s) it attacks and the
  persona(s) whose needs it answers.
- PASS: All 6 objectives cite a Problem number and at least one persona by
  name; every Problem 1–5 is attacked by at least one objective.
- FAIL: An objective with no traceable pain, or a Problem no objective
  addresses.
- Where: README §3, cross-checked against §2 and `Users/`.

### O4 — Out of Scope items carry reasons
- Checks: Every §4 exclusion states why, not just what.
- PASS: All 5 rows have a non-empty "Why" grounded in a fact from another
  README section or a persona file.
- FAIL: Any row whose reason is missing or is circular ("because it's out of
  scope").
- Where: README §4.

### O5 — Key Product Concepts cover every noun used later
- Checks: §5 defines every domain noun the rest of the document relies on.
- PASS: Every capitalized/glossary term used in §2–9 (Incident, the three
  Escalation types, LLM, Harness, MCP, Tool action, SLA, SPOF, Peak period,
  Permission boundary) is defined in §5.
- FAIL: A term used in §6–9 that §5 never defines.
- Where: README §5, cross-checked against §2–9.

### O6 — Key Product Decisions state alternatives and reasoning
- Checks: Each §7 decision names the alternative considered and why it lost.
- PASS: All 6 decisions include an "Alternative considered" and a
  "Rejected — because" clause tied to a stated problem.
- FAIL: A decision stated as a bare choice with no alternative or no
  reasoning.
- Where: README §7.

### O7 — Expected User Experience is checkable, per role
- Checks: §8 gives one claim block per persona, each claim independently
  verifiable.
- PASS: All 5 personas have a claim block; every claim names an observable
  condition (a time bound, a match, a visible label) rather than a vague
  adjective ("good", "fast").
- FAIL: A persona missing a block, or a claim with no observable condition.
- Where: README §8.

### O8 — Main Flows are numbered with a responsible role per step
- Checks: §9 flows are numbered and each step names who performs it.
- PASS: All 8 flows numbered; every step names a persona, "Genius", or a
  named component from §5.
- FAIL: A step with no named responsible role/actor.
- Where: README §9.

### O9 — No contradiction between sections   [CRITICAL]
- Checks: Scope (§4), Decisions (§7), and Flows (§9) agree with each other.
- PASS: Nothing in §9 performs an action §4 excludes; nothing in §7 is
  contradicted by a flow in §9; SLA figures agree with §5 (1 day / 3
  days) everywhere they appear.
- FAIL: A flow step that does something §4 says is out of scope, an SLA
  number that disagrees between sections, or a decision contradicted by a
  later flow.
- Where: README §4, §7, §9 (cross-check).

---

## B. Per-persona criteria

### Support Engineer (`SE`)

#### SE1 — Needs answered
- Checks: Each Need in `Users/SupportEngineer.md` is answered by a §3
  objective, §7 decision, or §9 flow.
- PASS: All 4 Needs map to at least one README element.
- FAIL: A Need with no corresponding objective/decision/flow.
- Where: README §3, §7, §9; `Users/SupportEngineer.md` Needs.

#### SE2 — Pains addressed
- Checks: Each Current pain is visibly resolved by README text.
- PASS: All 4 pains trace to the specific text that resolves them (not just
  a generic mention).
- FAIL: A pain with no resolving text.
- Where: README §2, §3, §7; `Users/SupportEngineer.md` Current pains.

#### SE3 — Day-in-the-work covered by Main Flows
- Checks: Her 7-step day is fully covered by §9.
- PASS: Every step in her "A day in their work" maps to a step in Flow 1, 3,
  4, 5, or 6.
- FAIL: A step in her day with no corresponding flow step.
- Where: README §9; `Users/SupportEngineer.md`.

### Platform Engineer (`PE`)

#### PE1 — Needs answered
- PASS: All 4 Needs map to a README element. FAIL otherwise.
- Where: README §3, §7, §9; `Users/PlatformEngineer.md` Needs.

#### PE2 — Pains addressed
- PASS: All 3 pains trace to resolving text. FAIL otherwise.
- Where: README §2, §3, §7; `Users/PlatformEngineer.md` Current pains.

#### PE3 — Day-in-the-work covered
- PASS: All 6 steps map into Flow 2, 4, 6, or 7. FAIL otherwise.
- Where: README §9; `Users/PlatformEngineer.md`.

### Data Science Lead (`DSL`)

#### DSL1 — Needs answered
- PASS: All 4 Needs map to a README element. FAIL otherwise.
- Where: README §3, §7, §9; `Users/DataScienceLead.md` Needs.

#### DSL2 — Pains addressed
- PASS: All 3 pains trace to resolving text. FAIL otherwise.
- Where: README §2, §7; `Users/DataScienceLead.md` Current pains.

#### DSL3 — Day-in-the-work covered
- PASS: All 5 steps map into Flow 4, 7, or 8. FAIL otherwise.
- Where: README §9; `Users/DataScienceLead.md`.

### Incident Manager (`IM`)

#### IM1 — Needs answered
- PASS: All 4 Needs map to a README element. FAIL otherwise.
- Where: README §3, §7, §9; `Users/IncidentManager.md` Needs.

#### IM2 — Pains addressed
- PASS: All 3 pains trace to resolving text. FAIL otherwise.
- Where: README §2, §7; `Users/IncidentManager.md` Current pains.

#### IM3 — Day-in-the-work covered
- PASS: All 5 steps map into Flow 4, 7, or 8. FAIL otherwise.
- Where: README §9; `Users/IncidentManager.md`.

---