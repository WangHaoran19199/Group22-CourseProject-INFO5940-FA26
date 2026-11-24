# Unit of work story map

**Units Generation · approved by Group 22 on November 24, 2026.** Every approved story belongs to [U-01, Draw & Dash local web game](unit-of-work.md). Under decision 1A, each section below is the single lead area responsible for demonstrating the outcome of its stories. The module column lists the principal technical contributors from the [approved Application Design](application-design.md#traceability-and-review-scenarios). The linked stories contain the full acceptance criteria. The checks below are intended for Construction and rehearsal.

## Pet creation and review

| Story | Formal unit | Contributing modules | Approved requirements | Acceptance check |
|---|---|---|---|---|
| [US-01 · Create or select an example](../user-stories/stories.md#us-01-start-with-a-sketch-a-description-or-an-example) | U-01 | UI, Pet | FR-01, NFR-03, NFR-04, NFR-06 | Draw two strokes, erase one and then clear the canvas. Verify drawing-only, text-only and combined submissions reach review. Selecting the bundled snack dog opens the editable source and card without a fresh interpretation request. |
| [US-02 · Speak and correct a transcript](../user-stories/stories.md#us-02-describe-a-pet-aloud) | U-01 | UI, Pet, API | FR-01, NFR-04, NFR-05, NFR-06 | Record a spoken description, stop recording, correct a misheard habit and submit. Check that permission was requested on record, tracks were released, and the reviewed transcript works alone and with other input. |
| [US-03 · Review and resolve conflict](../user-stories/stories.md#us-03-settle-what-the-game-understood) | U-01 | Pet, API | FR-02, FR-05, NFR-04, NFR-06 | Submit a winged drawing described as unable to fly. Answer the focused conflict prompt, mark the wings decorative, and confirm the edited card. Keep the drawing, apply only validated habits, and accept zero special habits. |
| [US-04 · Keep original artwork](../user-stories/stories.md#us-04-race-my-original-drawing) | U-01 | Pet, UI | FR-03, FR-05, NFR-06 | Draw a pet with a distinctive wing and add a description. Change only its habit. The same recognizable drawing appears on the track during its movement effect. |
| [US-05 · Generate a described racer](../user-stories/stories.md#us-05-see-a-pet-i-described-take-shape) | U-01 | Pet, API | FR-01, FR-03, NFR-05, NFR-06 | Create a blue, three-eared pet from text alone, then exercise the voice-only path. Confirm the appearance before requesting the generated static sprite. A change confined to habits reuses the artwork. |

## Race and session

| Story | Formal unit | Contributing modules | Approved requirements | Acceptance check |
|---|---|---|---|---|
| [US-06 · See autonomous habits](../user-stories/stories.md#us-06-watch-my-pets-habits-play-out) | U-01 | Engine, UI | FR-04, FR-05, NFR-01, NFR-03, NFR-06 | Run the player’s pet and two computer pets on the fixed course for about 30 seconds. Recognize the dog’s snack pause visually. If chase remains eligible after the pause, see the burst begin in sequence. |
| [US-07 · Cheer at most once](../user-stories/stories.md#us-07-choose-my-moment-to-cheer) | U-01 | Session, Engine | FR-04, NFR-01, NFR-06 | Finish a round without cheering. In another, use the cheer and immediately try again. Verify that the engine records at most one effective timed cheer. |
| [US-11 · See the official result](../user-stories/stories.md#us-11-see-the-official-finish-order) | U-01 | Engine, Session, UI | FR-04, FR-08, NFR-01, NFR-02, NFR-06 | Finish with a live response or recap outstanding. Show all three finishers immediately in official engine order and clear chatter. Identical course, traits, timed inputs and seed where used reproduce event order and ranking. |
| [US-13 · Replay the confirmed pet](../user-stories/stories.md#us-13-race-again-with-the-same-pet) | U-01 | Session, Engine, Commentary | FR-03, FR-04, FR-07, FR-08, NFR-02, NFR-05, NFR-06 | Replay after using a cheer. Retain the confirmed card and sprite, start with one available cheer and a fresh log, and suppress old captions, queued speech and late results. |
| [US-14 · Restart after failure](../user-stories/stories.md#us-14-restart-when-a-required-ai-step-fails) | U-01 | Session, Pet, Commentary, API | FR-09, NFR-02, NFR-04, NFR-06 | Fail a required sprite request. Identify image creation as the failed operation and offer restart. Select the prepared example and release a late completion from the prior session. It cannot replace the example or interrupt the new round. |

## Commentary and speech

| Story | Formal unit | Contributing modules | Approved requirements | Acceptance check |
|---|---|---|---|---|
| [US-08 · Follow opening and routine calls](../user-stories/stories.md#us-08-follow-the-opening-and-routine-race-calls) | U-01 | Commentary, API, Speech | FR-06, FR-07, NFR-02, NFR-05 | See the prepared AI opening before the race. Trigger the snack stop and get its routine caption without a live request. In a round without a stop, the snack caption never appears. |
| [US-09 · Follow live highlights](../user-stories/stories.md#us-09-catch-a-live-highlight-while-it-matters) | U-01 | Engine, Commentary, API | FR-06, NFR-01, NFR-02, NFR-05 | Hold the live response to a lead change. Verify the race and available cheer continue. Release a current response and check its racers and standings. Repeat after the lead has reversed and verify the superseded caption and audio are discarded. |
| [US-10 · Hear and mute during the race](../user-stories/stories.md#us-10-hear-the-action-and-mute-it-mid-race) | U-01 | Commentary, Speech, UI | FR-06, FR-07, FR-09, NFR-02, NFR-06 | Hear the routine snack call while seeing the dog dip, then mute mid-line. Verify speech and its queue stop while motion and captions continue. Exercise an eligible live highlight, unmute at the next eligible line, and check text plus status with no usable voice. |
| [US-12 · Understand the recap](../user-stories/stories.md#us-12-understand-a-moment-that-affected-the-result) | U-01 | Commentary, API, Speech | FR-07, FR-08, NFR-01, NFR-05 | Give the recap a completed log showing the cat passed the dog during its pause. Verify the pass and its effect on position against the log. An unused cheer gets no credit. Show the recap and read it when sound is enabled and available. |

## Coverage

Pet creation and review leads five stories, race and session leads five, and commentary and speech leads four. For a shared behavior, the lead verifies the whole player outcome. For example, the commentary and speech lead owns the mute check even though UI shows the control and the race keeps advancing. The race and session lead owns restart even when the failed operation was a sprite or recap request.

The requirement IDs retain the approved mapping to [FR-01 through FR-09 and NFR-01 through NFR-06](../requirements/requirements.md). Use the [dependency matrix](unit-of-work-dependency.md) to find the call or device boundary for a shared check. Mock results and device rehearsal provide distinct evidence.
