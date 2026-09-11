## R — Requirements

### Requirements

| ID | Requirement |
|---|---|
| R1 | As a Support Engineer or Platform Engineer, when they give the LLM an instruction on an incident, the system must classify the requested tool action as autonomous or approval-required before it executes, so that a single instruction can never again trigger an unreviewed destructive action. |
| R2 | As the system, when a tool action is classified approval-required, it must block execution until the requesting engineer confirms it, so that every destructive action has a human checkpoint. |
| R3 | As any user requesting incident status (Support Engineer, Platform Engineer, Incident Manager, or Customer), the system must return status read from, or validated against, the incident's current state — never a value cached from before the last state change — so that no one is told an already-closed incident is still open. |
| R4 | As a Support Engineer or Platform Engineer, when their question matches the recurring-question set, the system must answer through the deterministic answer path, so that the same question returns the same answer regardless of when it is asked. |
| R5 | The deterministic answer path must re-evaluate its answer against current incident data on every request, so that its answer changes if and only if the underlying facts changed. |
| R6 | As a Platform Engineer or Support Engineer, when the LLM does not have enough information to answer, it must respond "I don't know" instead of an unlabelled guess, so that a hallucinated answer is never indistinguishable from a verified one. |
| R7 | The system must scale out LLM-serving and tool-execution capacity ahead of the first week of each month, so that response availability during that peak week matches response availability during a normal week. |
| R8 | When request volume exceeds available capacity even after scale-out, the system must shed or deprioritize lower-urgency requests rather than letting every request degrade or fail silently, so that Customer Escalations keep being served within their SLA even at the worst peak. |
| R9 | The system must log every tool action the LLM executes — actor, action, permission classification, and result — to a queryable, append-only audit trail, so that any incident, including the database-deletion incident, can be reconstructed after the fact. |
| R10 | As an Incident Manager, the system must let them view and edit the permission-boundary classification of each tool action without a code deployment, so that a guardrail gap can be closed immediately after being found. |
| R11 | As an Incident Manager, the system must show SLA compliance broken down by Customer, Engineering, and Support Escalations, so that they can report accurately on whether SLAs are being met. |
| R12 | As a Customer, the system must let them view the status of only escalations belonging to their own company, so that no Customer can see another company's incident data. |
| R13 | The system must resolve or post a meaningful status update on every Customer Escalation within P1 and every Engineering Escalation within P2 of creation, so that both stated SLAs are met. |
| R14 | As a Data Science Lead, every LLM answer the system returns must be labelled with its source — deterministic answer path, fresh LLM inference, or "I don't know" — so that a wrong answer's root cause (stale data, model limitation, or harness gap) can be identified without guesswork. |
| R15 | The system must keep serving status reads and the deterministic answer path even when the LLM inference component is degraded or unreachable, so that an LLM outage does not also take down incident-status visibility. |
| R16 | Every database-write tool action the LLM triggers must be idempotent or de-duplicated at the harness layer, so that a retried instruction cannot execute the same destructive or duplicate write twice. |
| R17 | The system must remain available and tolerate the failure of any single component of the harness — not only the LLM-inference pool during peak week — so that no single point of failure ever takes down incident-status visibility, the deterministic answer path, or tool-action execution. |

### Parameters

| ID | Parameter | Value |
|---|---|---|
| P1 | SLA — Customer Escalations | 1 day |
| P2 | SLA — Engineering Escalations | 3 days |
| P3 | Incident volume | ~10,000 incidents/week; higher during the peak period (first week of each month) |
| P4 | Concurrent users | 50 to 100 engineers accessing the platform |

---

## E — Scale

Sizing the elastic behaviour that R7, R8, R13, and R15 require: how Genius-x
grows capacity ahead of the predictable peak, how it protects the SLA-bound
request classes once capacity is still exceeded, and how it keeps degrading
gracefully instead of failing outright. Figures below are **assumptions**
(no `Estimate` load numbers exist yet in this file to derive them from) and
are labelled as such; they must be replaced with derived numbers once `E —
Estimate` is written.

### Scaling triggers

| ID | Trigger | Response | Traces to |
|---|---|---|---|
| SC1 | Calendar-based: the first week of every month begins (**Peak period**) | Pre-scale LLM-serving and tool-execution capacity **before** the week starts, not reactively after load rises | R7 |
| SC2 | Request-queue depth or p95 latency crosses a configured threshold at any time (not only peak week) | Reactive horizontal scale-out of the same capacity pools, in addition to the calendar pre-scale | R7 |
| SC3 | Available capacity, after scale-out, is still exceeded by incoming request volume | Load shedding / prioritization engages (see below) instead of letting every request degrade | R8 |
| SC4 | The LLM-inference component is degraded or unreachable | Status reads and the deterministic answer path keep serving from their own capacity pool, independent of the LLM pool | R15 |

### Scale-out mechanics

- **Independent pools.** LLM-serving capacity, tool-execution capacity, and
  the deterministic-answer-path / status-read capacity scale as separate
  pools, so exhausting one (e.g. LLM inference under peak) cannot starve the
  others — this is what makes SC4 possible (R15).
- **Horizontal, not vertical.** Both pools scale by adding stateless
  instances behind a load balancer, not by resizing a single instance —
  required for the pre-scale in SC1 to be a capacity-add, not a redeploy.
- **N+1 redundancy** is maintained at every capacity level (normal week,
  peak week, and post-shed peak), so a single instance failure never removes
  the last unit of capacity for a pool.
- **Assumption (SC-A1):** pre-scale for the peak week targets a fixed
  multiplier over the trailing normal-week average load (e.g. peak factor
  applied uniformly across pools). The exact factor is an `Estimate`-derived
  parameter (`P#`), not invented here.

### Load shedding & prioritization (SC3)

| Priority | Request class | Rule |
|---|---|---|
| 1 (never shed) | Customer Escalation reads/writes and their SLA clock (1-day SLA) | Always served; shed lower tiers first to protect this tier's SLA (R8, R13) |
| 2 | Support Escalation activity | Shed only after tier 3 is fully shed |
| 3 | Engineering Escalation activity (3-day SLA — more slack to absorb delay) | First tier shed when capacity is exceeded |
| — (always on, own pool) | Status reads and the deterministic answer path | Not subject to shedding — served from the independent pool in SC4, since R3/R4/R15 require them regardless of LLM-pool load |

- Shedding is a **deprioritization** (queued, delayed, or answered with a
  "capacity constrained" signal) before it is an outright rejection — R8
  requires that lower-urgency requests are shed "rather than letting every
  request degrade or fail silently," not that they receive no response.
- Every shed or deprioritized request is still written to the audit trail
  (R9) with the reason, so an Incident Manager can see it happened.

### Degradation modes

| Failure | What keeps working | What is explicitly unavailable |
|---|---|---|
| LLM-inference pool degraded/unreachable | Status reads (R3), deterministic answer path (R4, R5), audit trail (R9) | Fresh LLM inference for non-recurring questions; those get an explicit "I don't know"-equivalent rather than a stale guess (R6) |
| Tool-execution pool at capacity | Status reads, deterministic answers, LLM Q&A without tool actions | New tool actions queue or shed per the priority table above, never execute unclassified (permission boundary from R1/R2 still applies once they run) |
| Both pools at capacity even after scale-out | Tier-1 (Customer Escalation) status and SLA clock | Tiers 2–3 shed per above |

### Monitoring & scale-back

- Incident Manager's SLA-compliance view (R11) and peak-load observability
  need are the operational signal that scale-out and shedding are working —
  a widening SLA-compliance gap during peak week is the trigger to revisit
  the SC-A1 pre-scale factor.
- Capacity scales back down after the peak week ends and the reactive
  triggers (SC2) fall back below threshold, so normal-week cost is not
  inflated by a permanently peak-sized deployment.