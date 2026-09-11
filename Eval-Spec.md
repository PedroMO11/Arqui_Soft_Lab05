## A. Overall criteria

### O1 — Sections present and on-vocabulary
- Checks: All nine §3 sections exist and are non-empty; every domain noun
  used matches `CONTEXT.md` (Incident, Customer/Engineering/Support
  Escalation, LLM, Harness, MCP, SLA, SPOF, Peak period).
- PASS: All nine sections present; no term used that is absent from or
  contradicts `CONTEXT.md`.
- FAIL: A section missing/empty, or a term invented that `CONTEXT.md` does
  not define.
- Where: README §1–9.

### O2 — Problem stands on its own   [CRITICAL]
- Checks: §2 states what hurts and why it is hard, with verbatim quotes
  where available.
- PASS: All five problems from `CONTEXT.md` §4 appear, each with a concrete
  failure description; at least the three quoted pains carry verbatim
  quotes attributed to a named persona.
- FAIL: A problem is summarized so vaguely a reader cannot tell what
  actually went wrong, or a quote is invented rather than sourced from a
  persona file / `CONTEXT.md`.
- Where: README §2.

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
- PASS: All 5 rows have a non-empty "Why" grounded in a fact from
  `CONTEXT.md`, another README section, or a persona file.
- FAIL: Any row whose reason is missing or is circular ("because it's out of
  scope").
- Where: README §4.

### O5 — Key Product Concepts cover every noun used later
- Checks: §5 defines every domain noun the rest of the document relies on.
- PASS: Every capitalized/glossary term used in §2–9 (Incident, the three
  Escalation types, LLM, Harness, MCP, Tool action, SLA, SPOF, Peak period,
  Permission boundary) is defined in §5 or `CONTEXT.md`.
- FAIL: A term used in §6–9 that §5 never defines and `CONTEXT.md` doesn't
  cover either.
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
  contradicted by a flow in §9; SLA figures match `CONTEXT.md` (1 day / 3
  days) everywhere they appear.
- FAIL: A flow step that does something §4 says is out of scope, an SLA
  number that disagrees between sections, or a decision contradicted by a
  later flow.
- Where: README §4, §7, §9 (cross-check).

### O10 — Referenced acceptance-criteria themes are well-formed
- Checks: Every persona-file "Associated acceptance criteria" entry names a
  distinct, testable theme that `REDALE.md` §R can later assign a stable
  `R#` to; no README section references an ID from a document that does not
  exist yet.
- PASS: No forward reference in README itself to an undefined ID; each
  persona's theme list is internally consistent (no theme worded so
  generically it could mean anything, no two personas' themes silently
  contradicting each other).
- FAIL: README §1–9 references an acceptance-criteria ID directly (it
  should not — that linkage lives in the persona files, per §2 of
  `SPEC-TEMPLATE.md`), or a persona's themes are vague or contradictory.
- Where: README §1–9; `Users/*.md` "Associated acceptance criteria".
- *Repair note: originally written against concrete `R#` numbers guessed in
  the persona files; the personas were corrected to name themes instead
  (those numbers were assigned before `REDALE.md` existed and had already
  drifted — e.g. `R1` meant "permission boundary" for three personas but
  "SLA tracking" for Customer). This criterion and SE4/PE4/DSL4/CU4/IM4
  below were repaired to match, logged in `Eval-Results.md` as an
  out-of-loop edit.*

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

#### SE4 — Acceptance-criteria themes testable and reachable
- Checks: Her cited themes are each testable and will be assignable a
  stable `R#` once `REDALE.md` §R exists.
- PASS: Each cited theme corresponds to a distinct, testable need (not a
  vague aspiration).
- FAIL: A theme cited that maps to nothing testable in her Needs/pains.
- Where: `Users/SupportEngineer.md` Associated acceptance criteria.

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

#### PE4 — Acceptance criteria testable and reachable
- PASS: Cited themes each map to a distinct testable need. FAIL
  otherwise.
- Where: `Users/PlatformEngineer.md` Associated acceptance criteria.

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

#### DSL4 — Acceptance criteria testable and reachable
- PASS: Cited themes each map to a distinct testable need. FAIL
  otherwise.
- Where: `Users/DataScienceLead.md` Associated acceptance criteria.

### Customer (`CU`) — external persona

#### CU1 — Needs answered
- PASS: All 3 Needs map to a README element. FAIL otherwise.
- Where: README §3, §9; `Users/Customer.md` Needs.

#### CU2 — Pains addressed
- PASS: Both pains trace to resolving text. FAIL otherwise.
- Where: README §2, §9; `Users/Customer.md` Current pains.

#### CU3 — Day-in-the-work covered
- PASS: All 5 steps map into Flow 1, 5, or 6. FAIL otherwise.
- Where: README §9; `Users/Customer.md`.

#### CU4 — Acceptance criteria testable and reachable
- PASS: Cited themes each map to a distinct testable need. FAIL
  otherwise.
- Where: `Users/Customer.md` Associated acceptance criteria.

#### CU5 — External scoping and permissions stated   [CRITICAL]
- Checks: README states portal scoping, per-company data isolation, and
  what a Customer may and may not see.
- PASS: §8 (Customer block) and §9 (Flow 5) both state the status view is
  read-only and scoped to the Customer's own company only.
- FAIL: README lets a Customer's status view include another company's
  data, or never states the scoping at all.
- Where: README §8, §9.

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

#### IM4 — Acceptance criteria testable and reachable
- PASS: Cited themes each map to a distinct testable need. FAIL
  otherwise.
- Where: `Users/IncidentManager.md` Associated acceptance criteria.

---