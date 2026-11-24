# AI-DLC State Tracking

## Project Information

- **Repository** — Group22-CourseProject-INFO5940-FA26
- **Project type** — Greenfield
- **Start date** — 2026-11-21T21:06:20Z
- **Current stage** — Inception complete. Workflow stopped before Construction.
- **Last completed stage** — Units Generation, approved by Group 22 on November 24, 2026.
- **Milestone review** — The [final A–E check](audit.md#final-milestone-check) is complete. Group 22 approved Inception and authorized publication to GitHub. Canvas submission is a separate team action.

## Workspace State at Kickoff

- **Existing application code** — No
- **Programming languages and build system** — None detected
- **Project structure** — README and AI-DLC setup files
- **Reverse engineering needed** — No
- **Workspace root** — `C:\Users\Wang_Haoran\Desktop\Cornl\1-1\AIapplRAG\PROJ\Group22-CourseProject-INFO5940-FA26`

## Requirements Analysis

- **Intent** — Deliver Draw & Dash, a casual pet-racing web game with player-created pets and event-based AI commentary.
- **Request type and estimated scope** — New greenfield project spanning pet creation, race simulation, and narration.
- **Depth and complexity** — Standard depth, moderate complexity.
- **Approved inputs** — The team approved the [README](../README.md), [vision](../vision.md), and [technical environment](../technical-environment.md) on November 24, 2026.
- **Verification** — The [current clarification record](inception/requirements/requirement-verification-questions.md) consolidates the team's confirmed decisions and distinguishes deferred design work from unverified runtime facts. The [earlier seven answers](history/requirements/verification-at-resumption.md) are preserved as recorded at resumption.

## User Stories

- **Assessment** — [Execute User Stories](inception/plans/user-stories-assessment.md) for the player-facing creation, racing, and narration journeys.
- **Planning** — The team approved the [plan](inception/plans/story-generation-plan.md) on November 24, 2026, selecting player-journey organization and one casual-player persona across five creation situations. Design parameters and runtime checks remain open.
- **Generation** — The [14 user stories](inception/user-stories/stories.md) cover six parts of a round and map to [one casual-player persona](inception/user-stories/personas.md). Each story has observable acceptance criteria. The requirement index covers FR-01 through FR-09 and NFR-01 through NFR-06.
- **Artifact approval** — Group 22 approved the stories and persona on November 24, 2026, and authorized Workflow Planning. See the [audit record](audit.md#user-stories--artifact-approval).

## Execution Plan Summary

The [approved execution plan and workflow diagram](inception/plans/execution-plan.md#workflow-visualization) cover 14 named stages. All six selected Inception stages are complete. Group 22 approved the three Units Generation artifacts. Reverse Engineering was skipped for the greenfield project.

The separate Construction forecast selects Functional Design, NFR Requirements, NFR Design, Code Generation, and Build and Test. It skips Infrastructure Design because the approved local runtime needs no provisioned environment. Operations is a v1.0.1 placeholder. The November milestone stops after explicit Units Generation approval and a rubric check. Starting Construction requires separate authorization.

## Extension Configuration

| Extension | Enabled | Decided At |
| --- | --- | --- |
| Resiliency Baseline | No | Requirements Analysis, [current Q15](inception/requirements/requirement-verification-questions.md#question-15) |
| Security Baseline | No | Requirements Analysis, [current Q16](inception/requirements/requirement-verification-questions.md#question-16). Team security basics still apply. |
| Property-Based Testing | Partial | Requirements Analysis, [current Q17](inception/requirements/requirement-verification-questions.md#question-17). Core game calculations and data transformations, including applicable round-trips. |

## Stage Progress

### Inception

- [x] Workspace Detection
- Reverse Engineering skipped for the greenfield repository.
- [x] Requirements Analysis
  - [x] Validate verification answers, generate requirements for review, and reconcile the consolidated clarification record
  - [x] Receive explicit team approval of this requirements document
- [x] User Stories
  - [x] Record stage assessment and prepare the story generation plan
  - [x] Record and review both planning answers
  - [x] Obtain explicit plan approval
  - [x] Generate stories and persona, check coverage, and present them for review
  - [x] Receive explicit team approval of the generated stories and persona
- [x] Workflow Planning
  - [x] Check the approved inputs and assess scope, impact, and risk
  - [x] Document stage choices, workflow diagram, estimated timing, and review gates
  - [x] Record the team's execution plan approval
- [x] Application Design
  - [x] Prepare the checklist, propose the component handoffs, and present the two design questions
  - [x] Record and validate team answers on card corrections and prepared examples, and confirm the plan
  - [x] Generate and validate the five Application Design artifacts
  - [x] Obtain explicit team approval of those artifacts
- [x] Units Generation (artifacts approved)
  - [x] Prepare the decomposition plan, review the approved design dependencies, and present the ownership question
  - [x] Record and validate the team’s answer and obtain explicit plan approval
  - [x] Generate and check the unit definition, dependency matrix, and story map
  - [x] Obtain explicit team approval of the unit artifacts

### Construction forecast

- [ ] Functional Design, NFR Requirements, and NFR Design for applicable units
- Infrastructure Design proposed for skip with localhost deployment
- [ ] Code Generation per unit
- [ ] Build and Test across units

## Handoff for the next context

Group 22 approved the [unit definition](inception/application-design/unit-of-work.md), [dependency matrix](inception/application-design/unit-of-work-dependency.md), and [story map](inception/application-design/unit-of-work-story-map.md). U-01 contains all 14 stories. The team explicitly stopped the workflow before Construction.

The team resolved the timing discrepancy identified in the [final milestone check](audit.md#final-milestone-check). The [correction record](audit.md#technical-environment-correction-and-publication-authorization) links the updated examples to the approved requirements and method contracts. The repository branch for the milestone is `main`. Construction requires a separate instruction.

Keep application code in the repository and AI-DLC documentation under `aidlc-docs/`.
