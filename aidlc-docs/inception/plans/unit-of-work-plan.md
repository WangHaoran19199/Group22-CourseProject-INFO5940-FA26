# Draw & Dash unit of work plan

**Units Generation · plan approved November 24, 2026.** Group 22 selected 1A and approved the single-unit decomposition for generation. Group 22 separately approved the three generated artifacts on November 24, 2026.

## Approved boundary

Use **one formal unit of work for the Draw & Dash web game**. Its browser and loopback Express server deliver one playable round together. The browser owns the race and ranking. The server owns credentials, provider calls, and validation. A shared unit gives the team one place to verify a complete round, including what happens when a response arrives after replay.

Keep the seven approved components as logical modules. This leaves room for teammates to work in parallel without turning the engine, narration, or each model call into a separately deployed service. The main trade-off is shared integration responsibility. The story map should give each story one lead area and name the modules that contribute to its acceptance criteria.

## Question 1

Within the proposed game unit, how should the story map identify who leads each story through acceptance?

A) Group ownership by player outcome (recommended). Use three areas covering pet creation and review, race and session, and commentary and speech. For example, the commentary area leads the mid-race mute story through caption display, speech cancellation, and continued racing. UI and API contributors can support more than one area. Each story has one outcome to demonstrate, with some coordination across shared modules.

B) Group ownership by technical component. Give each story one lead from the seven approved components and identify the contributing components separately. For example, Speech can lead the mute story, with Commentary, UI, and Session contributing. This makes it easy to divide work by technical specialty. Cross-component stories require the lead to coordinate more handoffs before the player can verify the result.

X) Other (please describe after [Answer]: tag below)

[Answer]: A) Assign each story one lead area by player outcome: pet creation and review, race and session, or commentary and speech. List the contributing modules for that story.

## Approved coverage and coordination

The team selected the lead areas below for acceptance of the full player journey. Individual assignments remain with the team.

| Lead area | Stories to account for | Handoff worth checking together |
|---|---|---|
| Pet creation and review | US-01, US-02, US-03, US-04, US-05 | Fresh inputs and a bundled source-plus-draft reach the same review. Direct edits take effect locally. Session receives the confirmed card and matching original or generated artwork together. |
| Race and session | US-06, US-07, US-11, US-13, US-14 | Engine produces an ordered update for UI and Commentary. Session publishes the official ranking, resets the cheer on replay, and prevents an old completion from altering the new round after restart. |
| Commentary and speech | US-08, US-09, US-10, US-12 | Commentary checks a highlight when it arrives and before queued speech starts. UI retains captions on mute. The recap uses the final record for the race that actually finished. |

### Decomposition considerations

| Consideration | Evidence and treatment in the unit artifacts |
|---|---|
| Story grouping and team alignment | The team already chose journey-based stories and a strengths-based division of work. Q1 settles how acceptance ownership should appear in the map. Kelan operates the shared workflow. All five members review the gates. Individual feature assignments can wait. |
| Dependencies and integration | Use the [approved call matrix](../application-design/component-dependency.md). UI calls Session. Session calls Pet, Engine, and Commentary. Pet and Commentary call API. Commentary controls Speech. Record external boundaries separately for OpenAI and browser devices. With one unit, the formal matrix has no inter-unit edges. Show the internal handoffs alongside it so that integration work is visible. |
| Business domain | The reviewed card and sprite, ordered race events, and official finish record already define the handoffs between creation, racing, and narration. Reuse those contracts. Replay and failure recovery span them. |
| Technical constraints | The [technical environment](../../../technical-environment.md) specifies one localhost demo, in-memory player data, a server-only key, mocks for routine checks, and $5 of the $20 API budget reserved for rehearsal and demo. There is no separate deployment or scaling need for a feature. Actual account access, latency, spend, microphone capture, and audible playback still need live verification. |
| Greenfield code organization | Propose a single npm project with repository-root `src/` and `tests/`. Keep browser modules in `src/browser/`, Express and provider code in `src/server/`, and shared request and event types in `src/shared/`. Keep test fixtures separate from player submissions. Document the layout in the unit definition. The multi-service directory convention does not apply to this proposed boundary. |

**Integration order for review.** Agree on the confirmed-pet, race-update, and finish-record contracts first. A prepared pet and mocked responses can then exercise a whole round while teammates work on fresh inputs and live narration behind the same interfaces. Later integration checks should include a superseded overtake, mute during speech, and a required request that fails across restart. Exact habit parameters, cutoff times, and input limits remain with their designated later stages.

## Plan checklist

- [x] Load the approved design, [execution plan](execution-plan.md), [requirements](../requirements/requirements.md), [clarification record](../requirements/requirement-verification-questions.md), [stories](../user-stories/stories.md), and [persona](../user-stories/personas.md).
- [x] Evaluate story grouping, dependencies, team alignment, technical constraints, domain boundaries, and greenfield code organization.
- [x] Propose a unit boundary, check all 14 stories for coverage, and present the ownership question.
- [x] Record the team's answer in Q1, check it for ambiguity or conflict, and obtain explicit plan approval before generation.
- [x] Generate `aidlc-docs/inception/application-design/unit-of-work.md` with the approved unit boundary, responsibilities, module ownership, greenfield directory strategy, and verification scope.
- [x] Generate `aidlc-docs/inception/application-design/unit-of-work-dependency.md` with the unit matrix, internal module handoffs, external dependencies, and integration order.
- [x] Generate `aidlc-docs/inception/application-design/unit-of-work-story-map.md` with a unit, lead area or component, contributors, and an acceptance check for each approved story.
- [x] Validate the boundaries and dependency directions against the design. Check US-01 through US-14 for missing or duplicate primary assignments and preserve their approved FR/NFR links.
- [x] Present the [unit definition](../application-design/unit-of-work.md), [dependency matrix](../application-design/unit-of-work-dependency.md), and [story map](../application-design/unit-of-work-story-map.md) for separate team review.
- [x] Record the team's decision on the three generated artifacts.
- [x] After explicit unit artifact approval, stop before Construction and check milestone completeness and consistency against rubric criteria A through E. See the [final milestone check](../../audit.md#final-milestone-check).

**Team response.** Group 22 selected 1A and explicitly approved generation. Its reply is preserved in the [audit record](../../audit.md#units-generation--plan-approved).
