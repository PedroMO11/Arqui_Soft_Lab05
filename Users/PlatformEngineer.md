# Diego Fernández - Platform Engineer

## Role
One of the 50 to 100 engineers who use Genius day to day for Engineering
Escalations, incidents his own team or another internal team files against
platform systems. He relies on Genius's LLM to pull data, run checks, and
summarize incident history while he investigates. He is a heavy user during
normal weeks and the person most affected when Genius degrades under load.

## A day in their work
1. An Engineering Escalation lands in his queue, either self-filed or
   assigned from another team.
2. He asks Genius for the incident's history and related past incidents to
   understand whether this is a recurring issue.
3. He asks Genius to run diagnostic queries or end-to-end tests through the
   LLM's tool actions instead of doing it by hand.
4. He iterates: he asks the same kind of question multiple times while
   narrowing down the cause, expecting consistent answers each time.
5. He resolves the incident within the 3-day Engineering Escalation SLA and
   closes it in Genius.
6. In the first week of the month, incident volume spikes and he depends on
   Genius the most, exactly when it has been least reliable.

## Needs
- Needs Genius to stay available and responsive during the first-week peak,
  in order to keep working through a larger incident queue without losing
  time to a stalled or unresponsive LLM.
- Needs the LLM's answers to a repeated question to stay consistent while
  investigating a single incident, in order to trust his own narrowing-down
  process instead of chasing noise.
- Needs a clear signal when the LLM does not actually know an answer, in
  order to avoid acting on a hallucinated result.
- Needs an audit trail of what tool actions the LLM executed on his behalf,
  in order to explain what happened if a fix does not work as expected.

## Current pains
- During the first week of the month, the highest-incident-volume period,
  the LLM frequently fails to respond or responds incorrectly.
- The LLM hallucinates: when it doesn't actually know an answer, it
  fabricates one instead of saying so.
- The LLM gives inconsistent answers to the same question depending on when
  it's asked, undermining the narrowing-down process during an
  investigation.

## What the system gives them
- On-demand incident history and tool-driven diagnostics through the LLM.
- Stable, repeatable answers for the same question within an investigation.
- Predictable availability during peak weeks, matched to the 3-day
  Engineering Escalation SLA.
- A record of every action the LLM took, for after-the-fact review.