# Requirements Verification

Choose one option per question and fill in each `[Answer]:` line. For an Other response, add a short explanation. If a course brief or team decision already settles any of these questions, include the file or link with your answer.

## Question 1

Which project should this repository deliver?

A) The LLM-driven virtual pet web app discussed earlier.

B) A different project the team has already chosen. Include its one-paragraph brief or a link to it.

C) Use this kickoff to compare project ideas before choosing one.

X) Other (please describe after [Answer]: tag below)

[Answer]: B) Draw & Dash is the team's chosen project. A player draws, types, or speaks to create a pet, confirms the AI's interpretation, and races two computer pets on one fixed course. The engine turns the pet's habits into visible behavior. AI commentary and a brief recap use the events that occurred. See the [approved vision](../../../vision.md).

## Question 2

If you chose the virtual pet, what should the first demo prove?

A) A user can interact with one on-screen pet. An LLM interprets an interaction, and the pet shows the result through its dialogue or behavior during the session.

B) A user can interact with one on-screen pet, and some pet state or memory persists when the user returns.

C) The team already has a different demo scope, or this question does not apply to the selected project. Include the demo goal if available.

X) Other (please describe after [Answer]: tag below)

[Answer]: C) Demonstrate a complete local race, from fresh or prepared input through clarification, race, ranking, recap, and replay. Preserve supplied artwork and generate a sprite when the player supplies no drawing. Show up to two habits taking effect in sequence, one timed cheer, and audible in-race commentary that the player can mute while gameplay continues.

## Question 3

What is the primary deliverable for this repository?

A) A course demo that runs on a team member's machine.

B) A browser demo that instructors or teammates can access remotely.

C) A deliverable specified in the course brief. Include the brief or its relevant requirements.

X) Other (please describe after [Answer]: tag below)

[Answer]: A) The December 16 product demo runs on a team laptop in a browser, with localhost serving the game and its API. It needs internet for OpenAI calls. The November 24 milestone consists of the team documentation and AI-DLC v1.0.1 Inception artifacts through Units Generation. See the [README](../../../README.md) and [technical environment](../../../technical-environment.md).

## Question 4

What inputs should the first demo be designed to handle?

A) Prepared examples and synthetic data for a team-run demo.

B) Free-form input from demo users, with no planned use of private datasets.

C) Real user records or an existing dataset. Describe the type of data, access constraints, and intended use without including the records themselves.

X) Other (please describe after [Answer]: tag below)

[Answer]: X) Both prepared team-created examples and fresh player input are required. Drawing, typing, and recorded voice each work alone and in combination. Players can edit the voice transcript and clarify conflicting details. Player submissions are session-only. The app discloses OpenAI processing before submission.

## Question 5

Should the AI-DLC Resiliency Baseline apply?

A) Yes. Use it during design to address fault tolerance, availability, observability, and recovery.

B) No. Skip this extension for the course prototype.

X) Other (please describe after [Answer]: tag below)

[Answer]: B) Off. A failed required API call can end the session with an explanation and a restart option. The team uses mocks for routine development and testing.

## Question 6

Should the AI-DLC Security Baseline apply?

A) Yes. Enforce the extension's security rules as blocking constraints.

B) No. Skip this extension.

X) Other (please describe after [Answer]: tag below)

[Answer]: B) Off. The agreed security basics still apply, including server-only credentials, input validation and size limits, microphone permission on use, and excluding raw player content from application logs.

## Question 7

Should the AI-DLC Property-Based Testing extension apply?

A) Yes. Enforce the full extension.

B) Partial. Apply it to pure functions and serialization round-trips.

C) No. Skip this extension.

X) Other (please describe after [Answer]: tag below)

[Answer]: B) Partial. Apply property-based testing to pure game calculations and data transformations, including applicable serialization round-trips. Examples include one-cheer consumption, the two-habit limit, and reproducibility with identical race inputs. Use fast-check with Vitest.

Under the relevant answer, add any confirmed course or team constraints that affect the first milestone, such as a deadline, required agent behavior, evaluation criteria, approved model or API access, cost limits, or accessibility needs.
