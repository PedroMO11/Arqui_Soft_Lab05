# Maria Torres - Support Engineer

## Role
Front-line staff who receive problem reports from Customers. She tries to
resolve them herself; when she cannot, she either forwards the report into
Genius as a Support Escalation, or opens a Customer Escalation directly when
the business impact is high. She is also the person who talks to the LLM
inside Genius to look up incident information and to ask it to take action
(a database query, a status check) on her behalf.

## A day in their work
1. A Customer contacts Support with a problem.
2. Maria checks Genius for related open incidents and recent history before
   answering the Customer.
3. If she can resolve it herself, she closes the loop with the Customer and
   logs the resolution.
4. If she cannot, she creates a Support Escalation (or, for a
   business-critical case, a Customer Escalation) and gives Genius's LLM
   instructions for what she needs: status information, a database lookup,
   or an action to run.
5. She waits for the LLM's response, expecting it within the Customer
   Escalation SLA (1 day) if the case is customer-facing.
6. She reports back to the Customer using what Genius told her.
7. During the first week of the month, she repeats this dozens of times a
   day, often asking the same handful of recurring questions.

## Needs
- Needs a clear boundary on what the LLM can do on its own versus what
  requires her explicit confirmation, in order to never again cause damage
  like a destructive database action from a routine escalation.
- Needs the incident status Genius shows her to reflect the current state,
  in order to give Customers accurate updates instead of stale ones.
- Needs the same recurring question to get the same answer every time she or
  a teammate asks it, in order to trust Genius enough to answer Customers
  without double-checking manually.
- Needs Genius to stay responsive during the first week of the month, in
  order to keep meeting the Customer Escalation SLA when incident volume is
  highest.

## Current pains
- Routine instructions given for a customer escalation led the LLM to
  delete the database with no confirmation step in between, the incident
  described in the case study.
- Genius shows an incident's status as open for hours after it has actually
  been closed, leading her to relay stale information to Customers.
- The same recurring question gets a different answer from Genius each time
  it's asked, forcing her to double-check before trusting it.
- During the first week of the month, Genius stops responding or answers
  incorrectly, right when Customer Escalation volume is highest.

## What the system gives them
- A single place to look up incident history and current status before
  responding to a Customer.
- A way to ask the LLM to perform actions (queries, checks) instead of doing
  them by hand.
- Guardrails so instructions she gives can no longer cause an unreviewed
  destructive action.
- Consistent, current answers to the recurring questions she asks daily.