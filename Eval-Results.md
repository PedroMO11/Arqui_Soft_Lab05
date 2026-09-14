# Eval-Results

Evaluated against the current working-tree state of `README.md`, `REDALE.md`,
and `Users/*.md` (`SupportEngineer.md`, `PlatformEngineer.md`,
`DataScienceLead.md`, `IncidentManager.md` — `Users/Customer.md` no longer
exists, and `Eval-Spec.md` was updated to drop the Customer (`CU`) criteria,
`O10`, and the four `*4` acceptance-criteria criteria accordingly, since
none of the current persona files carry an "Associated acceptance criteria"
section).

## Iteration 1 — FAIL (4/9 overall, 1/1 remaining CRITICAL failing)

**Overall:** 4 / 9 (O-criteria) PASS, including 1 CRITICAL criterion (O3)
failing — automatic FAIL regardless of the numeric score.

### Overall criteria

| ID | Verdict | Evidence | Gap |
|---|---|---|---|
| O1 | **FAIL** | 9 sections present/non-empty. | §7 Decision 2 names the recurring-question mechanism "cache answer path"; §9 Flow 4 step 4 independently calls the same mechanism "the deterministic path." Neither name is defined in §5 — two different names for what appears to be one mechanism, and §5 defines neither. |
| O2 [CRITICAL] | **PASS** | All 5 problems in README §2 are present, each with a concrete failure description, and each traces in substance to a persona's Current pain: Problem 1 (DB deletion) ↔ SE pain 1 / DSL pain 1; Problem 2 ("not learning") ↔ DSL pain 3 / PE pain 2; Problem 3 (inconsistent recurring answers) ↔ SE pain 3 / PE pain 3; Problem 4 (peak-week failure) ↔ SE pain 4 / PE pain 1; Problem 5 (stale status) ↔ SE pain 2. | — |
| O3 [CRITICAL] | **FAIL** | All 6 objectives are present, each a clear single-sentence goal. | None of the 6 objectives cites a Problem number or names a persona, as the spec's PASS bar requires. Traceability is only recoverable by inference (Objective 1 ↔ Problem 1, etc.), never stated in the text. |
| O4 | **FAIL** | §4 lists exclusions as plain bullets. | Only 4 rows exist (spec expects 5), and none of the 4 states a "Why" — it is a bare list of what is excluded with no reasoning. |
| O5 | **FAIL** | §5 defines 15 terms, covering Incident, the 3 Escalation types, LLM, Harness, MCP, Tool action, SLA, SPOF, Peak period, Permission boundary, Recurring-question set, Audit trail, Load prioritization. | Same root cause as O1: "cache answer path" (§7) and "the deterministic path" (§9) are both used but neither is defined in §5. |
| O6 | **FAIL** | All 6 decisions are phrased as "do X instead of Y," so an alternative is named. | None of the 6 includes a "Rejected — because" clause tied to a stated problem — the reasoning for why the alternative lost is absent; only the bare choice remains. |
| O7 | PASS | All 5 personas have a claim block in §8; every claim names an observable condition (a time bound, a match, a named capability) — including the Platform Engineer's peak-week claim, phrased as a checkable relative comparison ("same responsiveness ... as a normal week"). | — |
| O8 | PASS | All 8 flows are numbered; nearly every step names a persona, "Genius," or a named §5 component. | Two steps are passive and name no actor directly (Flow 6 step 2, "Incident moves to a closed state"; Flow 7 step 1, "incident volume rises sharply") — both still reference named §5 terms (Incident, SLA, the peak-period definition) as their subject, so this is treated as a pass, but it is the weakest part of an otherwise clean criterion. |
| O9 [CRITICAL] | PASS | Nothing in §9 performs an action §4 excludes (Flow 5's Customer status view stays a status query, not a full portal); SLA figures (1 day / 3 days) agree between §5, §9 Flow 1/2, and §8; no §7 decision is contradicted by a later flow. | The §7/§9 naming drift (O1/O5) is a vocabulary problem, not an action or SLA contradiction, so it does not fail O9. |

### Per-persona criteria

| ID | Verdict | Evidence | Gap |
|---|---|---|---|
| SE1 | PASS | All 4 Needs map to a README element: permission boundary → Objective 1/Decision 1/Flow 4; status freshness → Objective 2/Decision 3/Flow 5; recurring consistency → Objective 3/Decision 2/Flow 4 step 4; peak responsiveness → Objective 4/Decision 4/Flow 7. | — |
| SE2 | PASS | All 4 pains trace to specific resolving text: DB deletion → Decision 1/Flow 4 steps 2-3; stale status → Decision 3/Flow 5 step 2; inconsistent answers → Decision 2/Flow 4 step 4; peak failure → Decision 4/Flow 7. | — |
| SE3 | PASS | Her 7-step day maps into Flows 1, 3, 4, 5, 6 (customer contact → Flow 1/3 step 1; checking history/asking LLM → Flow 4/5; self-resolution → Flow 6 steps 1-2; escalation creation → Flow 1/3; instructing the LLM → Flow 4 step 1; reporting back → Flow 6 step 3); the peak-week step is a repetition of steps 1-6, not a distinct activity. | — |
| PE1 | PASS | All 4 Needs map to README elements: peak availability → Objective 4/Decision 4/Flow 7; consistent mid-investigation answers → Objective 3/Decision 2; "I don't know" signal → Objective 5/Decision 5/Flow 4 step 4; audit trail → Objective 6/Decision 6/Flow 4 step 5. | — |
| PE2 | PASS | All 3 pains trace to resolving text: peak failure → Problem 4/Decision 4/Flow 7; hallucination → Problem 2/Decision 5; inconsistent answers → Problem 3/Decision 2/Flow 4 step 4. | — |
| PE3 | PASS | His 6-step day maps into Flows 2, 4, 6, 7 (escalation intake → Flow 2; history/diagnostics/iteration → Flow 4; resolution and closure → Flow 6; peak dependency → Flow 7). | — |
| DSL1 | PASS | 3 of 4 Needs map cleanly: root-cause visibility → Objective 5; permission boundary → Objective 1/Decision 1; predictable/explainable peak behavior → Objective 4/Objective 6. | "A way to detect drift" in the recurring-question set has no mechanism in §3/§7/§9 — only §8's claim block promises it, and this criterion's "Where" is limited to §3/§7/§9. Decision 2's deterministic routing *prevents* drift rather than *detecting* it. Accepted as a reasonable proxy, but it is a stretch. |
| DSL2 | PASS | All 3 pains trace to resolving text: unguarded DB/Slack access → Problem 1/Decision 1; can't distinguish stale-vs-model → Objective 5; "isn't learning" bundling several problems → Problem 2 (near-direct match: Problem 2 names the same three causes — outdated data, invented answers, wrong answers). | — |
| DSL3 | **FAIL** | 4 of 5 steps map cleanly into Flows 4, 7, 8 (reviewing reports → Flow 8 step 1; determining cause → Flow 8 step 3; reviewing tool actions → Flow 8 step 2; deciding the fix → Flow 8 step 3). | Step 5, "monitors whether the platform holds up during the first-week peak," has no corresponding actor in Flow 7 — Flow 7 step 3 names only the Incident Manager as monitoring load/SLA during peak week; the Data Science Lead is never mentioned in any peak-week flow step. |
| IM1 | PASS | All 4 Needs map to README elements: audit trail → Objective 6/Decision 6/Flow 4 step 5/Flow 8 step 2; adjustable permission boundary → Objective 1/Decision 1/Flow 8 step 3; predictable peak behavior → Objective 4/Decision 4. | "SLA-compliance visibility per escalation type" is only backed by Flow 7 step 3 in general terms ("monitors ... SLA compliance"), not broken out by escalation type until §8 — which is outside this criterion's §3/§7/§9 "Where." Accepted as a proxy, same as the prior iteration. |
| IM2 | PASS | All 3 pains trace to resolving text: found out after the fact → Problem 1/Decision 1 (boundary inserted before execution); no peak visibility → Decision 4 (capacity) plus Flow 7 step 3 (monitoring). | The "single place to see SLA compliance" and "peak visibility" claims live mainly in §8/§9 (Flow 7), not in §2/§7 as this criterion's "Where" column states — the same scope mismatch flagged in the prior iteration, now confirmed to persist unchanged. |
| IM3 | **FAIL** | 4 of 5 steps map cleanly into Flows 4, 7, 8 (SLA/load monitoring, audit-trail pulls, permission-boundary adjustment). | Step 5, "reports reliability and SLA-compliance numbers to the business," has no corresponding step in any Main Flow — Flow 7 ends at monitoring, Flow 8 ends at classifying/adjusting; neither includes a reporting-to-the-business step. |

---

## Summary

**Overall verdict: FAIL.** One CRITICAL overall criterion still fails (O3 —
objectives don't cite a Problem number or name a persona), and four
non-critical overall criteria fail (O1, O4, O5, O6), for an overall score of
4/9. Two per-persona criteria fail (DSL3, IM3).

Compared to the prior iteration, real progress was made on the criteria the
last two edits actually targeted: **O2 now passes** (problems trace to
persona pains without needing invented verbatim quotes), and the Customer
persona's removal is no longer a spec/reality mismatch since `Eval-Spec.md`
was updated to match. The remaining failures are unchanged, pre-existing
gaps in `README.md` itself — not new regressions from this session's edits.

### Corrections needed, by failing criterion

1. **O1 / O5 — undefined/duplicate term.** §7 Decision 2 says "cache answer
   path"; §9 Flow 4 step 4 says "the deterministic path." Pick one name,
   add it to §5 Key Product Concepts as a defined term, and use that single
   name in both §7 and §9.

2. **O3 [CRITICAL] — objectives don't cite their source.** Rewrite each of
   the 6 objectives in §3 to name the Problem number(s) it addresses and at
   least one persona by name, e.g. "Objective 1 (Problem 1; Support
   Engineer, Data Science Lead): every tool action the LLM can take is
   classified as autonomous or approval-required." This is the one CRITICAL
   failure keeping the overall verdict at FAIL — fixing it alone would clear
   the automatic-FAIL trigger, though O1/O4/O5/O6 would still hold the
   numeric score down.

3. **O4 — missing exclusion and missing reasons.** Add the fifth
   Out-of-Scope row that existed in earlier drafts (retraining/fine-tuning
   the LLM) and give each of the 5 rows a "Why" grounded in a fact from
   another section or a persona file, e.g. "Replacing the local LLM with an
   external/hosted model — Why: incident data cannot leave the company's
   infrastructure (§1)."

4. **O6 — decisions lack reasoning.** Add a "Rejected — because ..." clause
   to each of the 6 decisions in §7, tied to a stated problem, e.g.
   "Decision 1: insert a deterministic permission boundary ... instead of
   relying on the LLM's own judgment. Rejected — because trusting the LLM's
   judgment alone is exactly what let it delete the database in Problem 1."

5. **DSL3 — Flow 7 omits the Data Science Lead.** Add her as a named actor
   in Flow 7 (Peak-week operation) step 3, alongside the Incident Manager,
   e.g. "Incident Manager monitors load and SLA compliance; Data Science
   Lead monitors whether the model and harness hold up under peak load."

6. **IM3 — no reporting step in any flow.** Add a step to Flow 7 or Flow 8
   (or a new step) where the Incident Manager reports reliability and
   SLA-compliance numbers to the business — her "A day in their work" step
   5 currently has nothing in §9 to map onto.

None of this reflects a content problem with the underlying design — the
objectives, decisions, and flows are still logically sound and consistent
with each other (O7, O8, O9 all PASS, and O2 now passes cleanly). The gap is
that `README.md` still lacks the explicit citations, reasoning clauses, and
two missing flow steps that `Eval-Spec.md` checks for.

---

## Iteration 2 — NO CRITICAL FAILURES (7/9 overall, 12/12 per-persona)

**Overall:** 7 / 9 (O-criteria) PASS, 0 of 3 CRITICAL criteria failing.
2 non-critical overall criteria remain FAIL (O4, O6) — both by the user's
explicit choice this round, not an oversight. All 12 per-persona criteria
now PASS.

### Overall criteria

| ID | Verdict | Evidence | Gap |
|---|---|---|---|
| O1 | **PASS** | §7 Decision 2 and §9 Flow 4 step 4 both now say "cache answer path"; the term is defined in §5 Key Product Concepts. No other invented term found outside the §5 glossary. | — |
| O2 [CRITICAL] | PASS | Unchanged from Iteration 1 — all 5 problems still trace in substance to a persona's Current pain. | — |
| O3 [CRITICAL] | **PASS** | All 6 objectives now end with an explicit "— Addresses Problem N (persona, persona)" clause: Obj 1 → Problem 1 (Support Engineer, Incident Manager, Data Science Lead); Obj 2 → Problem 5 (Support Engineer, Customer); Obj 3 → Problem 3 (Support Engineer, Platform Engineer); Obj 4 → Problem 4 (Support Engineer, Platform Engineer, Incident Manager); Obj 5 → Problem 2 (Platform Engineer, Data Science Lead); Obj 6 → Problem 1 (Platform Engineer, Incident Manager, Data Science Lead). Every Problem 1–5 is attacked by at least one objective. | — |
| O4 | **FAIL** | All 4 existing rows now carry a "Why," stated in short, plain, direct language, each grounded in another README section (§1's data-residency constraint, §1's harness-only scope, §5's three-category Incident definition, §9 Flow 5's company-scoped status view). | Still only 4 rows; the spec's PASS bar reads "All 5 rows." The user explicitly chose to keep 4 rows rather than restore the missing "retraining/fine-tuning the LLM" exclusion, so this is a known, accepted gap rather than an oversight. |
| O5 | **PASS** | "Cache answer path" is now defined in §5; it was the only undefined term flagged in Iteration 1. No other §2–9 term is missing from §5. | — |
| O6 | **FAIL** | Unchanged — all 6 decisions are still bare "X instead of Y" statements. | The user explicitly declined this fix this round ("No agregas lo de Rejected"). No "Rejected — because" clause exists anywhere in §7. |
| O7 | PASS | Unchanged from Iteration 1. | — |
| O8 | PASS | Unchanged from Iteration 1 (same two passive, name-light steps in Flow 6/7, still treated as a pass). | — |
| O9 [CRITICAL] | PASS | The new Flow 7 steps (Data Science Lead added to step 3; new step 4 for Incident Manager reporting) don't perform anything §4 excludes and don't contradict any §7 decision; SLA figures still agree everywhere. | — |

### Per-persona criteria

| ID | Verdict | Evidence | Gap |
|---|---|---|---|
| SE1 | PASS | Unchanged from Iteration 1. | — |
| SE2 | PASS | Unchanged from Iteration 1. | — |
| SE3 | PASS | Unchanged from Iteration 1. | — |
| PE1 | PASS | Unchanged from Iteration 1. | — |
| PE2 | PASS | Unchanged from Iteration 1. | — |
| PE3 | PASS | Unchanged from Iteration 1. | — |
| DSL1 | PASS | Unchanged from Iteration 1 (same drift-detection stretch noted). | Same stretch as Iteration 1: "detect drift" is only promised in §8, not backed by a §3/§7/§9 mechanism. |
| DSL2 | PASS | Unchanged from Iteration 1. | — |
| DSL3 | **PASS** | Flow 7 step 3 now reads "Incident Manager monitors load and SLA compliance throughout the week; Data Science Lead monitors whether the model and harness hold up under the load" — this directly matches her day step 5 ("monitors whether the platform holds up during the first-week peak"). All 5 of her day steps now map cleanly. | — |
| IM1 | PASS | Same mapping as Iteration 1, now reinforced: the new Flow 7 step 4 (reporting SLA numbers to the business) gives his "SLA-compliance visibility" need a more direct anchor than the Iteration 1 proxy. | — |
| IM2 | PASS | Unchanged from Iteration 1. | Same Where-scope mismatch noted in Iteration 1 persists (resolving text lives in §9 Flow 7, not §2/§7 as the criterion's "Where" column states) — a spec-authoring quirk, not a README gap. |
| IM3 | **PASS** | Flow 7 now has a step 4: "Incident Manager reports reliability and SLA-compliance numbers to the business" — this directly matches his day step 5, word for word in substance. All 5 of his day steps now map cleanly. | — |

---

## Summary — Iteration 2

**Overall verdict: no CRITICAL failures.** All three CRITICAL criteria
(O2, O3, O9) now pass, and all 12 per-persona criteria pass. The two
remaining overall failures, O4 and O6, are both open by deliberate choice
this round rather than by omission:

- **O4** stays FAIL on a technicality: the spec's PASS bar names "5 rows,"
  and `README.md` still lists 4 Out-of-Scope items (the user chose to keep
  4 rather than restore a 5th). Every row that does exist now has a short,
  plain "Why."
- **O6** stays FAIL because the user explicitly declined to add
  "Rejected — because" reasoning clauses to §7's decisions this round.

If both are left as-is going forward, `Eval-Spec.md`'s O4 and O6 wording
should probably be revisited the same way O2/O10 were in the prior
session — either to accept 4 rows and a bare "why" as the real target, or
to accept "instead of Y" alone as sufficient alternative-naming without a
separate reasoning clause — so the spec stops flagging a gap the design has
knowingly settled on.
