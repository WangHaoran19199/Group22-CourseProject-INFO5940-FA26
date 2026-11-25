# Draw & Dash

*Draw it and erase it, draw and dash.*

INFO 5940 · Fall 2026 · Group 22 · Approved by the team on November 24, 2026

| Team member | Cornell NetID |
|---|---|
| Haoran Wang | hw877 |
| Kelan Mao | km2268 |
| Ken Li | dl2362 |
| Wangxinzhu Zou | wz476 |
| Yiou Li | yl4498 |

- **Coding agents used by the team** — OpenAI Codex and Claude Code
- **AI-DLC version** — 1.0.1

Draw & Dash is a standalone browser game for casual players who enjoy inventing unusual creatures and seeing what they do. A character creator with preset abilities makes it hard to express a specific personality, like a dog that gives up first place for a snack and then chases the pet that passed it. A multimodal language model can interpret a sketch and an open-ended description and suggest which supported race behaviors capture that personality. The player gets to see their own creature on the track and recognize the habits they imagined.

Players can draw, type, speak and record a description, or combine those inputs. They can erase individual strokes, clear the canvas, and edit a voice transcript before submitting it. OpenAI’s `gpt-transcribe` transcribes recordings, and `gpt-6.1-terra` uses the drawing or description to propose the pet’s appearance and up to two playable habits. The player can correct the proposal before confirming it. For example, if the sketch shows wings and the description says the pet cannot fly, the game asks whether the wings are decorative. The player’s drawing becomes the artwork shown on the track. If the player supplies only text or voice, `gpt-image-2.6-flare` creates a simple hand-drawn-style image from the confirmed appearance.

The game calls an OpenAI API at specific points in the round. We’re deferring the tool-calling race director because the fixed course has few decisions for it to make. The race engine applies the same movement rules to all pets, handles habits one at a time, records what happens, and determines the official ranking. Before the race, `gpt-6.1-terra` writes the introduction and lines for routine events. Those lines appear when their events happen, and selected moments, such as a lead change, can trigger new commentary based on the race log and current standings. After the finish, the model uses the completed log to explain how habits and player actions affected the result. During the race, browser speech synthesis can read routine lines when the narrator is free and selected live highlights while the event is still current. Players can mute speech and keep the captions. 

By the December 16 final demo, one player will be able to create a pet from scratch or start with a prepared example, race two computer-controlled pets, see the official ranking and a short AI recap, and replay with the same pet. Each race runs on one fixed course for about 30 seconds, and the player can choose when to cheer once. Simple motions make habits visible even with sound muted. For example, a dog dips toward a treat and pauses, then may speed up to chase a rival. A single course, bundled opponents, and reusable motions keep the full round achievable. Extra courses and same-device multiplayer are optional. The initial API budget is $20, including $5 for rehearsal and the demo. Kelan Mao will verify account access, response times, and actual costs.

We meet in person once a week and use instant messaging apps to coordinate between meetings. We share development across three areas based on each member’s strengths and the needs of each feature. These areas cover creating and reviewing pets, running races and handling replay, and providing captions and spoken commentary. Kelan Mao runs the shared AI-DLC workflow. All five members discuss major decisions and review the work together before approving each stage.

[Vision document](vision.md) · [Technical environment](technical-environment.md) · [AI-DLC state](aidlc-docs/aidlc-state.md)
