---
category: taste/corpus
tags: [analysis, structured, evidence, thematic, demo]
last_updated: 2026-06-09
agent_action: "Neutral demonstration of the analytical register — source-forward, thematically structured, evidence-anchored, not preachy. Replace with your own when you adopt lib-me."
---

# Sample: Analytical Breakdown — Why Incidents Repeat (Thematic Study)

> Demonstration only. Shows the analytical register: numbered themes, each anchored
> to specific evidence, observational rather than prescriptive, ending with a
> proposed structure for further work.

---

## 1. The Cause Named in the Postmortem Is Rarely the Cause

- Most postmortems close on the proximate trigger: "a config change increased memory pressure." True, but it's the last domino, not the table.
- The deeper question — *why was a single config change able to take down the service?* — usually goes unasked, because the proximate cause is satisfying enough to stop the meeting.

## 2. Repeats Cluster Around Unowned Boundaries

- Incidents recur most where two systems meet and neither team fully owns the seam: the queue between service A and service B, the schema both read but only one writes.
- Evidence: in a backlog of repeat incidents, the modal location is an interface, not a component. Components have owners; interfaces have assumptions.

## 3. Action Items Decay Faster Than the Risk

- A postmortem produces action items. Within a quarter, the urgent ones ship and the structural ones — "add backpressure," "split the shared table" — slide.
- The risk doesn't slide with them. So the same class of incident returns, now with a postmortem that cites the *previous* postmortem's undone action item.

## 4. Measurement Changes Behavior, Naming Changes Measurement

- What gets counted gets managed. Teams that tag incidents by *contributing condition* (missing backpressure, unbounded retry, silent default) see the pattern; teams that tag by *service* see noise.
- The taxonomy is the intervention. You can't fix a category you never named.

---

## Proposed Structure for Further Work

1. **Inventory the seams.** List every interface where two teams share state; mark which side owns each assumption.
2. **Re-tag the incident history** by contributing condition rather than by service, and look for the modal condition.
3. **Track action-item decay** as its own metric — percentage of structural items still open at 90 days.
4. **Close the loop** by treating a repeat incident as a defect in the *previous* postmortem, not a fresh event.
