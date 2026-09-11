# Carlos Mendez — Incident Manager (On-call Lead)

## Role
Oversees SLA compliance across all three escalation types and is the
administrator of Genius itself: he configures which tool actions the LLM may
take autonomously, reviews the audit trail after anything unusual happens,
and is accountable to the business when the first-week peak causes SLA
misses. He does not work individual incidents day to day; he watches the
system as a whole.

## A day in their work
1. He opens Genius's admin view each morning to check SLA compliance across
   Customer, Engineering, and Support Escalations from the previous day.
2. During the first week of the month, he watches load and error-rate
   indicators closely, since that is when Genius has historically degraded.
3. When Support or Engineering reports a bad LLM answer or an unexpected
   action, he pulls the audit trail for that incident to see exactly what
   the LLM was asked, what it did, and through which permission it acted.
4. Following the database-deletion incident, he periodically reviews and
   adjusts the permission boundary that decides which tool actions the LLM
   may execute autonomously versus which need human approval.
5. He reports reliability and SLA-compliance numbers to the business.

## Needs
- Needs a complete, per-incident audit trail of every tool action the LLM
  took and who triggered it, in order to investigate incidents like the
  database deletion after the fact.
- Needs to configure and adjust the permission boundary on autonomous LLM
  actions himself, in order to close a guardrail gap without waiting on
  Data Science or Engineering to ship a code change.
- Needs visibility into SLA compliance per escalation type, in order to
  report accurately on whether the 1-day and 3-day SLAs are being met.
- Needs the system's peak-week behavior to be predictable enough to report
  on, in order to set expectations with the business before the first week
  of each month.

## Current pains
- "We only found out what the LLM did after it had already deleted the
  database — there was nothing stopping it beforehand."
- No single place today to see whether SLAs are being met across all three
  escalation types.
- No visibility into the system during the first-week peak until Support or
  Engineering starts complaining.

## What the system gives them
- An administrable permission boundary for autonomous LLM tool actions.
- A queryable, per-incident audit trail of every action the LLM executed.
- An SLA-compliance view broken down by escalation type.
- Load and error-rate visibility during peak periods.

## Associated acceptance criteria
R10 (configurable permission boundary) · R9 (complete tool-action audit
trail) · R11 (SLA compliance visibility by escalation type) · R7, R8
(peak-load observability and load shedding). See `REDALE.md` §R.
