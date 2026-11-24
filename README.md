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

Draw & Dash is a standalone browser game for casual players who enjoy drawing unusual creatures and discovering what they will do. A character creator with a fixed list of pets and abilities has trouble capturing a dog that would abandon first place for a snack and then chase the friend who passed it. We want a player to invent that dog in their own words or sketch it, recognize it on the track, and see both habits play out in a race.

Players can draw, type, speak, or combine those inputs. The drawing tools include a stroke eraser and a clear-canvas option. An OpenAI model interprets the pet’s appearance and proposes up to two playable habits. Players can correct its interpretation. If the drawing suggests wings and the description says the pet cannot fly, the game asks whether the wings are decorative. A drawing becomes the racer’s original artwork. For a pet created entirely through text or voice, the game generates a simple hand-drawn sprite from the confirmed appearance.

Our planned product models are OpenAI’s `gpt-6.1-terra` for interpretation and commentary, `gpt-image-2.6-flare` for generated sprites, and `gpt-transcribe` for recorded descriptions. The race engine applies shared movement rules, resolves habits sequentially, records events, and determines the official ranking. AI prepares the introductions and routine lines, comments live on selected events, and explains the result from the log. During the race, browser speech synthesis can read routine lines when the narrator is free and selected live highlights while the event is still current. Players can mute speech and keep the captions. We have deferred the tool-calling race director because a fixed course gives it little to decide. Our initial API budget is $20, with $5 reserved for rehearsal and the final demo. Kelan will verify account access and measure latency and actual costs.

For the December 16 demo, one player races two computer-controlled pets on a fixed course lasting about 30 seconds. The race runs autonomously, with one timed cheer for the player. A dip toward a treat or a rapid rocking motion beside a scratching post makes a distraction visible even with sound muted. Fresh inputs and prepared examples lead into the same race, ranking, brief AI analysis, and replay. One course and reusable movement effects leave room to make character creation and commentary work well. Additional courses and same-device multiplayer are optional.

We use OpenAI Codex and Claude Code with AI-DLC v1.0.1. Kelan Mao operates one shared workflow. We meet in person weekly and use WeChat between meetings. We divide development by module, feature, and each member’s strengths. All five members discuss important decisions and review the artifacts together before approving a gate.

[Vision document](vision.md) · [Technical environment](technical-environment.md) · [Existing AI-DLC state](aidlc-docs/aidlc-state.md)
