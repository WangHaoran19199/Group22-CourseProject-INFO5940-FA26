# Draw & Dash requirements clarification record

This November 24, 2026 revision consolidates decisions from the team’s earlier discussions and the approved [README](../../../README.md), [vision](../../../vision.md), and [technical environment](../../../technical-environment.md). The questions and options organize those decisions retrospectively. Each filled answer summarizes a choice the team had already confirmed. The functional (FR) and quality (NFR) identifiers refer to [approved requirements](requirements.md).

The [previous seven-question record](../../history/requirements/verification-at-resumption.md) preserves the answered version used at workflow resumption. Its Q5–Q7 extension decisions are retained separately as Q15–Q17 below. The [audit log](../../audit.md) records both versions.

## Question 1

Who is the December demo for, and what does a complete round demonstrate?

A) A casual player creates a pet and races it against two computer pets.

B) Several players take turns creating pets on the same device for a shared race.

X) Other (please describe after [Answer]: tag below)

[Answer]: A) A casual player should recognize their own creature and its habits on the track, even when it loses. The required round runs from creation and confirmation through a race, ranking, AI recap, and replay. Extra courses and same-device multiplayer are optional after the core game is complete. Online multiplayer, accounts, a pet database, and persistent training are outside the MVP.

*FR-04, FR-05, FR-08*

## Question 2

Which creation inputs must the demo accept?

A) Require a drawing, with a typed or spoken description as optional context.

B) Accept drawing, typed text, or recorded voice independently and in combination.

X) Other (please describe after [Answer]: tag below)

[Answer]: B) Players can create a fresh pet with any one input, combine inputs, or start from a prepared team example. The drawing tools include a stroke eraser and Clear Canvas. Players can edit a voice transcript. Both fresh and prepared pets go through review and racing.

*FR-01*

## Question 3

Who resolves a mismatch between the drawing and the description?

A) Apply one input as the default whenever the two disagree.

B) Show the proposed interpretation and let the player clarify and correct it.

X) Other (please describe after [Answer]: tag below)

[Answer]: B) The player reviews appearance and habits before racing. For wings paired with “cannot fly,” ask whether flight is intended. Decorative wings stay visible, and the confirmed choice governs race behavior.

*FR-02*

## Question 4

What artwork should appear on the track?

A) Keep a supplied drawing and generate artwork when the pet has no drawing.

B) Keep a supplied drawing and use customizable preset assets for all pets without drawings.

X) Other (please describe after [Answer]: tag below)

[Answer]: A) Use the player's original artwork whenever supplied. For text- or voice-created pets without a drawing, generate a simple hand-drawn-style static sprite from the confirmed appearance. Animate the sprite with reusable movements and effects. Reuse it for replay and keep approved artwork when only a habit is corrected.

*FR-03, FR-05, FR-08*

## Question 5

How much control does a player have after the start?

A) Watch the race without any in-race action.

B) Choose when to cheer once during an otherwise autonomous race.

C) Direct the pet’s movement throughout the race.

X) Other (please describe after [Answer]: tag below)

[Answer]: B) One human-created pet races two computer pets on a single fixed course with recognizable objects and opportunities to overtake. The race lasts about 30 seconds, runs to completion without a cheer, and permits at most one effective player cheer per race.

*FR-04*

## Question 6

How many signature habits can a pet have, and how do their effects interact?

A) Use at most one special habit.

B) Allow up to two habits and execute their movement effects in sequence.

X) Other (please describe after [Answer]: tag below)

[Answer]: B) Habits may help or distract the pet. A dog can pause at a snack and then burst forward if its chase trigger still holds. A cat can rock toward a scratching post. Reuse pause, slow, and burst rules with different props and visual cues so players can recognize a reaction with sound and captions off.

*FR-05, NFR-01*

## Question 7

Who determines the course events, movement, and official result?

A) Use the fixed course and shared engine rules, with the LLM interpreting and narrating.

B) Include a tool-calling AI director to select race events as part of the MVP.

X) Other (please describe after [Answer]: tag below)

[Answer]: A) The engine applies the same rules to every pet, records events and standings, and determines the finish order. Identical initial state, timed inputs, and seed if used must reproduce the race result. AI uses the confirmed pet and race facts for narration. The director is deferred.

*FR-04, FR-06, FR-08, NFR-01*

## Question 8

When is the text commentary generated?

A) Prepare every line in advance and select lines during play.

B) Generate every in-race line after its event.

C) Prepare routine lines in advance and generate selected highlights from actual events.

X) Other (please describe after [Answer]: tag below)

[Answer]: C) Generate the opening and routine lines before the race. Display a prepared line when its matching event occurs. A lead change near a habit, a consequential chain of habits, or a close finish can merit a live line. The race continues during generation. Suppress superseded and previous-race lines. The budget estimate allows up to two live calls per race.

*FR-06, NFR-02, NFR-05*

## Question 9

Which lines can players hear during the race, and how does mute work?

A) Offer captions only.

B) Use browser speech synthesis for the opening, eligible in-race lines, and recap.

C) Use hosted voice generation for the commentary.

X) Other (please describe after [Answer]: tag below)

[Answer]: B) Spoken commentary is required. Speak a routine line if the narrator is free and a selected live highlight while its event is current. Speak one line at a time with at most one pending highlight. Keep captions available when speech is busy, muted, or unavailable. Show sound status if the device has no usable voice. Mute clears current and queued speech immediately, and unmute picks up the next eligible line. A player should be able to hear the snack call, recognize the visible dip toward the treat, and mute it while the race continues.

*FR-07, FR-09, NFR-02*

## Question 10

What should happen at the finish and on replay?

A) Show ranking and replay only.

B) Show ranking, give a brief AI analysis of the logged race, and offer replay.

X) Other (please describe after [Answer]: tag below)

[Answer]: B) Display the engine’s official ranking immediately, clear race chatter, and show the required AI recap. Speak it when sound is enabled. Explain habits and actions using recorded facts, such as a cat passing the dog during its snack stop. A replay can reuse the confirmed pet within the session, starts with one new cheer, and clears previous narration.

*FR-07, FR-08, NFR-01, NFR-02*

## Question 11

Where will the product run, and which stack will the team use?

A) Run a standalone local web game with TypeScript in the browser and on the server.

B) Run a local web game with a TypeScript frontend and a Python backend.

X) Other (please describe after [Answer]: tag below)

[Answer]: A) Use vanilla TypeScript, Vite, HTML5 Canvas, Node.js, npm, and a loopback Express server on a team laptop. Live OpenAI calls need internet access. Use the versions in the approved technical environment. The November 24 milestone covers Inception through Units Generation. The playable demo is due December 16.

*NFR-03*

## Question 12

Which model provider and budget will support the demo?

A) Use hosted OpenAI models with a shared project budget.

B) Run local models for all product AI features.

X) Other (please describe after [Answer]: tag below)

[Answer]: A) Plan for `gpt-6.1-terra` for interpretation and commentary, `gpt-image-2.6-flare` for sprites without drawings, and `gpt-transcribe` for recorded input. `gpt-6-luna` is a possible lower-cost commentary substitute. Use browser speech synthesis for playback. Of the initial $20 budget, $5 is reserved for rehearsal and the final demo. Reuse sprites and bundled computer pets. Kelan Mao owns the live access and usage checks.

*NFR-05*

## Question 13

What happens to player data during and after a session?

A) Keep player data in session memory and disclose provider processing.

B) Save player profiles and media across sessions.

X) Other (please describe after [Answer]: tag below)

[Answer]: A) Disclose OpenAI processing before submission. Request microphone permission when recording starts and stop the tracks afterward. Validate input types, sizes, and model outputs. Keep credentials server-side and raw submissions and generated content out of logs. Discard player data locally when the session ends. Provider retention is governed separately.

*NFR-04*

## Question 14

Which testing approach will the team use for routine work?

A) Use mocks for routine tests and separate live checks on the demo laptop.

B) Make live provider calls during routine automated tests.

X) Other (please describe after [Answer]: tag below)

[Answer]: A) Use mock AI responses and a fake speech interface for repeatable tests. Use Vitest and fast-check for game logic and transformations, and Playwright for browser journeys including clarification, artwork preservation, mute, recap, and recovery. Verify real model behavior and device audio separately.

*NFR-05, NFR-06*

**AI-DLC v1.0.1 extension opt-ins**

## Question 15

Should the AI-DLC Resiliency Baseline apply?

A) Yes. Use it during design to address fault tolerance, availability, observability, and recovery.

B) No. Skip this extension for the course prototype.

X) Other (please describe after [Answer]: tag below)

[Answer]: B) Off. A failed required API call can end the session. Identify the failed operation and offer restart. Clear prior narration when restarting.

## Question 16

Should the AI-DLC Security Baseline apply?

A) Yes. Enforce the extension's security rules as blocking constraints.

B) No. Skip this extension.

X) Other (please describe after [Answer]: tag below)

[Answer]: B) Off. The agreed security basics still apply, including server-only credentials, input validation and size limits, microphone permission on use, and excluding raw player content from application logs.

## Question 17

Should the AI-DLC Property-Based Testing extension apply?

A) Yes. Enforce the full extension.

B) Partial. Apply it to pure functions and serialization round-trips.

C) No. Skip this extension.

X) Other (please describe after [Answer]: tag below)

[Answer]: B) Partial. Apply property-based testing to pure game calculations and data transformations, including applicable serialization round-trips. Examples include one-cheer consumption, the two-habit limit, and reproducibility with identical race inputs. Use fast-check with Vitest.

*Failure handling is covered by FR-09, security basics by NFR-04, and testing by NFR-06.*

## Deferred design decisions

The team agreed to sequential habits, one cheer, hybrid commentary, and playback priority. The initial habit catalog, effect magnitudes and durations, tie-breaks, and cheer behavior during a pause still need design work. Highlight thresholds, line freshness and finish cutoff, acceptable sprite wait time, target-device voice selection, and final recording and upload limits also remain open. The approved technical environment’s example limits and cost estimates are planning inputs.

## Unverified runtime facts

The selected software versions passed isolated compatibility checks. Access to the planned models on the team’s account, actual charges and latency (including sprite generation), microphone capture, and audible playback on the demo laptop still need live verification. A possible switch to Luna depends on measured cost, quality, and timing.

**Review status.** All clarification answers above come from confirmed decisions. Group 22 approved the requirements document on November 24, 2026.
