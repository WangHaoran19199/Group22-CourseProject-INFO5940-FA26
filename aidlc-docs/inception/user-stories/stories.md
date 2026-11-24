# Draw & Dash user stories

**Status** — Approved by Group 22 on November 24, 2026, under the [story generation plan](../plans/story-generation-plan.md). The stories describe the required December 16 demo. Their acceptance criteria describe what the team should observe in a future build.

**Player** — All stories follow [Sam, the casual player](personas.md#p-01). The five creation situations belong to this same player. Requirement IDs refer to the [approved requirements](../requirements/requirements.md) and the [clarification record](../requirements/requirement-verification-questions.md).

## Creation

### US-01 Start with a sketch, a description, or an example

**Persona** P-01 · **Requirements** FR-01, NFR-03, NFR-04, NFR-06

As a casual player, I want to start with whatever I have in mind, so I can enter a pet even if I don’t want to draw or type.

**Acceptance criteria**

- A drawing alone, typed text alone, and a drawing with typed text each lead to pet review. Before submitting, I can see which selected inputs will be sent to OpenAI.
- With two strokes on the canvas, erasing one leaves the other visible. Clear Canvas removes all strokes. If I keep a typed description, I can still submit it on its own.
- Choosing a bundled example takes me through the same pet review, confirmation, and race as fresh input.

### US-02 Describe a pet aloud

**Persona** P-01 · **Requirements** FR-01, NFR-04, NFR-05, NFR-06

As a casual player, I want to describe my pet aloud and correct what the game heard, so a misheard word doesn’t change my idea.

**Acceptance criteria**

- Microphone permission is requested when I choose to record. When recording ends, capture stops and the microphone tracks are released.
- After transcription, I can change “likes naps” to “likes snacks”. When I submit, the corrected words are used in review. Voice alone is sufficient.
- With a drawing, typed text, or both already supplied, I can add a spoken description. The review accounts for the other inputs and my edited transcript.

## Review and confirmation

### US-03 Settle what the game understood

**Persona** P-01 · **Requirements** FR-02, FR-05, NFR-04, NFR-06

As a casual player, I want to correct the pet card and answer questions about conflicting details, so the racer reflects what I meant.

**Acceptance criteria**

- The card shows the proposed appearance and zero, one, or two special habits. I can correct the interpretation before confirming. A race can start only after confirmation.
- Given a winged drawing and the description “cannot fly”, I get a focused clarification. If I confirm the wings are decorative, they remain visible and the pet gains no flight behavior.
- An unsupported effect in a model response cannot enter the confirmed race rules. A pet with no special habits can still be confirmed and raced.

### US-04 Race my original drawing

**Persona** P-01 · **Requirements** FR-03, FR-05, NFR-06

As a casual player, I want to recognize my own lines on the track, so the pet I drew is the one I see race.

**Acceptance criteria**

- With a supplied drawing, the confirmed sprite uses that artwork, including after I add text or speech. A distinctive feature such as a drooping wing stays recognizable during movement.
- If I change only a habit, the approved drawing stays the same. During a visible reaction, the game moves the static artwork and can add a prop or effect.

### US-05 See a pet I described take shape

**Persona** P-01 · **Requirements** FR-01, FR-03, NFR-05, NFR-06

As a casual player, I want to see the creature I imagined even when I have no drawing, so I can recognize it on the track.

**Acceptance criteria**

- For text alone, voice alone, or text and voice together without a drawing, confirming the appearance produces a static, simple hand-drawn-style sprite. In a fixture describing a blue, three-eared pet, those confirmed features can be compared with the displayed sprite.
- The race animates the same sprite. A later habit-only change reuses the approved artwork without another image-generation request.

## Racing and commentary

### US-06 Watch my pet’s habits play out

**Persona** P-01 · **Requirements** FR-04, FR-05, NFR-01, NFR-03, NFR-06

As a casual player, I want to see why my pet fell behind or caught up, so a funny loss is as understandable as a win.

**Acceptance criteria**

- Starting a round puts my pet and two bundled computer pets on one fixed course. They finish autonomously in about 30 seconds, using the same movement and habit rules.
- With sound muted and captions covered for observation, a teammate can identify the dog dipping toward a snack and pausing, or the cat scratching a post. Reusable effects such as pause, slow, and burst make the behavior visible.
- If a rival passes a dog during its snack pause, a chase cannot accelerate the dog during that pause. Once the pause ends, a still-eligible chase can begin and be seen. With zero habits the pet still completes the course.

### US-07 Choose my moment to cheer

**Persona** P-01 · **Requirements** FR-04, NFR-01, NFR-06

As a casual player, I want to decide when to give one cheer, so I can take part while the race still runs itself.

**Acceptance criteria**

- When I use an available cheer during the race, the engine records one timed cheer and the control shows it has been used. A second attempt cannot apply another cheer.
- If I never cheer, all three racers can finish and the engine still produces an official ranking.

### US-08 Follow the opening and routine race calls

**Persona** P-01 · **Requirements** FR-06, FR-07, NFR-02, NFR-05

As a casual player, I want the introduction and routine calls to match what I see, so I can follow a race even with sound off.

**Acceptance criteria**

- The AI opening and routine lines are prepared before the race. The opening is displayed before racing and is eligible for speech when sound is enabled.
- When the dog stops at a snack, its matching prepared caption appears without waiting for a live model response. In a round without that stop, the snack call never appears.

### US-09 Catch a live highlight while it matters

**Persona** P-01 · **Requirements** FR-06, NFR-01, NFR-02, NFR-05

As a casual player, I want a surprising overtake called as it happens, so the commentary helps me notice who passed whom.

**Acceptance criteria**

- Given a logged lead change chosen for a live highlight, the request uses the current race’s log and standings. A current returned caption identifies the actual racers and the order of the pass.
- While a live response is pending, racers keep moving and the cheer control still works if unused. Duplicate submissions for the same highlight are suppressed, with at most one live highlight request in flight.
- If standings supersede the claim, the race ends, or a replay starts before the response arrives, that line is discarded from captions and speech.

### US-10 Hear the action and mute it mid-race

**Persona** P-01 · **Requirements** FR-06, FR-07, FR-09, NFR-02, NFR-06

As a casual player, I want to hear calls while I watch my pet and silence them instantly, so I can choose how to follow the race.

**Acceptance criteria**

- With browser speech enabled and the narrator free, I hear the snack call while the race is running, recognize the dog dipping toward the treat, and mute it mid-line. Speech stops and its queue clears immediately. Racers and captions continue.
- Routine lines that arrive during speech are captioned and their audio is skipped. A selected live overtake can be spoken during the race if still current. Only one line speaks at once and at most one highlight waits. A fresher or higher-priority highlight can replace the pending one.
- Sound on / Mute is available from the start screen through results. Unmuting starts with the next eligible line. On a device without a usable voice, I see the sound status and can follow the captions.

## Results

### US-11 See the official finish order

**Persona** P-01 · **Requirements** FR-04, FR-08, NFR-01, NFR-02, NFR-06

As a casual player, I want to know where everyone finished as soon as the race ends, so I can tell the result even if the narrator is still catching up.

**Acceptance criteria**

- At the finish the three pets appear in engine-determined order, even when a live line or the recap is pending. In-race chatter clears and a late line cannot change the ranking.
- Repeating a simulation with identical confirmed racers, course, timed inputs, and seed (if used) reproduces both the official event order and ranking. Different narrator wording has no effect.

## Recap

### US-12 Understand a moment that affected the result

**Persona** P-01 · **Requirements** FR-07, FR-08, NFR-01, NFR-05

As a casual player, I want a short explanation of what my pet did, so I can connect the finish order to a moment I saw.

**Acceptance criteria**

- If the log records a cat passing the dog during its snack pause, the AI recap can explain that loss of position. Each factual claim agrees with the log and official ranking. A round without a cheer cannot credit one.
- When ready, the brief recap is displayed alongside the result and read aloud if sound is enabled and a usable voice is available. With sound muted, I can read it.

## Replay

### US-13 Race again with the same pet

**Persona** P-01 · **Requirements** FR-03, FR-04, FR-07, FR-08, NFR-02, NFR-05, NFR-06

As a casual player, I want to try another round with my confirmed pet, so I can try a different cheer without recreating the character.

**Acceptance criteria**

- Choosing replay within the session reuses the confirmed card and sprite, including a previously generated sprite. The new race starts with one available cheer and a fresh event log.
- A line queued, speaking, or returned late from the previous race cannot become a caption or be heard in the new round.

### US-14 Restart when a required AI step fails

**Persona** P-01 · **Requirements** FR-09, NFR-02, NFR-04, NFR-06

As a casual player, I want to know which step failed and have a way to start over, so I can get back to the game.

**Acceptance criteria**

- If a required request fails, the message names the operation and offers restart. For example, a failed sprite request identifies pet image creation, and a failed recap identifies the recap.
- After restart, I can begin a new creation or select a prepared example. Outstanding text and speech from the failed session cannot appear in the new one.

## Requirement coverage

| Requirement | Review topic | Stories |
|---|---|---|
| FR-01 | Create with fresh inputs or examples | US-01, US-02, US-05 |
| FR-02 | Review, correct, and clarify | US-03 |
| FR-03 | Preserve or generate artwork | US-04, US-05, US-13 |
| FR-04 | Autonomous race and timed cheer | US-06, US-07, US-11, US-13 |
| FR-05 | Observable sequential habits | US-03, US-04, US-06 |
| FR-06 | Opening, routine captions, and live highlights | US-08, US-09, US-10 |
| FR-07 | Spoken commentary and sound controls | US-08, US-10, US-12, US-13 |
| FR-08 | Official results, grounded recap, and replay | US-11, US-12, US-13 |
| FR-09 | Failed requests and unavailable speech | US-10, US-14 |
| NFR-01 | Reproducible engine results and grounded claims | US-06, US-07, US-09, US-11, US-12 |
| NFR-02 | Timely captions, speech, and ranking | US-08, US-09, US-10, US-11, US-13, US-14 |
| NFR-03 | Local runtime | US-01, US-06 |
| NFR-04 | Session privacy and validated inputs | US-01, US-02, US-03, US-14 |
| NFR-05 | Provider usage and budget | US-02, US-05, US-08, US-09, US-12, US-13 |
| NFR-06 | Verification strategy | US-01, US-02, US-03, US-04, US-05, US-06, US-07, US-10, US-11, US-13, US-14 |

## Verification across the journey

- **Local play (NFR-03).** Walk through creation to replay on the demo laptop using the selected vanilla TypeScript, Vite, HTML5 Canvas, Node.js, npm, and loopback Express setup. The browser runs the simulation. Hosted AI requests go through the local server and need internet access.
- **Privacy (NFR-04).** Check the disclosure before sending selected inputs, microphone access on record, and capture release on stop. Inspect the client configuration for exposed keys and the application logs for raw or generated content. Try an oversized or unsupported input and an invalid model effect. Verify that submissions stay in application session memory and are discarded locally at session end. Provider retention is separate.
- **Cost (NFR-05).** Use mocks in routine development and automated tests. Bundle the two computer pets and reuse confirmed sprites. The $20 budget includes $5 reserved for rehearsal and the demo. The planning estimate allows up to two live highlight calls per race. The [technical environment](../../../technical-environment.md) records the planned models, request policy, and cost assumptions.
- **Tests (NFR-06).** Use Vitest examples and partial fast-check properties for the one-cheer limit, zero through two habits, sequential effects, repeatable race order, and lossless data round-trips where applicable. Preserve failing seeds and shrunk cases. Use Playwright with mock responses for independent and combined inputs, clarification, sprite use, race, captions, mute, recap, replay, and failures. Separately observe actual microphone capture and audible mid-race playback on the demo laptop.

## Scope and open details

Extra courses and same-device multiplayer are optional after the core round works. Online multiplayer, accounts, a pet database, persistent care or training, a tool-calling race director, hosted voice generation, and per-pet skeletal or video animation are outside the demo.

Later design work will settle the initial habit catalog, tie-breaks, effect sizes and durations, cheering during a pause, highlight selection and freshness thresholds, the finish-to-recap cutoff, and recording and upload limits. Team-account model access, actual charges and latency, sprite wait time, target-device capture and audible playback, and any switch to the lower-cost commentary model await live checks.
