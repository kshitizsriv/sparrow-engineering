# Why we pivoted and what we learned

> [!NOTE]
> The team moved from simulated-user feedback toward exploratory testing of live apps.<br>
> Replayable observations made the evidence easier to verify, while flaky runs remained a limit.

## Context

We started by asking how a person with a particular goal and set of tendencies would move through a design. The prototype took screen images and built a navigable map.

It generated a persona trace that described what the persona noticed, what it tried, and where it encountered friction. A success judge assessed the task. A UX audit turned the trace into issues for review.

## What simulation taught us

The route map was the environment we gave the persona. If a route omitted a control or pointed to the wrong screen, the persona's behavior reflected that map. That made screen understanding and action grounding part of the evaluation problem. A detailed explanation of a choice still depended on whether the simulator presented the right options.

TEA gave us a structure for examining those choices. A reviewer could look at the simulated intention alongside the resulting action and the modeled emotion. The trace helped identify where to look next.

The task judge answered a narrower question about whether the journey reached its goal, and the audit used the journey to identify possible points of friction.

## Why exploratory testing

By November 2025, our conversations with early-stage and enterprise teams showed why simulated-user feedback was weak at both ends of the market. Very early-stage teams changed their flows every week and could talk to real users directly, so a simulated opinion added little. Large enterprises already had research teams and participant panels.

They needed a validated method before trusting synthetic feedback.

Our own data pointed the same way. Model-generated satisfaction scores clustered near the top of the scale, so they rarely separated a good flow from a bad one.[^csat]

> [!IMPORTANT]
> Exploratory testing produced evidence that was easier to trust because we could replay a broken transition and see whether it reproduced.

Exploratory testing also reused most of what we had built, including the browser agents, DOM grounding, and the state graph.

## What changed technically

We added a Playwright extractor and grounded model-chosen intentions in the controls it returned. The persona runner still began with a goal and recorded a journey. The explorer added a coordinator, a shared queue, and run-scoped state identifiers so separate browser agents could contribute to one graph.

It tracked long-running operations and recorded visible and browser errors alongside state observations. The UI exposed the resulting flow from each exploration run for inspection.

The report changed with the execution model. Design simulation produced a task verdict and a UX audit from a generated journey. Live persona runs added page assertions. Exploratory runs added counts of observed states and transitions, and the system had visual test entities for tracking cases and issues.

Each output needed enough context for someone to evaluate the claim it made.

Exploratory testing hit its own limits. Dynamic pages, timing, and selector drift made runs flaky. The QA tooling market was crowded, which made it hard to establish a clear advantage. We wound Sparrow down in March 2026.

## What we'd do differently

We'd decide what evidence the report needs before expanding the exploration loop. For UX simulation, we'd test whether a persona adds a useful signal against a neutral baseline and usability sessions. For functional testing, we'd evaluate replay success and confirmed defects alongside the run record.

We'd also document the decision to change direction when we make it, while the constraints and alternatives are easy to reconstruct.

[^csat]: Median model-generated CSAT of 4.96 out of 5 across 31 stored insight records, including internal test runs.

[← Previous](agent-execution-and-evaluation.md) · [README](../README.md) · [Next →](../README.md)
