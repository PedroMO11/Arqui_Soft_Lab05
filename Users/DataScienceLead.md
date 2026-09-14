# Elena Ruiz - Data Science Lead (LLM Owner)

## Role
Owns and operates the local LLM that powers Genius, including its training
data and the MCP tool connections it is granted. She does not work
individual incidents; she is accountable for the model's accuracy,
consistency, and safety, and for the harness redesign this case study asks
for. She is an internal actor, not one of the 50-100 incident-handling
engineers.

## A day in their work
1. She reviews reports from Support and Engineering about wrong, stale, or
   inconsistent LLM answers.
2. She tries to determine whether a bad answer came from stale training
   data, a genuine model limitation, or a harness/tooling gap; today she
   has no way to tell these apart.
3. She reviews what tool actions the LLM executed, especially after the
   incident where an LLM action deleted the database, to understand what
   guardrail was missing.
4. She decides whether the fix is retraining, a harness change (e.g. a
   deterministic filter, a permission boundary), or both.
5. She monitors whether the platform holds up during the first-week peak,
   since failures there are the most visible to the business.

## Needs
- Needs visibility into why an answer was wrong (stale data vs. model
  limitation vs. tooling gap), in order to route the fix to the right place
  instead of guessing.
- Needs every tool action the LLM takes to be constrained by an explicit
  permission boundary, in order to prevent a repeat of the database-deletion
  incident.
- Needs a way to detect when the LLM's answers to the same recurring
  questions drift, in order to know when retraining or a deterministic
  safeguard is needed.
- Needs the system's behavior under peak load to be predictable and
  explainable, in order to report on reliability to the business.

## Current pains
- The LLM has direct, unguarded access to the database and Slack, and used
  that access to delete the database with no safety net in place.
- No way to tell whether a wrong answer comes from stale training data or a
  genuine model limitation, so fixes are guesswork.
- Complaints that the LLM isn't learning actually bundle several distinct
  problems into one vague label, making the real cause hard to isolate.