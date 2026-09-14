## R - Requirements

### Requirements

| ID | Requirement |
|---|---|
| R1 | As a Support Engineer or Platform Engineer, when they give the LLM an instruction on an incident, the system must classify the requested tool action as autonomous or approval-required before it executes, so that a single instruction can never again trigger an unreviewed destructive action. |
| R2 | When a tool action is classified approval-required, it must block execution until the requesting engineer confirms it. |
| R3 | As any user requesting incident status, the system must return status read from, or validated against, the current incident state, never a stale or cached value. |
| R4 | As a Support Engineer or Platform Engineer, when their question matches the recurring-question set, the system must answer through the cache answer path, so that the same question returns the same answer regardless of when it is asked. |
| R6 | When the LLM does not have enough information to answer, it must label the answer as unknown instead of returning an unlabelled guess, so that a hallucinated answer is never indistinguishable from a verified one. |
| R7 | The system must scale out LLM-serving and tool-execution capacity ahead of the first week of each month, so that response availability during that peak week matches response availability during a normal week. |
| R8 | When request volume exceeds available capacity, the system must deprioritize lower-urgency requests rather than letting every request fail silently, so that Customer Escalations keep being served within their SLA even at the worst peak. |
| R9 | The system must log every tool action the LLM executes, including the actor, the action taken, the permission used, and the result, so it can be reviewed after the fact. |
| R11 | The system must show SLA compliance broken down by Customer, Engineering, and Support Escalations, so that the Incident Manager can report accurately on whether SLAs are being met. |
| R13 | The system must post a meaningful status update on a Customer Escalation within its SLA, so that a Customer is never left waiting with no update. |
| R14 | Every LLM answer the system returns must be labelled with its source, fresh data, a deterministic rule, or the model itself, so that the source can be identified easily. |
| R15 | The system must keep serving status reads even when the LLM inference component is unreachable, so that an LLM outage never blocks status visibility. |
| R17 | The system must remain available and tolerate the failure of any single component of the harness. |

### Parameters

| ID | Parameter | Value |
|---|---|---|
| P1 | SLA Customer Escalations | 1 day |
| P2 | SLA Engineering Escalations | 3 days |
| P3 | Incident volume | ~10,000 incidents/week; higher during the peak period (first week of each month) |
| P4 | Concurrent users | 50 to 100 engineers accessing the platform |