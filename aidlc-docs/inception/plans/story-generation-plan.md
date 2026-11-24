# Draw & Dash — Story generation plan

**Plan approval.** Group 22 approved this plan on November 24, 2026. Organize stories by player journey and map them to one casual-player persona across drawing, typing, speaking, combining inputs, and trying a prepared example.

**Inputs.** Use the [approved requirements](../requirements/requirements.md), the [current clarification record](../requirements/requirement-verification-questions.md), and the [stage assessment](user-stories-assessment.md). The product scope, one-human-player race, required commentary, budget, and data handling are already settled.

## Proposed method

Organize the stories in the order a player experiences a round. Give each story a stable ID, a persona, a player goal and its value, relevant FR/NFR IDs, and observable acceptance criteria. Use Given/When/Then where a trigger or change of state matters. For example, an in-race mute check needs an audible line already playing and a way to observe the pets continuing after speech stops.

Use one story per independently reviewable outcome. Keep input methods as scenarios within a common goal where the outcome is the same. Split them when the player expects a different result, such as preserving a supplied drawing or generating a sprite from a description. Keep system-wide constraints traceable without turning internal components into player roles.

| Part of the round | Coverage to carry forward |
|---|---|
| Creation | FR-01. Cover drawing, typing, speaking, combining inputs, and trying a prepared example. Include the stroke eraser, Clear Canvas, and editable voice transcript. |
| Review and confirmation | FR-02, FR-03. Cover correcting the interpretation, resolving conflicting details, and confirming a pet whose sprite preserves supplied artwork or is generated from its description. |
| Racing and commentary | FR-04, FR-05, FR-06, FR-07, FR-09, NFR-01, NFR-02. Cover the autonomous race, one timed cheer, sequential habits, and visible reactions. Connect the opening, routine lines, and live highlights to audible feedback, captions, mute, stale events, and unavailable device speech. |
| Results | FR-08, NFR-01, NFR-02. Show the engine’s official ranking at the finish and clear race chatter. |
| Recap | FR-08. Explain a meaningful moment using the event log and read the recap when sound is enabled. |
| Replay | FR-08, FR-09. Cover replay with the same confirmed pet, a fresh cheer, and cleared narration. Cover the restart path where a required request fails. |
| Constraints across stories | NFR-03, NFR-04, NFR-05, NFR-06. Trace local delivery, session privacy, provider and budget constraints, and verification strategy to the affected outcomes. Distinguish an acceptance scenario from evidence it has already passed. |

## Question 1

How should a teammate find and review a story in `stories.md`?

A) By player journey (recommended). Follow a round from creation through replay, with requirement IDs on each story. This makes handoffs such as finishing a race while a highlight is pending easy to review.

B) By feature. Group creation, simulation, and narration separately. Feature ownership is easy to see, but a whole round spans several groups.

C) By persona. Put each player archetype’s goals together. Shared actions such as cheering and muting may need cross-references.

D) By game domain. Use pet, race, and event concepts as the primary groups. Related rules stay together, but the player’s order of actions is less visible.

E) By epic. Group small stories under larger outcomes such as creating a recognizable racer. This gives an overview and adds a level of navigation.

X) Other (please describe after [Answer]: tag below). For a hybrid, identify the primary grouping and any secondary labels.

[Answer]: A) Player journey. Organize the stories around creation, review and confirmation, racing and commentary, results, recap, and replay.

## Question 2

How should `personas.md` represent the agreed casual-player audience?

A) One casual-player persona with distinct situations for sketching, describing a pet, and replaying it (recommended). This reflects one player being able to switch or combine input methods.

B) Two illustrative casual-player personas, one motivated by seeing their own artwork race and one by seeing an imagined creature take shape from a description. Both can use every input method. This makes the different expectations for artwork easier to compare.

X) Other (please describe after [Answer]: tag below)

[Answer]: A) One casual-player persona with several creation situations. Cover drawing, typing, speaking, combining inputs, and trying a prepared example as situations for the same primary player.

Map each story to this primary player. Use motivations, context, and needs grounded in the approved inputs. Mark illustrative details as such.

## Execution checklist

- [x] Assess the value of User Stories and read the approved requirements and revised clarification record.
- [x] Prepare the coverage outline, breakdown options, and planning questions.
- [x] Collect and review both answers for clarity and consistency with the approved scope.
- [x] Present the updated plan for team review.
- [x] Record explicit team approval of the plan.
- [x] Generate `aidlc-docs/inception/user-stories/personas.md` using the chosen framing.
- [x] Generate `aidlc-docs/inception/user-stories/stories.md` using the chosen organization.
  - [x] Give each story observable acceptance criteria and map it to the relevant persona and requirement IDs.
  - [x] Check the stories against INVEST: Independent, Negotiable, Valuable, Estimable, Small, and Testable.
  - [x] Check all nine functional and six quality requirements for coverage, including failure and replay scenarios. Separate required scope from optional features.
- [x] Present the [stories](../user-stories/stories.md) and [persona](../user-stories/personas.md) at the User Stories review gate.
- [x] Record the team's separate approval of the generated artifacts before Workflow Planning.

## Items carried forward

Later design work will specify the initial habit catalog, trigger ties, effect sizes and durations, cheering during a pause, thresholds for live highlights, freshness and finish cutoffs, and final upload and recording limits. Keep those values open when writing acceptance criteria.

Live checks will establish team-account model access, actual latency and charges, sprite wait time, microphone capture, available voices, and audible playback on the demo laptop. A possible switch to Luna for commentary depends on that evidence. The approved testing strategy uses mocks for routine work.
