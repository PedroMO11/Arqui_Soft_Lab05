# Sofia Reyes — Customer (external actor)

## Role
An external client of the company. She never uses Genius directly and never
talks to the LLM; she reports a problem to Support, and later checks whether
it has been resolved through a read-only status view scoped to her own
company's escalations. She is the origin of every Customer Escalation and
the reason its SLA exists.

## A day in their work
1. She hits a problem with the company's product and contacts Support.
2. Support Engineer Maria opens a Customer Escalation in Genius on her
   behalf; Sofia does not see Genius's internal tooling or the LLM at any
   point.
3. She waits, expecting a resolution or a meaningful update inside the
   Customer Escalation SLA (1 day).
4. She checks the status of her own reported problem through the read-only
   customer status view, scoped strictly to her own company's escalations.
5. If the status she sees does not match what Support told her minutes
   earlier, she loses confidence in the process and escalates again.

## Needs
- Needs a resolution or update within the 1-day Customer Escalation SLA, in
  order to trust that her business-impacting report was taken seriously.
- Needs the status she is shown to reflect the true current state of her
  escalation, in order to stop calling Support to double-check.
- Needs to see only her own company's escalations, in order to trust the
  company is not leaking other customers' incident data to her.

## Current pains
- "I was told my issue was still open, then found out it had been closed
  hours earlier" — the stale-status pain reported by Support on her behalf.
- Waiting past the SLA with no update, because Genius's internal delays are
  invisible to her until Support follows up.

## What the system gives them
- A Customer Escalation created and tracked against a fixed SLA from the
  moment she reports it.
- A read-only, per-company-scoped status view so she can check progress
  without calling Support.
- Confidence that the status she is shown is current, not stale.

## Associated acceptance criteria
R13 (SLA tracking on Customer Escalations, P1) · R3 (status freshness) ·
R12 (per-company data isolation on the status view). See `REDALE.md` §R.
