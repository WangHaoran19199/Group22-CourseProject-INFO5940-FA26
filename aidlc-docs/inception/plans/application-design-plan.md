# Draw & Dash application design plan

**Status** — Group 22 approved this Application Design plan and selected 1A and 2A on November 24, 2026. The [execution plan](execution-plan.md) sets the stage sequence. The team separately approved the five generated design artifacts and authorized Units Generation.

## Design approach for review

Follow the player’s round from draft pet to replay. Start with ordinary TypeScript modules in the browser and one local Express server. The component names below describe proposed responsibilities. Units Generation will use the approved design to determine the units of work.

| Responsibility | Proposed handoff | What the design must preserve |
|---|---|---|
| Creation and review | The creation screen collects a sketch, typed words, an edited transcript, or a combination. A pet card carries the proposed appearance, allowed habits, and any conflict that requires an answer. Confirmation supplies the race with the accepted card and sprite. | A prepared example goes through the same review. A submitted drawing stays recognizable. Without a drawing, generate the static sprite from the confirmed appearance. A habit-only change can reuse approved artwork. US-01–US-05 |
| Race and display | The browser advances the engine with elapsed game time and player input. Each update returns the current positions and any newly recorded events in order. Canvas draws the positions and visible reactions. | One human pet and two computer pets share the fixed course and movement rules. Up to two habits run sequentially. Only one cheer can affect each race. The engine owns order and ranking. US-06–US-07, US-11 |
| Commentary and sound | A browser commentary module reads the engine’s events, selects prepared lines, and requests a live line for an eligible highlight. A returned line carries its race and source event. The module checks that the claim still holds before showing it and again before queued speech starts. | An overtake can reverse while the request is pending. Dropping the superseded line must leave the race running. A current routine call or selected live highlight can be heard mid-race. Mute clears speech while captions continue. US-08–US-10 |
| Results and session | The browser shows the official ranking at finish and requests the AI recap from the completed race log. Replay gives the confirmed pet a fresh race identity and cheer. Restart clears the failed session and its pending work. | Ranking appears while the recap is pending. The recap describes only recorded actions. An old race’s text or speech cannot enter a new round. US-11–US-14 |
| Local API and provider | The server validates submissions and model output and handles transcription, interpretation, sprite generation, and narration. Browser modules can use mock responses in routine tests. | Credentials stay on the server. Player media and generated content stay out of logs. The player sees which required operation failed and can restart. NFR-03–NFR-06 |

**Recommended event flow.** Pass engine updates directly to rendering and commentary. This lets a test supply two overtakes in order, hold the first commentary response, and release it after the second. The team can then check that the first line is discarded and the engine’s ranking still stands. A publish/subscribe layer would make it easier to add more consumers, at the cost of tracing event order across listeners. The fixed-course MVP already has two known consumers.

## Question 1

When a player corrects a proposed pet card, how should the review screen apply that correction?

A) Edit the card directly (recommended). Let the player edit the appearance description, choose or remove supported habits, and answer a focused conflict prompt. For the winged doodle, choosing “decorative wings” resolves flight while preserving the drawing. Validate the edited card before the player confirms it. The player can revise the original description and request a fresh interpretation when they want a different suggestion. Common edits are immediate and avoid another model call.

B) Send corrections back for interpretation. Let the player describe each change in their own words, send it with the draft and any clarification answer, and show the revised card for confirmation. This accommodates corrections that span several fields. Each revision adds a model round trip and another result to review. The player's explicit clarification still governs the outcome.

X) Other (please describe after [Answer]: tag below)

[Answer]: A) Apply edits and focused clarification answers directly to the draft. The player can explicitly request a fresh interpretation.

## Question 2

When Sam chooses a team-prepared example, what should the game load before the shared pet review screen?

A) Bundle the team’s source input and a matching, editable draft card (recommended). For example, open the snack-loving dog’s sketch and suggested habits immediately. The player still reviews and confirms it, and the race still uses AI commentary and recap. This gives rehearsals a predictable starting point and saves the initial interpretation call. The team must keep each bundled card consistent with its input.

B) Bundle the source input and interpret it on selection. Send the example through the same live interpretation request used for fresh input, then open the editable card. This demonstrates interpretation on every demo path. A prepared run takes another request and its first suggestion can vary.

X) Other (please describe after [Answer]: tag below)

[Answer]: A) Bundle the team’s source input and matching editable draft card. Selection opens the shared review screen without an interpretation call.

Both choices keep fresh drawing, text, and voice available independently and together. For either example path, changing the pet or settling a conflict still requires player confirmation. The approved artwork and replay rules apply to both.

## Execution checklist

- [x] Read the approved scope, requirements, stories, persona, technical environment, and execution plan.
- [x] Identify the components, data exchanged, and asynchronous calls that need design contracts.
- [x] Present the two open decisions together with recommendations and trade-offs.
- [x] Record the team’s answers and check both against the approved requirements. Resolve any material ambiguity and confirm the plan before generation.
- [x] Generate `components.md` with responsibilities, ownership of session and race state, and public interfaces.
- [x] Generate `component-methods.md` with method signatures, input and output types, and error outcomes for creation, racing, commentary, and replay.
- [x] Generate `services.md` with orchestration for confirming a pet, preparing a race, handling a live highlight, and finishing or restarting a round.
- [x] Generate `component-dependency.md` with a dependency matrix, communication directions, and data-flow diagrams with text equivalents.
- [x] Generate `application-design.md` consolidating the design and linking the four supporting files under `aidlc-docs/inception/application-design/`.
- [x] Cross-check the five artifacts against US-01 through US-14, FR-01 through FR-09, and NFR-01 through NFR-06. Walk through a multimodal conflict, a snack followed by a chase, a late highlight, mid-race mute, and a failed request followed by restart.
- [x] Present the [design artifacts](../application-design/application-design.md) for explicit team approval before Units Generation.
- [x] Record the team's decision on the generated design artifacts.

**Deferred details.** Functional Design can settle habit tie-breaks, effect sizes and durations, cheering during a pause, and numerical commentary thresholds. Final upload and recording limits and acceptable wait times belong to later design and rehearsal. Model access, latency, actual charges, microphone capture, and audible playback on the demo laptop still need live checks. The $20 budget, $5 reserve, mock-based testing, browser speech policy, and extension selections carry forward.

**Team response.** Both choices use the same editable card and explicit confirmation. They preserve FR-01 through FR-03 and the artwork reuse rule. The [audit record](../../audit.md#application-design--plan-approved) contains the team’s approval and requested stopping point.

[Approved requirements](../requirements/requirements.md) · [Clarification record](../requirements/requirement-verification-questions.md) · [Stories](../user-stories/stories.md) · [Persona](../user-stories/personas.md) · [Vision](../../../vision.md) · [Technical environment](../../../technical-environment.md) · [README](../../../README.md)
