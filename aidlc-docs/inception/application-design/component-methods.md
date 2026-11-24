# Component methods

**Application Design · approved by Group 22 on November 24, 2026.** These signatures describe input, output, ownership, and failure behavior. Names are proposed TypeScript interfaces. Field layouts, exact thresholds, and business-rule algorithms belong to Construction.

## Data exchanged

| Contract | Meaning |
|---|---|
| `CreationInput / AudioClip / Transcript` | Selected drawing (optional), typed words (optional), and reviewed transcript (optional). A recording is a separate typed media payload used for transcription. Clearing the drawing preserves text and transcript. |
| `DraftCard / CardEdit / ConfirmedCard` | Appearance, zero to two supported habit IDs, and any focused conflicts and answers. Edits can update fields or answer a conflict. A confirmed card contains validated behavior and the player’s resolved choices. |
| `PetDraft / ConfirmedPet / Sprite` | A revision of source input and editable card, then a confirmed card paired with artwork. The sprite records its origin (drawing or generated) and which drawing or confirmed appearance it represents. |
| `PreparedExample / Appearance` | A bundled source input paired with its matching editable draft card. Appearance is the confirmed visual description sent for a generated sprite. A habit change alone leaves it intact. |
| `VoiceStatus / Listener / Unsubscribe` | Device speech availability or playback status, a no-argument change notification, and a function that removes a subscription. The device adapter may report unavailability after an attempted playback. |
| `RaceSetup / RaceBrief` | A race ID, the player and two bundled opponents, course, and optional seed. Engine receives the simulation setup. Commentary receives the race ID, display names, relevant confirmed traits, and possible course events. |
| `TimedInput / RaceSnapshot / RaceUpdate` | A player cheer with game time. A snapshot holds race ID, time, phase, positions, active visible effects, standings and cheer availability. An update holds that snapshot and newly logged events in engine order. |
| `RaceEvent / HighlightFacts / FinishRecord` | Each event has a race-scoped sequence ID, game time, type, affected racers and relevant prop or action. Highlight facts include the source event, supporting log entries, and standings. The finish record holds the race ID, official ranking, and completed event log with the racer names needed for narration. |
| `PreparedLines / Caption / CommentaryView` | Opening and event-keyed routine text. A caption has race ID, kind, text, priority, and applicable event reference(s) and time. For prepared routine text, bind those references when the matching event occurs. CommentaryView holds visible text, sound status, and any operation failure. |
| `ScreenState / PlayerIntent / SessionPort` | Read-only UI view of phase, draft or confirmed pet, latest race update, ranking, narration, and any error. Intents include creating, editing, confirming, cheering, muting, replaying, and restarting. SessionPort exposes dispatch, time advancement, reading the view, and change notification. |
| `Result<T> / OperationError` | A successful value or a failure carrying the operation, category, and safe player-facing message. A local correction or rejected input can return a field issue. An active required API failure offers restart. A superseded request cannot produce an error in the new draft or race. |

## Browser interfaces

### UI

| Signature | Contract |
|---|---|
| `mount(session: SessionPort) → void` | Bind the form, pointer and sound controls. Forward user actions and race-time updates. Subscribe to view changes so a late recap can appear after animation has ended. |
| `render(screen: ScreenState) → void` | Show the appropriate screen. Render generated strings as plain text. Draw the latest authoritative positions and apply each newly seen visual event once. |
| `startRecording() → Promise<Result<void>>` | Request permission only on record. If capture is unavailable, identify the input issue and leave drawing and typing available. |
| `stopRecording() → Promise<Result<AudioClip>>` | Finish the clip and release tracks on success, cancellation, or error. Disclose processing before sending the clip for transcription. |
| `releaseCapture() → void` | Stop any remaining tracks on restart, exit, or disposal. A late capture completion cannot populate a new session. |

### Session

| Signature | Contract |
|---|---|
| `dispatch(intent: PlayerIntent) → void` | Route actions to Pet, Engine via a timed input, or Commentary. Accept confirmation, start, replay and restart only in a valid phase. |
| `advanceTo(raceTimeMs: number) → void` | Advance an active race using elapsed game time and buffered inputs. Publish the engine update for UI and pass it to Commentary without waiting for HTTP or speech. |
| `read() → ScreenState` | Project the draft, current race snapshot, official result, captions and sound status for rendering. |
| `subscribe(onChange: Listener) → Unsubscribe` | Notify UI when the view changes, including after a user edit, provider response, or sound status change. Listener takes no arguments and reads the current view. |

### Pet

| Signature | Contract |
|---|---|
| `loadExample(example: PreparedExample) → PetDraft` | Copy a bundled source and matching card into review. Increment the draft revision, so an older fresh-input request cannot replace it. |
| `transcribe(clip: AudioClip) → Promise<Result<Transcript>>` | Request transcription through API. Expose the result for editing before interpretation. |
| `interpret(source: CreationInput) → Promise<Result<PetDraft>>` | Request a new proposal for fresh input or explicit reinterpretation. Attach it only to the matching draft revision. |
| `edit(draft: PetDraft, change: CardEdit) → Result<PetDraft>` | Update appearance, habits, or a conflict answer locally. Track unresolved questions and keep the source sketch. Return field feedback for an invalid edit. |
| `confirm(draft: PetDraft, previous?: ConfirmedPet) → Promise<Result<ConfirmedPet>>` | Validate resolved answers and supported habits. Select the supplied drawing, reuse matching artwork, or request a generated sprite. Return the accepted card and artwork as a pair. |
| `reset() → void` | Invalidate the draft revision and release transient submissions. A prepared example is copied again on the next selection. |

### Engine

| Signature | Contract |
|---|---|
| `start(setup: RaceSetup) → RaceUpdate` | Initialize the fixed course, three racers, empty log, and one available player cheer. Return starting positions. |
| `advanceTo(raceTimeMs: number, inputs: TimedInput[]) → RaceUpdate` | Apply inputs in game-time order, using reproducible simulation steps independent of display frame rate. Advance sequential habit effects and emit new ordered events. A consumed cheer cannot take effect again. |
| `result() → FinishRecord or absent` | Return the official order and completed log after all racers finish. During racing there is no final record. |

### Commentary

| Signature | Contract |
|---|---|
| `prepare(brief: RaceBrief) → Promise<Result<PreparedLines>>` | Fetch and install the opening and routine lines for the active race. Display the opening before the race and offer it for speech if eligible. |
| `observe(update: RaceUpdate) → void` | Take the current snapshot and append new log facts once. Match routine lines and launch at most one live highlight request at a time. Completion uses the latest snapshot. |
| `setSound(enabled: boolean) → void` | Update sound preference. Mute cancels speech and clears the pending slot immediately. Unmute waits for the next eligible line. |
| `finish(record: FinishRecord) → Promise<Result<Caption>>` | Synchronously retire in-race captions, requests, and speech. Request the required recap from the authoritative final record and publish it if still on those results. |
| `clearRace() → void` | Invalidate the old race, pending responses and speech callbacks, clear captions, and cancel speech. Retain the current sound setting for the next round. |
| `read() → CommentaryView / subscribe(onChange: Listener) → Unsubscribe` | Let Session observe captions, audio status, and current-operation errors without introducing a race event bus. |

### Speech

| Signature | Contract |
|---|---|
| `availability() → VoiceStatus` | Report whether device speech can currently be used. |
| `play(line: Caption, onDone: Listener) → VoiceStatus` | Speak one eligible line using browser speech synthesis. Notify Commentary when it ends or errors. The callback is scoped to that playback attempt. |
| `cancel() → void` | Stop current playback. Commentary invalidates callbacks and owns the one pending highlight slot. |

## API

Each proposed local route uses POST. The browser client exposes the named operation as an asynchronous method. Express validates the payload and returns a successful result or an operation-specific error. Audio and drawings use validated media transfers. Raw player content and model responses stay out of logs.

| Client signature | Local route | Contract |
|---|---|---|
| `transcribe(input: AudioClip) → Promise<Result<Transcript>>` | `/api/voice/transcribe` | A short recording after disclosure. Return editable text and release the original recording when it is no longer needed. |
| `interpret(input: CreationInput) → Promise<Result<DraftCard>>` | `/api/pets/interpret` | At least one selected drawing, typed description, or edited transcript. Return appearance, proposed supported habits, and focused questions. The server validates the proposed values. |
| `generateSprite(input: Appearance) → Promise<Result<Sprite>>` | `/api/pets/sprite` | Send the confirmed description. Return a simple hand-drawn-style static image suitable for scaling on Canvas. |
| `prepareLines(input: RaceBrief) → Promise<Result<PreparedLines>>` | `/api/commentary/prepare` | One pre-race request for the opening and routine phrases indexed by an event type and relevant racer or habit. |
| `highlight(input: HighlightFacts) → Promise<Result<Caption>>` | `/api/commentary/highlight` | Send the selected event with supporting log facts and current standings. Return text associated with the requesting race and source event. |
| `recap(input: FinishRecord) → Promise<Result<Caption>>` | `/api/commentary/recap` | Send the completed race facts and official ranking. Return a short recap referring to the events it describes. |

**Request ownership.** The browser associates every creation request with its session and draft revision, and every narration request with its race and operation. The server binds returned text to the request’s facts and identifiers. The browser checks that association again before applying the result. A required request failure for the active operation is visible and restartable. Superseded responses, including errors, are discarded. Input errors identified before submission are shown where the player can correct them.

**Provider selection.** The approved plan uses `gpt-transcribe` for clips, `gpt-image-2.6-flare` for generated sprites, and `gpt-6.1-terra` for card interpretation and text. `gpt-6-luna` remains a candidate for commentary pending measurements. Speech playback uses the browser. See the [technical environment](../../../technical-environment.md) for planned provider endpoints and budget assumptions.

[Component responsibilities](components.md) · [Service sequences](services.md) · [Design overview](application-design.md)
