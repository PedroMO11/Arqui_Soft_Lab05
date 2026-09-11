# Genius-x — Domain Context Spec

## 1. Summary

Genius is the platform the company uses to track and resolve incidents:
Customer Escalations, Engineering Escalations, and Support Escalations. It
is built around a local LLM, reachable through an API, that both answers
questions about incidents and executes tool actions — database queries and
end-to-end tests — through MCP connectors. Because incident detail cannot be
made public, the LLM runs inside the company's own infrastructure rather
than calling an external model. Support Engineers, Platform Engineers, and
an Incident Manager rely on it daily; a Data Science Lead owns the model
itself. This document specifies Genius-x: a redesign of the harness around
that LLM, aimed at making it reliable and safe to operate at the company's
current incident volume.

## 2. Problem

The current harness — an API in front of the LLM, with MCP connectors
straight to the Database and to Slack and no layer in between — has failed
in five concrete ways:

1. **Unbounded destructive action.** A Support Engineer gave the LLM
   instructions for a Customer Escalation, and the LLM chose to delete the
   database. There was no permission boundary distinguishing an action the
   LLM may take on its own from one that needs human approval. As Incident
   Manager Carlos puts it: *"We only found out what the LLM did after it had
   already deleted the database — there was nothing stopping it
   beforehand."*
2. **Answers that don't reflect reality.** Users report the LLM "isn't
   learning" — in practice this is several distinct failures: answers built
   on outdated data, answers the model does not actually know but invents
   anyway, and answers that are simply wrong.
3. **Inconsistent answers to the same question.** A known, recurring set of
   common questions gets a different answer from the LLM depending on the
   day it is asked, even though the facts have not changed. Platform
   Engineer Diego: *"It answers the same question differently depending on
   when I ask it."*
4. **Degraded behavior under predictable peak load.** During the first week
   of every month — the highest-incident-volume period — the LLM tends to
   not respond, or responds incorrectly, more often than during normal
   weeks. Diego again: *"The LLM tends to not respond, or responds wrong,
   exactly during the first week of the month."*
5. **Stale status reads.** Support reports that Genius sometimes shows the
   status of an incident that was already closed hours earlier. Support
   Engineer Maria: *"Genius told me an incident's status when it had already
   been closed for hours."*

None of these are model-quality problems alone; each is a gap in the harness
around the model — the parts of Genius-x this document and its companions
are scoped to fix.

## 3. Objectives

1. **Bound the LLM's autonomous actions.** Every tool action the LLM can
   take is classified as autonomous or approval-required, closing the gap
   that let a single instruction delete the database (attacks Problem 1;
   needs of Maria, Elena, Carlos).
2. **Guarantee fresh incident status.** Any status Genius returns — to
   Support, to Engineering, or to a Customer — reflects the incident's
   current state, not a stale one (attacks Problem 5; needs of Maria,
   Diego, Sofia).
3. **Guarantee consistent answers to recurring questions.** The known set of
   common questions returns the same answer on repeated asking, absent an
   actual change in the underlying facts (attacks Problem 3; needs of
   Maria, Diego, Elena).
4. **Hold up under first-week peak load.** Genius stays available and within
   its stated latency and SLA targets during the highest-incident-volume
   week of the month (attacks Problem 4; needs of Diego, Carlos).
5. **Make wrong answers explainable and reducible.** Every answer is
   traceable to whether it came from fresh data, a deterministic rule, or
   the model itself — including the model saying it does not know rather
   than inventing an answer (attacks Problem 2; needs of Diego, Elena).
6. **Make every LLM action auditable.** Every tool action the LLM executes
   is logged with who triggered it, what it did, and under which permission,
   so it can be reviewed after the fact (attacks Problem 1; needs of Elena,
   Carlos).

## 4. Out of Scope

| Excluded | Why |
|---|---|
| Retraining or fine-tuning the LLM itself | This case study is scoped to Reliability Patterns and Fault Tolerance in the harness around the model, not to model training. A retraining pipeline is a Data Science concern that Genius-x's harness surfaces the need for (Objective 5) but does not itself build. |
| Replacing the local LLM with an external/hosted model | Incident detail cannot be made public; the local deployment is a hard constraint from the original design, not a decision this redesign revisits. |
| A general-purpose ticketing UI redesign | The Customer, Support Engineer, Platform Engineer, and Incident Manager workflows already exist around Genius; this spec changes the reliability of the LLM harness underneath them, not their UI. |
| New incident categories beyond Customer, Engineering, and Support Escalations | No pain or requirement in this document points at a fourth category; adding one would be scope invented, not scope needed. |
| A full self-service Customer support portal | Customers get a read-only, per-company-scoped status view (Objective 2, Main Flow 5) so they stop needing to call Support to check status — that is the full extent of direct Customer-facing surface this redesign adds. Anything beyond status visibility (e.g. customers filing escalations themselves) is a different product decision, not required by any stated pain. |

## 5. Key Product Concepts

Definitions below reuse `CONTEXT.md` exactly; see that file for the full
glossary.

- **Incident** — a reported problem Genius tracks from creation to
  resolution, always exactly one of the three categories below.
- **Customer Escalation** — an incident reported directly by a Customer;
  direct business impact; 1-day SLA.
- **Engineering Escalation** — an incident reported by Engineering, for
  itself or another internal area; 3-day SLA.
- **Support Escalation** — an incident originally reported by a Customer to
  Support, then forwarded by Support into Genius.
- **LLM (local)** — the in-house language model that answers questions and
  executes tool actions; never sends incident data outside the company.
- **Harness** — every component around the LLM (this redesign's subject)
  that gives it better performance, reliability, and usability.
- **MCP** — the protocol connecting the LLM to external tools; today, the
  Database and Slack.
- **Tool action** — anything the LLM executes through an MCP connector
  beyond a text answer: a database query, a database write, an end-to-end
  test run.
- **SLA** — the maximum time to respond to an incident: 1 day for Customer
  Escalations, 3 days for Engineering Escalations.
- **SPOF** — a component whose failure alone takes down the system or a
  critical capability; identified explicitly in `Architecture-Diagram.md`.
- **Peak period** — the first week of each month, where incident volume and
  the current system's failure rate both peak.
- **Permission boundary** — the classification of each tool action as
  autonomous or approval-required, introduced by this redesign to close
  Problem 1.
- **Recurring-question set** — the known, bounded set of common questions
  that Support and Engineering ask about incidents daily, called out in the
  case study as the ones whose answers must not drift day to day.
- **Deterministic answer path** — the answer route this redesign gives to
  the recurring-question set: a lookup against current incident state
  rather than an LLM inference, so the same question always returns the
  same answer while the facts are unchanged.
- **Audit trail** — the structured, queryable log of every tool action the
  LLM executes: actor, action, permission used, and result, introduced to
  replace unstructured Slack history as the record of what the LLM did.
- **Load shedding / prioritization** — the peak-week behavior of rejecting
  or deprioritizing lower-urgency requests once capacity is exceeded,
  instead of letting every request degrade silently.

## 6. Users and Their Needs

| Persona | Summary of needs | Details |
|---|---|---|
| Support Engineer | Safe LLM guardrails; fresh status; consistent recurring answers; availability at peak | [Users/SupportEngineer.md](Users/SupportEngineer.md) |
| Platform Engineer | Availability at peak; consistent answers mid-investigation; visible hallucination signal; audit trail | [Users/PlatformEngineer.md](Users/PlatformEngineer.md) |
| Data Science Lead | Root-cause visibility (stale data vs. model vs. tooling); enforced permission boundary; drift detection; predictable peak behavior | [Users/DataScienceLead.md](Users/DataScienceLead.md) |
| Customer (external) | SLA-bound resolution; fresh status; per-company data isolation | [Users/Customer.md](Users/Customer.md) |
| Incident Manager | Full audit trail; configurable permission boundary; SLA-compliance visibility; peak-load observability | [Users/IncidentManager.md](Users/IncidentManager.md) |

## 7. Key Product Decisions

1. **Insert a deterministic permission boundary between the LLM and every
   MCP connector, instead of relying on the LLM's own judgment.**
   Alternative considered: keep prompting the LLM to "be careful" with
   destructive actions. Rejected — this is exactly what was in place when
   the LLM deleted the database; a prompt-level instruction is not
   verifiable and already failed once.
2. **Route the known set of recurring common questions through a
   deterministic answer path, instead of always asking the LLM.**
   Alternative considered: rely solely on periodic retraining/fine-tuning to
   fix inconsistency. Rejected — retraining runs on a cadence measured in
   weeks at best, but Support and Engineering need the *same* answer to a
   *known* question every day in between; a deterministic path guarantees
   that independent of the training cycle.
3. **Require every status read to be served from, or invalidated against,
   the current incident state, instead of trusting a cached or
   remembered answer.** Alternative considered: let the LLM answer status
   questions directly from its own context. Rejected — this is exactly how
   the stale-status problem (Problem 5) happens; a status read must always
   reflect the entity's current state in the data model.
4. **Scale out horizontally ahead of the first-week peak, with load
   shedding/prioritization instead of a fixed-capacity deployment.**
   Alternative considered: keep a single fixed-size deployment and accept
   degraded performance during peaks. Rejected — the peak is predictable
   (same week every month) and the current fixed-capacity approach is the
   documented cause of Problem 4.
5. **Give the LLM an explicit "I don't know" path instead of forcing an
   answer.** Alternative considered: always return the model's best guess.
   Rejected — an unlabelled guess is indistinguishable from a correct
   answer to the person reading it, which is how hallucinated answers reach
   Customers and Engineers today.
6. **Log every tool action centrally (actor, action, permission used,
   result), instead of relying on Slack history as the audit trail.**
   Alternative considered: keep using the Slack MCP connector's message
   history for traceability. Rejected — Slack messages are not structured,
   not guaranteed complete, and were not enough to explain the
   database-deletion incident after the fact.

## 8. Expected User Experience

**Support Engineer**
- Can give the LLM an instruction for a Customer or Support Escalation and
  see, before any destructive action runs, whether it requires her approval.
- Sees an incident's status match its true current state, including recent
  closures.
- Gets the same answer to the same recurring question every time she asks
  it, on any day.

**Platform Engineer**
- Can reach Genius and get a response during the first week of the month at
  the same responsiveness Genius gives in a normal week, instead of the
  non-response or wrong-answer behavior seen today.
- Gets consistent answers to the same question while investigating a single
  Engineering Escalation.
- Sees an explicit "I don't know" instead of an invented answer when the LLM
  lacks the information.
- Can pull an audit trail of every tool action the LLM executed for a given
  incident.

**Data Science Lead**
- Can tell, for any flagged wrong answer, whether the cause was stale data,
  a model limitation, or a harness gap.
- Sees every autonomous tool action constrained by the permission boundary;
  no tool action bypasses it.
- Can detect drift in the recurring-question set before Support or
  Engineering reports it.

**Customer**
- Sees her Customer Escalation resolved or meaningfully updated within 1
  day.
- Sees a status that matches what Support tells her, with no lag.
- Sees only escalations belonging to her own company in the status view.

**Incident Manager**
- Sees SLA compliance broken down by Customer, Engineering, and Support
  Escalations from a single view.
- Can adjust which tool actions the LLM may take autonomously without a
  code change.
- Can pull the complete audit trail for any incident, including exactly
  what the LLM did and under which permission.

## 9. Main Flows

1. **Customer Escalation creation**
   1. Customer reports a problem to Support Engineer.
   2. Support Engineer creates a Customer Escalation in Genius.
   3. Genius starts the 1-day SLA clock.

2. **Engineering Escalation creation**
   1. Platform Engineer (or another internal area) files an Engineering
      Escalation directly in Genius.
   2. Genius starts the 3-day SLA clock.

3. **Support Escalation forwarding**
   1. Customer reports a problem to Support Engineer.
   2. Support Engineer attempts resolution directly.
   3. If unresolved, Support Engineer forwards it into Genius as a Support
      Escalation.

4. **LLM-assisted investigation**
   1. Support Engineer or Platform Engineer asks the LLM a question, or
      instructs it to take a tool action, against an open incident.
   2. The permission boundary classifies the requested tool action as
      autonomous or approval-required.
   3. If approval-required, the requesting engineer confirms before the
      action runs; if autonomous, it runs directly.
   4. If the question matches the known recurring-question set, Genius
      answers through the deterministic path; otherwise the LLM answers,
      labelling an answer "I don't know" when it lacks the information.
   5. Every tool action taken is logged to the audit trail with actor,
      action, permission used, and result.

5. **Status check**
   1. Support Engineer, Platform Engineer, Incident Manager, or Customer
      requests the current status of an incident.
   2. Genius returns status read from, or validated against, the current
      incident state — never a stale or cached value from before the last
      state change.
   3. Customer only ever sees escalations scoped to her own company.

6. **Resolution and closure**
   1. Support Engineer or Platform Engineer resolves the incident.
   2. Incident moves to a closed state within its SLA.
   3. Support Engineer reports the resolution back to the Customer, where
      applicable.

7. **Peak-week operation**
   1. In the first week of the month, incident volume rises sharply.
   2. Genius scales out and, if load still exceeds capacity, sheds or
      prioritizes lower-urgency requests rather than degrading silently.
   3. Incident Manager monitors load and SLA compliance throughout the week.

8. **Post-incident audit review**
   1. Incident Manager or Data Science Lead flags an incident for review
      (e.g. an unexpected or destructive action, a reported wrong answer).
   2. They pull the audit trail: every tool action, actor, permission used,
      and result for that incident.
   3. Data Science Lead classifies the root cause (stale data, model
      limitation, or harness gap); Incident Manager adjusts the permission
      boundary if the gap was there.
