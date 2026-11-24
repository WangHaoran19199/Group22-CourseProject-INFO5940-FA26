# Draw & Dash unit of work

**U-01 · Draw & Dash local web game · approved by Group 22 on November 24, 2026.** The browser and its loopback Express server form one formal unit. Its deliverable for December 16 is a playable round on the demo laptop, from creating or selecting a pet to understanding the result and replaying. The unit owns all 14 stories for [Sam, the casual player](../user-stories/personas.md#p-01). [Approved unit plan](../plans/unit-of-work-plan.md)

## Responsibility and ownership

Each story has one lead area responsible for demonstrating its player outcome. The approved components can contribute to several areas. Group 22 will assign individuals according to strengths and tasks. The [story map](unit-of-work-story-map.md) identifies the lead and principal contributors for every story.

| Lead area | Stories | Responsibility at the handoff |
|---|---|---|
| Pet creation and review | US-01–US-05 | Deliver a confirmed card and its matching sprite to Session. Fresh drawing, text and voice work independently or together. A bundled example opens with editable source and draft. Direct edits and conflict answers take effect before confirmation. Use the submitted drawing when present. Otherwise generate a hand-drawn-style sprite from confirmed appearance. |
| Race and session | US-06, US-07, US-11, US-13, US-14 | Run one human pet and two bundled rivals on one fixed course for about 30 seconds. Engine applies zero to two habits sequentially, admits one timed cheer, and determines event and finish order. Session publishes the ranking immediately and manages replay and restart. |
| Commentary and speech | US-08–US-10, US-12 | Generate the opening and routine lines before the race. Match each routine caption to its event and selected live captions to current race facts. Coordinate browser speech, mute, and the required post-race analysis from the completed log. |

**Module ownership.** UI owns the controls, microphone capture, strokes and Canvas display. Pet owns the revision under review. Session owns the confirmed pet, phase and active race identity. Engine owns simulation state, log and rankings. Commentary owns published text and the pending highlight. Speech owns the current utterance. API owns HTTP validation and server-only provider access. The [dependency matrix](unit-of-work-dependency.md) shows who calls whom.

## Greenfield code organization

Use one npm project at the repository root. The paths below describe the intended Construction layout.

| Location | Contents and boundary |
|---|---|
| `src/browser/` | Vanilla TypeScript and Vite entry point. Group UI, Session, Pet, Engine, Commentary and Speech by module. Place the local API client here and bundle the team-authored demo examples and opponents with the browser. |
| `src/server/` | Loopback Express entry point, request validation, and OpenAI adapters for the six [local API operations](component-methods.md#api). Read credentials from the server environment. |
| `src/shared/` | Browser/server request and result types and shared domain contracts for confirmed pets, race events and finish records. Keep DOM, microphone, credentials and provider clients in their owning modules. |
| `tests/` | Vitest and fast-check checks for pure rules and transformations, mock API and speech fixtures, and Playwright journeys. Use synthetic inputs for tests. Keep user submissions in session memory. |
| Repository root | npm manifest and lockfile, TypeScript, Vite and test settings. Additional project configuration can live in `config/` as needed. AI-DLC records live in `aidlc-docs/`. |

## Verification scope for Construction

- **Creation.** Reach review with each input alone, all inputs together, and a prepared example. Resolve decorative wings, confirm zero to two supported habits, and verify original artwork or generated sprite reuse after a habit-only edit.
- **Race and narration.** Exercise a snack pause followed by an eligible chase, a second cheer attempt, two overtakes during one live request, and finish while commentary is pending. Compare event order and ranking under identical timed inputs and seed where applicable.
- **Lifecycle.** Hear a snack call mid-race, mute while the racers keep moving, and follow the captions. Replay with the confirmed sprite and a fresh cheer. Simulate a required API failure, restart, and ensure an old response cannot enter the new session.
- **Privacy and cost.** Disclose selected inputs before submission, enforce server-side validation and input limits, stop microphone tracks, and keep credentials and raw or generated player content out of logs. Routine checks use mocks. Kelan will verify team-account access, cost and latency during authorized live checks. The $20 budget includes $5 for rehearsal and the demo.

Exact habit parameters, highlight thresholds, playback cutoffs and final upload limits remain open for later design. Microphone capture, audible playback, sprite wait times and provider costs on the demo setup await live evidence. The [approved requirements](../requirements/requirements.md) define the MVP boundary, including deferred courses and multiplayer.
