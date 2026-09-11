# CONTEXT.md — Genius-x

Domain context and glossary for Case Study #5 (Reliability Patterns / Fault
Tolerance). This file is the frozen input for every other document in this
repo. It is written once, from the assignment brief, and is not edited to
make any later evaluation pass.

---

## 1. Domain overview

**Genius** is the internal software a company uses to manage the incidents
it handles every day. An **incident** is a reported problem that needs
tracking, investigation, and resolution.

To keep incident detail private, the Data Science area deployed a **local
LLM** (it does not call an external API, so incident data never leaves the
company). The Incident Management area was given an **API** to that LLM so
it can both retrieve information and take actions on the company's behalf —
running database queries, running end-to-end tests, and similar operations.

The current version, **Genius-x**, is a redesign triggered by two incidents
with the LLM itself (see §4). The assignment asks for a new **harness**
around that LLM, plus an architecture diagram that shows how reliability and
fault tolerance are achieved.

---

## 2. Actors

| Actor | Description |
|---|---|
| **Customer** | An external client of the company. Reports problems that affect them directly; does not use Genius directly. |
| **Support Engineer** | Front-line staff who receive reports from customers, try to resolve them, and escalate to Genius when they cannot. |
| **Engineering** (Software/Platform Engineer) | Internal staff who file or receive incidents about the company's own systems. Also the primary user base of the Genius platform (50 to 100 people accessing it). |
| **Data Science area** | Owns and operates the local LLM that powers Genius. Responsible for its training data and behavior, not for the day-to-day incident workflow. |
| **Genius (system)** | The incident-management platform: the API, the LLM, and everything the harness adds around it. |

---

## 3. Key product concepts (glossary)

| Term | Definition |
|---|---|
| **Incident** | A reported problem that Genius tracks from creation to resolution. Every incident belongs to exactly one of the three categories below. |
| **Customer Escalation** | An incident reported directly by a customer. It has direct business impact and the tightest response SLA. |
| **Engineering Escalation** | An incident reported by the Engineering area, for itself or for another internal area. |
| **Support Escalation** | An incident originally reported by a customer to the Support area, then forwarded ("processed") by Support into Genius. |
| **LLM (local)** | The language model that powers Genius. Runs inside the company's own infrastructure so incident data is never sent to a third party. It both answers questions about incidents and executes actions through tools. |
| **Harness** | Every component built around the LLM to improve its performance, reliability, and usability — everything except the model itself. The assignment's core deliverable is redesigning this harness. |
| **MCP (Model Context Protocol)** | The mechanism the current harness uses to connect the LLM to external tools. Today there are two MCP connectors: one to the incident **Database** and one to **Slack**. |
| **Tool action** | Anything the LLM can execute through an MCP connector beyond answering in text — for example, a database query, a database write, or an end-to-end test run. |
| **SLA (Service Level Agreement)** | The maximum time Genius has to respond to an incident, measured from report to response. Differs by incident type (§5). |
| **SPOF (Single Point of Failure)** | A component whose failure alone would take down the whole system or a critical capability. The assignment explicitly asks for these to be identified in the new design. |
| **Peak period** | The first week of each month, when incident volume is highest and the current system's failure rate is also highest (see §4). |

---

## 4. Problems observed with the current system

These are the concrete failures the redesign must address. They come
directly from the assignment brief and are the raw material for the
Problems-to-solve section of later documents.

1. **Destructive action without authorization boundaries.** A Support
   Engineer gave the LLM instructions for a customer escalation, and the LLM
   chose to delete the database. The LLM had no defined limits on which
   actions it may take autonomously versus which require approval.
2. **No learning / stale answers.** Users report the LLM "does not learn":
   it repeatedly gives wrong answers or answers based on outdated data.
3. **Inconsistent answers to the same question.** For a known, recurring set
   of common questions, the LLM gives a different answer each day, even
   though the underlying facts have not changed.
4. **Degraded behavior under peak load.** During the first week of each
   month — the highest-incident-volume period — the LLM tends to not
   respond at all, or responds incorrectly, more often than during normal
   periods.
5. **Stale status reads.** Support reports that Genius sometimes answers
   with the status of an incident that was already closed hours earlier —
   the answer does not reflect the current state of the incident.

---

## 5. Non-functional requirements (as given)

| Parameter | Value |
|---|---|
| Incident volume | ~10,000 incidents per week, more during peaks |
| Concurrent users | 50 to 100 engineers accessing the platform |
| SLA — Customer Escalations | 1 day |
| SLA — Engineering Escalations | 3 days |
| Availability | Must be highly available and fault-tolerant |
| Latency | Real-time / near-real-time for critical decisions — minimal latency where a decision affects an incident directly |

No SLA is stated in the brief for Support Escalations; treat this as an open
point to resolve in the requirements document, not as an assumption to
silently fill in here.

---

## 6. Current architecture (before the redesign)

The existing harness is minimal: an API sits in front of the LLM, and the
LLM reaches two external systems through MCP connectors, with no
reliability layer in between.

```
API -> LLM -> MCP Database -> Database
           -> MCP Slack    -> Slack
```

This is the architecture the case study says is insufficient — it has no
guardrails on what actions the LLM may take, no mechanism to keep answers
consistent or current, and no stated behavior under peak load.

---

## 7. Scope of the assignment

**In scope (what must be produced):**
- A redesigned harness for the LLM — the components around the model that
  give it better performance, reliability, and user experience.
- An architecture diagram that shows reliability and fault-tolerance
  patterns applied to this system.
- Identification of SPOFs and high-risk components in the current and
  proposed design.

**Explicitly named background for this design:** Reliability Patterns and
Fault Tolerance (the two topics this case study is scored on, 20 points
total).

**Not specified by the brief** (open points for the requirements document
to settle, not to be assumed here): the exact retraining/fine-tuning cadence
for the LLM, the specific authorization model for autonomous actions, and
the Support Escalation SLA.
