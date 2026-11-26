# Persona simulation

> [!NOTE]
> Sparrow paired OCEAN traits with Thought–Emotion–Action events to model a journey.<br>
> The success judge evaluated completion, and the UX audit looked for friction.

## What the simulation needed to explain

We used personas to explore how people with different tendencies might approach the same task. We wanted each journey step to show the persona's intent, the options on the current screen, and where the simulated experience got harder.

The persona needed to carry its goal and recent history through the journey so that repeated friction influenced later decisions.

## Architecture and end-to-end flow

The prototype initialized a persona biography and task alongside the screen route map. Each turn produced a Thought–Emotion–Action event. The simulator chose one of the screen's available actions, advanced through the route, and passed the trace and navigation history to the success judge and UX audit.

The summarizer assembled the verdict, the audit, and the journey for review.

## Traits and the TEA loop

OCEAN stands for openness, conscientiousness, extraversion, agreeableness, and neuroticism. The persona initializer filled in missing trait values and derived parameters for patience, exploration, and backtracking. The TEA engine put the biography and traits in the action prompt and used patience to decide when repeated friction should end a run.

The thought captures the simulated intention, the emotion records the modeled response, and the action identifies the behavior the runner attempts.

We carried trait scales into graph traversal and live persona testing. The live runner updated an internal emotional state after an action succeeded or failed. It linked that state to the action and the page observation, collected friction and failed intent matches, and included recommendations in its journey report.

The database contains 81 persona trace summary rows, including internal test runs.[^traces]

### Illustrative TEA trace

**Illustrative format, not from a real run.** The task is to add and complete a reminder to water a plant in a fictional task-list app. Each row preserves the main fields of a TEA event.

| screen | thought | emotion | action | outcome |
|---|---|---|---|---|
| Task list | We need a reminder to water the plant. | neutral | Select New task | The task form opens. |
| New task | A short title will make the reminder easy to spot. | confident | Enter “Water the plant” in Title | The form displays the title. |
| New task | The title is enough. We’re ready to save it. | confident | Select Save | The task appears in the active list. |
| Task list | We’ve watered the plant. We’ll mark the task done. | neutral | Select the task’s checkbox | The task leaves the active list. |
| Task list | We want to make sure it was recorded as done. | uncertain | Open Completed | The completed list shows the task with a checked box. |

## Task completion and UX friction

> [!IMPORTANT]
> We treated task completion and UX friction as separate questions.

| Option | Pros | Cons | Decision |
|---|---|---|---|
| Success judge | Examined the trace, final screen, and actions, then returned a verdict and supporting evidence. | Depended on the journey evidence. An empty journey gave an inconclusive verdict. | Used for task completion, with a heuristic fallback. |
| UX audit | Connected patterns such as backtracking and negative emotion labels to recommendations. | Emotion labels reflected modeled experience. | Used for friction review, with a heuristic fallback. |
| Explicit assertions | Checked the current address or text on a live page. | Required conditions defined for that task. | Accepted on live pages. |

## How a journey could break

An empty journey gave the success judge too little evidence, so it returned an inconclusive verdict. If the model selected an invalid action, the prototype recorded a fallback event. In the live loop, an unmatched intention triggered another attempt with different wording. After retries, the runner showed the model the extracted DOM choices.

That kept the run moving and gave the model more direct guidance than it received on the initial intent-only turn.

## Limitations and trade-offs

The persona produces modeled behavior. Emotion labels reflect trait coefficients and update rules, and the live runner estimates some progress from steps taken. Those signals help prioritize a journey for review. Changing the model or trait parameters can change the trace. Understanding how people feel requires direct observation.

Heuristic judgments also depend on how much information the route and the observations retain.

## What we'd do differently today

We'd record persona parameters, model settings, and the inputs available at each decision with the report. We'd compare runs with a neutral baseline and moderated usability sessions, then use the differences to decide where the simulation adds evidence. We'd keep a visible marker when the runner switches from intent matching to explicit action choices.

[^traces]: Count of data rows in `run_persona_teas`. Each row holds a run/persona summary rather than individual events.

[← Previous](screen-understanding-and-navigation.md) · [README](../README.md) · [Next →](autonomous-exploratory-testing.md)
