# Services and orchestration

**Application Design · approved by Group 22 on November 24, 2026.** The services below are sequences of calls among the [components](components.md). Their [method contracts](component-methods.md) can run against mocks in routine development. Only the local API is a separate server process.

## Create and confirm a pet

**Owner — Pet, with Session coordinating the screen.** UI accepts any one or any combination of drawing, typed text, and spoken input. The stroke eraser removes one stroke. Clear Canvas removes all strokes while preserving other inputs. A voice clip is disclosed and transcribed through API. UI releases the tracks and lets the player correct the transcript. Only the reviewed words enter the interpretation request.

1. For fresh input, send the selected drawing, text, and edited transcript through `interpret`. For a prepared example, `loadExample` copies both the bundled source and its matching draft directly into the same review screen (2A). Selection costs no interpretation call.
2. Show appearance, supported habits and unresolved conflicts. A winged drawing paired with “cannot fly” produces a focused question. Choosing decorative wings preserves the drawing and rules out flight behavior. `edit` applies the answer or field changes locally (1A). Only the player’s explicit request for reinterpretation starts a new interpretation call.
3. On confirmation, validate the current revision, answered conflicts and zero to two allowed habits. A supplied drawing is the racing sprite. Without a drawing, reuse a previously approved sprite if its appearance still matches, or generate a simple static hand-drawn sprite from the confirmed appearance. A habit-only edit keeps that artwork.
4. Publish the confirmed pet only when both the card and the matching artwork are ready. Ignore an interpretation, transcript or sprite that completes after its draft was superseded. Double-clicking confirm for the same revision shares the pending operation.

**Review example.** Select the prepared snack dog, remove its chase habit, and confirm. The original sketch still races. The model-call record for this example has neither an interpretation call on selection nor an image call on confirmation. The race still uses generated introductions, commentary, and recap.

## Prepare and run a race

**Owner — Session.** Allocate a new race ID, assemble the confirmed pet, the two bundled opponents and the fixed course, and ask Commentary to `prepare` the opening and routine lines in one batch. The player’s Start interaction enables available browser speech if sound is on. Show the opening before advancing the race clock. Use the checked preparation result only for the race that requested it.

Engine starts from the confirmed traits, course, and any seed used. Each tick carries elapsed game time and timed inputs. Session forwards the resulting snapshot and ordered new events to UI and Commentary. The animation loop continues during model calls. Cheering is optional. Engine accepts at most one effective cheer per race and records its time. All participants share the movement rules.

**Review example.** A dog begins a snack pause. UI uses the static sprite, snack prop and movement cue to show the dip. A rival passes while the dog is eating. If chase is still eligible when the pause ends, Engine emits the next movement event. UI can show the burst even with sound muted and captions covered. Fixed initial state, timed input and seed must reproduce the event order and finish order.

## Select, display and speak commentary

**Owner — Commentary.** Maintain the latest engine snapshot and a record of handled event IDs. Rendering and commentary consume the same ordered events. A routine line is eligible only when its matching event occurs. A selected important event, such as a change of leader after a snack stop, can request live text from API. Send the source event, relevant participants, supporting events and current standings. Keep one live highlight request in flight and suppress duplicate requests for the same event. The budget estimate assumes up to two live requests per race. Exact selection thresholds remain open.

| When | Caption | Browser speech |
|---|---|---|
| Matching routine event | Show the prepared line immediately. | Speak only when sound is enabled, a voice is available and speech is idle. Skip audio for a busy routine line. |
| Live response arrives | Check race identity, active phase, event time, and whether the asserted event is still current. Discard a superseded line. Display an eligible line immediately. | Speak if free. Otherwise retain at most one pending highlight, replacing it with a fresher or higher-priority eligible line. Recheck eligibility when the voice becomes free. |
| Player mutes mid-line | Captions remain available and the current race continues. | Cancel active speech, empty the pending slot, and invalidate the old completion callback. Unmute picks up the next eligible line. |
| Device has no usable voice | Display sound status and continue captions. | Skip playback. |
| Finish, replay, or restart | Retire in-race captions and invalidate old requests before publishing the new view. | Cancel race chatter and discard the pending slot. Only the active results screen can speak its recap. |

The API validates text responses against the requested operation and supporting event references. Use names, actions and standings from the engine facts supplied in the request. A reply attributing a pass to the wrong racer or crediting an unused cheer cannot be published as a supported fact. Structural validation checks identifiers and factual fields. A later live rehearsal must also judge whether the generated wording accurately describes them.

**Review example.** The cat passes the dog and a live request starts. If the dog retakes the lead before the response returns, discard a line announcing that the cat now leads. If a valid line was already captioned but its pending audio goes stale, skip playback. The speech check and the response check protect different moments.

## Finish, replay and restart

**Owner — Session, with Commentary for the recap.** On the engine’s first terminal update, publish the official ranking immediately and clear race chatter in the same transition. Pass the final record to `finish` exactly once. The recap can explain that the cat passed the dog during the snack pause when both events appear in the log. Display the recap when ready and offer it for speech when enabled.

| Transition | Retain | Invalidate or release |
|---|---|---|
| Replay from results | Current confirmed card and sprite, bundled opponents, sound preference | Retire old race ID, pending messages, timers and playback. Allocate a fresh race ID, log and cheer. A late recap from the previous race cannot enter the new one. |
| Required API failure in current work | A safe operation label for the error screen. If the recap failed, the official ranking is already available. | Offer restart. Stop work that belongs to the failed flow. A late failure belonging to an old race or draft cannot end the current one. |
| Restart after failure or end session | Bundled team assets stay available for future selections. | Invalidate session and race identities, stop capture and speech, release draft, confirmed pet, sprite references, captions and event log. Restart returns to fresh creation or example selection. |

**Review example.** Fail sprite generation for a voice-created pet. The player sees that pet image creation failed and can restart with the prepared dog. Even if an old response then arrives, the dog’s card and subsequent commentary belong to the new session.

## Verification and cost

Use mocked API responses and a fake speech adapter for ordinary Vitest and Playwright checks. Exercise delays, invalid results, duplicates, speech completion, and out-of-order responses explicitly. Use fast-check for bounded habits, cheer consumption, reproducible results and applicable lossless data round-trips. A later authorized rehearsal checks the target laptop and team account. Kelan tracks aggregate usage against the $20 budget, including the $5 rehearsal and demo reserve.

[Dependency matrix and flows](component-dependency.md) · [Design overview](application-design.md)
