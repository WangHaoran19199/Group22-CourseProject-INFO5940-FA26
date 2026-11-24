# Draw & Dash application design

**Approved by Group 22 on November 24, 2026 · AI-DLC v1.0.1 Inception.** The team approved the [design plan](../plans/application-design-plan.md), including direct edits to the pet card (1A) and bundled examples containing source input and an editable draft (2A). This design connects those decisions to a complete round for [Sam, the casual player](../user-stories/personas.md#p-01).

## A round from sketch to recap

Sam sketches a dog with wings and says it cannot fly. After Sam edits the transcript, the game interprets the selected inputs and asks about the conflict. Sam marks the wings as decorative, confirms the habits, and sees the original lines on the track. The same review screen also opens immediately when Sam selects the team’s prepared dog. A fresh pet described entirely through speech or text gets a static hand-drawn-style sprite after confirmation.

On one approximately 30-second course, Sam’s dog races two bundled computer pets. The dog can give up the lead for a snack and then chase the rival that passed it. All three pets obey the same movement rules. Sam can decide when to use one cheer or let the race finish without it. The engine emits the snack stop, pass, and subsequent burst in order. Canvas animates those facts. Commentary can describe them without deciding the outcome.

At the finish, Sam sees the engine’s ranking immediately. An AI recap can then explain where the dog lost a position, using the completed race log. Replay retains the card and sprite, grants one new cheer, and starts a fresh log.

## Structure and interfaces

| Owner | Data or decision it controls | Handoff |
|---|---|---|
| Player interface (UI) | Form, sketch strokes, recording lifecycle, dialogs, visible race and results | Sends player intents and game time to Session. Reads screen updates, animates static artwork and renders captions as text. |
| Game session (Session) | Session phase, active race ID and confirmed pet | Dispatches edit and race actions. Supplies the same ordered engine update to UI and Commentary. Publishes results before the recap completes. |
| Pet creation and review (Pet) | Draft revision, conflicts, validated card, and artwork source | Returns the confirmed card and sprite together. Existing artwork can be reused for replay and habit-only changes. |
| Race engine (Engine) | Timed inputs, sequential habits, movement, events, and finish order | Returns read-only snapshots, ordered new events and the completed race record. The same inputs and seed, if used, must reproduce the result. |
| Race commentary (Commentary) | Event-to-caption matching, live request selection, speech eligibility, recap | Prepares the opening and routine lines, requests selected highlights, and checks freshness before display and again before queued playback. |
| Browser speech (Speech) | Device voice availability and active utterance | Accepts a single line or cancellation and reports completion. Commentary keeps at most one pending highlight. |
| Local AI API (API) | Validation, provider credentials, model calls and safe errors | Six operations cover transcription, interpretation, sprite generation, opening and routine lines, live highlights, and recap. |

Session progresses through creation, review, race preparation, racing, and results. A required operation failure in the active session identifies the step and offers restart. Request results carry the identity of their session and draft revision or race. Restart invalidates that identity before returning to creation. Replay retires only the old race and preserves the confirmed pet. These identities keep a late completion from replacing a card, announcing a past overtake, or ending a new round.

## Realtime commentary

The opening and routine calls are generated before racing. A matching routine caption appears immediately and is spoken when the narrator is free. A live call is eligible only for a selected event that still describes the active race. It may be spoken during the race, even if a different routine line played earlier. Keep one live request in flight and suppress duplicate submissions. Keep at most one spoken line active and one pending highlight. These are separate limits. A newer or more important highlight can replace the pending one.

If Sam mutes while the dog dips toward the snack, active speech and queued audio stop immediately. Racers and captions continue. Unmute starts with the next eligible line. A missing device voice leaves sound status and captions visible. Finish clears in-race chatter before the recap is offered for speech.

## Traceability and review scenarios

The IDs below follow the [approved stories](../user-stories/stories.md) and [requirements](../requirements/requirements.md). Each row identifies where the team can inspect a design decision and what a later test should observe.

| Story | Outcome | Principal components | Requirements | Review scenario |
|---|---|---|---|---|
| US-01 | Create or select an example | UI, Pet | FR-01, NFR-03, NFR-04, NFR-06 | Erase one stroke, clear the canvas, and combine remaining inputs. Load a bundled source and editable draft into the common review screen. |
| US-02 | Speak and correct a transcript | UI, Pet, API | FR-01, NFR-04, NFR-05, NFR-06 | Record on demand, release tracks, edit a misheard word, and interpret the reviewed transcript alone or with other inputs. |
| US-03 | Review and resolve conflict | Pet, API | FR-02, FR-05, NFR-04, NFR-06 | Decorative wings stay visible. The resolved choice controls behavior. Direct corrections trigger no additional interpretation call. |
| US-04 | Keep original artwork | Pet, UI | FR-03, FR-05, NFR-06 | Draw a distinctive wing, then change a habit. The same sketch remains the animated static sprite. |
| US-05 | Generate a described racer | Pet, API | FR-01, FR-03, NFR-05, NFR-06 | Confirm a text-only or voice-only description. Generate a hand-drawn-style sprite from that appearance and reuse it on a habit-only change. |
| US-06 | See autonomous habits | Engine, UI | FR-04, FR-05, NFR-01, NFR-03, NFR-06 | Three pets use the same rules on the fixed course. Observe a snack pause, then a still-eligible chase. A pet with no habits also finishes. |
| US-07 | Cheer at most once | Session, Engine | FR-04, NFR-01, NFR-06 | Record the first valid timed cheer. Repeated input cannot grant another. A round with no cheer still reaches the result. |
| US-08 | Follow opening and routine calls | Commentary, API, Speech | FR-06, FR-07, NFR-02, NFR-05 | Prepare the opening and routine lines before racing. Display the snack line only on a matching logged event. |
| US-09 | Follow live highlights | Engine, Commentary, API | FR-06, NFR-01, NFR-02, NFR-05 | Hold an overtake call until the lead reverses. Discard the obsolete line while the race and cheer control continue. |
| US-10 | Hear and mute during the race | Commentary, Speech, UI | FR-06, FR-07, FR-09, NFR-02, NFR-06 | Hear the snack call while the dog dips. Mute mid-line and verify racers and text continue. With no voice, show captions and sound status. |
| US-11 | See the official result | Engine, Session, UI | FR-04, FR-08, NFR-01, NFR-02, NFR-06 | Show the three finishers in engine order immediately. A pending live response or recap cannot change or delay the ranking. |
| US-12 | Understand the recap | Commentary, API, Speech | FR-07, FR-08, NFR-01, NFR-05 | Use the final log to explain a recorded pass during the pause. Display the recap and speak it if enabled and available. |
| US-13 | Replay the confirmed pet | Session, Engine, Commentary | FR-03, FR-04, FR-07, FR-08, NFR-02, NFR-05, NFR-06 | Retain card and sprite. A new race identity starts with an empty log and a fresh cheer. Old text and speech are ineligible. |
| US-14 | Restart after failure | Session, Pet, Commentary, API | FR-09, NFR-02, NFR-04, NFR-06 | Fail a required sprite request. Identify the step, restart, select the example, and discard any late completion from the old session. |

## Local runtime, cost, and verification

The selected runtime is vanilla TypeScript with Vite and HTML5 Canvas in the browser, backed by Node.js, npm, and a loopback Express server. Show which inputs will be sent to OpenAI before submission, including a clip before transcription. Keep API credentials server-side, enforce media types and limits, validate model data, and display generated text as plain text. Keep submissions in session memory. Log usage counts and error categories only as needed, excluding player content and generated output. Request `store: false` for Responses calls. Local cleanup and provider retention have separate scopes.

The $20 budget includes $5 reserved for rehearsal and the demo. Bundle the computer racers, skip initial interpretation for prepared examples, generate a sprite only when needed, and reuse it for the same confirmed appearance. Use mocks for routine development. Vitest examples and partial fast-check properties cover the pure engine and data transformations. Playwright exercises the player journeys with mocked provider and speech boundaries. Tests of model access, latency, actual spend, microphone capture and audible speech on the demo laptop are still pending. Resiliency and Security Baselines are off. The approved privacy and error requirements apply.

**Deferred to later design or rehearsal.** Tune habit tie-breaks, effect sizes and durations, cheer timing during a pause, highlight selection and freshness, the finish cutoff, and input limits. The approved budget and speech behavior already constrain these choices. Extra courses and same-device multiplayer remain optional after the core demo. Online multiplayer, accounts, a pet database, persistent care or training, an AI race director, hosted speech generation, and per-pet skeletal or video animation are outside the MVP.

## Supporting artifacts

- [Components](components.md) defines responsibilities, state ownership, and public boundaries.
- [Component methods](component-methods.md) specifies data types, inputs, outputs, errors, and six local API operations.
- [Services](services.md) traces confirmation, race preparation, commentary, results, replay, and failure recovery.
- [Component dependency](component-dependency.md) provides the call matrix, two data-flow diagrams, and text equivalents.

Group 22 approved the five design artifacts and authorized Units Generation on November 24, 2026. See the [approval record](../../audit.md#application-design--artifact-approval).
