# Unit of work dependencies

**Units Generation · approved by Group 22 on November 24, 2026.** Read each row as the caller or dependent and each column as the dependency. [U-01](unit-of-work.md) contains the browser game and its local Express server.

## Formal unit matrix

| Dependent unit | U-01 · Draw & Dash local web game |
|---|---|
| U-01 · Draw & Dash local web game | Same unit |

There are zero inter-unit dependencies. To coordinate work within U-01, use the seven direct calls below. Return values and completion callbacks follow the originating call. They do not add a reverse dependency. The directions and call modes follow the [approved component matrix](component-dependency.md).

## Internal handoffs

| Caller | Callee | Mode | Contract and integration check |
|---|---|---|---|
| UI | Session | Sync | Dispatch player intents, send elapsed race time and render the latest screen. A second cheer attempt and a mid-race mute both reach the active session. |
| Session | Pet | Sync and async | Load examples and apply card edits locally. Fresh interpretation and some artwork take time. Accept a confirmed card and sprite only if their draft still belongs to the current session. |
| Session | Engine | Sync | Pass confirmed racers, course, game time and timed inputs. Receive positions and ordered new events. Use the completed finish record as the sole source of official ranking. |
| Session | Commentary | Sync and async | Pass the race brief, new engine updates and final record. Captions and errors arrive through the commentary view. Advancing game time and publishing the finish order continue while text is pending. |
| Pet | API | Async | Transcribe a selected clip, interpret selected inputs, and generate artwork when needed. Selecting the prepared pet and making a direct habit edit bypass interpretation. |
| Commentary | API | Async | Prepare opening and routine lines, request selected live highlights, and generate the recap. Associate each response with the active operation and race. An overtake that has been reversed cannot be announced as the current lead. |
| Commentary | Speech | Sync dispatch | Offer one eligible line or cancel it. The later device completion callback lets Commentary reassess the pending highlight. Muting clears pending audio and leaves captions available. |

## External dependencies

| Boundary | What U-01 needs | Verification approach |
|---|---|---|
| Local Express API to OpenAI | Internet and authorized team-account access for interpretation, image generation, transcription, and commentary. Express keeps credentials server-side and validates inputs and results. | Use fixed provider responses during development. In a separately authorized rehearsal, check actual model access, quality, timing and charges. |
| Browser UI to input and display devices | Pointer and Canvas for drawing and animation, native controls for text, and a microphone for the voice creation path. | Exercise the browser journey with fixtures, then check permission, capture and track release on the demo laptop. |
| Browser Speech to the device voice | An available synthesis voice and enabled sound for audible calls. Captions and sound status remain available when the device lacks a usable voice. | Use a fake speech adapter to control completion and cancellation. Listen on the demo laptop during a running race. |

## Integration order

1. **Agree on the handoffs.** Use the approved [component methods](component-methods.md) for `ConfirmedPet`, `RaceUpdate`, and `FinishRecord`. A synthetic prepared dog and sample race events give each lead the same inputs to work with.
2. **Connect one complete round with fixtures.** Select the prepared dog, confirm it, run the autonomous race, show the official ranking and mocked recap, then replay. Confirm that the new race has a fresh cheer and event log while retaining the card and sprite.
3. **Exercise the other paths behind the same interfaces.** Creation can add fresh input, conflict resolution and sprite handling while commentary exercises live highlight responses and speech using ordered events. Delay a response across a second overtake and complete an old speech callback after mute. Check a required sprite or recap failure followed by restart.
4. **Rehearse the real boundaries later.** Use fresh and prepared pets on the target laptop, within the approved budget and reserve. Integration with mocks can establish request ordering and stale-result handling. Device and provider behavior require their own observations.

[Player-outcome story map](unit-of-work-story-map.md) · [Approved orchestration](services.md)
