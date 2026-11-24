# Draw & Dash — Vision

*Approved by the team · INFO 5940 Group 22 · November 24, 2026*

## Player and purpose

Draw & Dash is a single-player browser game for casual players who enjoy doodling creatures or making up their personalities. Players sketch a pet, describe it in writing or aloud, or combine those inputs. They confirm its appearance and up to two habits, then watch it race two computer-controlled pets. The fun is seeing how those habits change the race. A food-loving dog might give up the lead for a snack, making the race memorable even if it loses.

## One complete round

1. **Create.** Draw a creature, type a description, or record a spoken description. The stroke eraser removes individual strokes, and Clear Canvas lets the player start the drawing over. Each method works on its own. Players can also combine them or begin with a prepared example. Voice yields an editable transcript.
2. **Confirm.** Review the appearance and up to two special habits. Correct an interpretation or answer a focused question if the inputs disagree. The player’s original drawing becomes the racing character whenever one is supplied. Otherwise the game generates a hand-drawn-style sprite. A correction to a habit can preserve the approved appearance.
3. **Race.** See and hear an AI opening introduction, then watch one human-created pet and two computer pets run the same fixed course in about 30 seconds. The player gets one timed cheer. The engine owns movement, overtakes, and finish order.
4. **Recognize a habit.** A dog may stop at food, lose its lead, and then accelerate because it chases other pets. A cat may stop to scratch a post. A small set of movement rules, including pause, slow, and burst, combines with visible props and movements of the static artwork. Up to two habits can take effect in sequence. Computer pets follow the same rules.
5. **Hear what happened.** Routine AI-written lines appear when their matching events occur. Selected important events prompt live AI text based on actual race state. During the race, browser speech synthesis can read a routine line when the narrator is free or a live highlight while its event is still current. Only one line plays at a time, and at most one highlight waits. Players can mute speech while continuing to read captions.
6. **See the result and replay.** The engine shows the official ranking. A brief AI recap uses the event log to explain how habits and player actions affected performance. For example, it may say that the cat passed the dog while the dog ate if the log records that overtake. Replay can reuse the confirmed pet within the session.

## December scope

The six steps above are required for the local standalone web demo by December 16. Both fresh inputs and prepared examples must work. Required AI requests that fail lead to a clear message and a restart option. The game keeps player data within the application session and discloses when input is sent to OpenAI.

| Priority | Features |
|---|---|
| Optional if the core game is complete | Additional courses and same-device multiplayer. |
| Out of scope for the demo | Online multiplayer, accounts, a pet database, persistent training or companion care, and a tool-calling AI race director. Hosted voice generation and per-pet skeletal or video animation are outside the chosen implementation. |

## How the team can judge the demo

- A first-time player can create a pet through drawing alone, text alone, or voice alone. Combining inputs can produce a clarification the player can resolve before racing.
- With captions hidden and audio muted for observation, a teammate can identify a food stop or scratching reaction from the sprite and prop.
- With sound on and the race still running, a player hears the snack call, recognizes the dog dipping toward the treat, and mutes the line. Speech stops immediately while the racers and captions continue.
- Replaying the simulation with identical initial state, any random seed used, and timed inputs reproduces the official event order and ranking. Narrator wording can vary.
- Each live claim and recap statement can be checked against the recorded events.

## Open questions

Which initial habits and props make the two-habit sequence easiest to see? How long should a pause last, and how should the engine break simultaneous trigger ties or time a cheer during a pause? What lead change or finish margin qualifies for a live highlight? How long can a line wait before it becomes stale, and when should in-race playback cut off as the results appear? Team testing can also settle acceptable sprite wait times, speech voices on the demo laptop, and whether another commentary model offers enough savings at equivalent quality.
