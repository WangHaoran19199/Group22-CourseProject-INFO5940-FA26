# Draw & Dash — Technical environment

*Approved by the team · November 24, 2026. The team has selected the stack versions, frontend, API budget and reserve, mock-based testing strategy, and general speech playback policy.*

## Local runtime and deployment

The demo runs on a team laptop. A browser handles drawing, player controls, race simulation, animation, and spoken commentary. A local Express server validates submissions, calls OpenAI, and returns results. OpenAI is the hosted model provider. The application needs internet access for live AI features. During development Vite serves the frontend and proxies application API requests to Express. For the demo, Express can serve the built frontend and API from one localhost origin. Browser sessions hold pet cards, sprites, and race logs in memory. Prepared examples are bundled team-created assets.

| Component | Selected version | Role |
|---|---|---|
| Node.js / npm | 24.22.3 LTS / bundled 11.19.1 | Shared runtime, package manager, and eventual npm lockfile |
| TypeScript / Vite | 7.0.2 / 8.3.8 | Strict TypeScript, browser development server and build |
| HTML Canvas, native DOM and browser speech synthesis | Browser APIs | Drawing and sprites, forms and dialogs, spoken captions |
| Express | 5.3.0 | Local server and OpenAI boundary |
| Vitest / fast-check | 5.1.1 / 4.11.0 | Unit tests and generated tests for calculation and data invariants |
| Playwright Test | 1.65.1 | Browser tests for drawing, review, race, and results |

The exact packages installed together under Node 24.22.3 with strict engine checks in an isolated macOS test. TypeScript compiled a sample, Vite built it, Express registered a route, and Vitest ran a fast-check property. Playwright opened local Chrome and checked a 2D canvas, a modal dialog, and the speech synthesis interface. Audible playback and microphone capture still need a check on the demo laptop. [Node release](https://nodejs.org/en/blog/release/v24.22.3) · [Vite requirements](https://vite.dev/guide/) · [Vitest requirements](https://vitest.dev/guide/) · [Playwright requirements](https://playwright.dev/docs/intro)

### Frontend

The team selected vanilla TypeScript with Vite and HTML5 Canvas. The frontend uses the native DOM without React. Keep the current screen, draft pet, confirmed card, and race phase in explicit session state. Forms, the editable transcript, the confirmation dialog, and results can use ordinary HTML controls. The drawing canvas receives pointer input and supports both a stroke eraser and a clear-canvas action. A separate animation loop draws the race from simulation state.

## OpenAI services and budget

Kelan Mao owns access verification. The team set an initial API budget of $20, including $5 reserved for rehearsal and the final demo. These are documented model IDs and standard US-dollar rates checked November 24. A paid request on the team account, actual latency, and observed cost per race are pending.

| Model | Planned endpoint and capabilities | Published rate used for planning |
|---|---|---|
| `gpt-6.1-terra` | `POST /v1/responses`. Accepts text and image input, returns text, supports structured outputs and function calling. Use a validated pet-card schema for interpretation, then short text for commentary and recap. | $1 per million input tokens and $5 per million output tokens. [Model](https://developers.openai.com/api/docs/models/gpt-6.1-terra) |
| `gpt-image-2.6-flare` | `POST /v1/images/generations`. Generate one static transparent PNG from confirmed appearance only when no drawing was supplied. Start with a low-quality 1024 × 1024 square, a common right-facing silhouette, flat colors, and an ink outline. Scale it proportionally on the track. | $5 per million text-input tokens, $8 per million image-input tokens if used, and $30 per million image-output tokens. [Model](https://developers.openai.com/api/docs/models/gpt-image-2.6-flare) · [Image guide and calculator](https://developers.openai.com/api/docs/guides/image-generation) |
| `gpt-transcribe` | `POST /v1/audio/transcriptions`. Send a short recorded clip as multipart form data and display the returned transcript for correction. | $0.0045 per audio minute. [Model](https://developers.openai.com/api/docs/models/gpt-transcribe) · [File transcription](https://developers.openai.com/api/docs/guides/speech-to-text) |
| `gpt-6-luna` | Optional substitute for commentary on `POST /v1/responses`. Supports text/image input, structured outputs, and function calling. Compare its factual accuracy and timeliness on saved synthetic race events before switching. | $0.10 per million input tokens and $0.50 per million output tokens. [Model](https://developers.openai.com/api/docs/models/gpt-6-luna) |

### What one complete round could cost

The estimate assumes two bundled computer racers. It allows up to five Terra calls covering interpretation, one batch for the opening and routine lines, up to two live highlights, and the required recap. Across those calls, assume **5,000 billable input tokens** (including any drawing input) and **700 billable output tokens** (including any billed reasoning). That is **$0.0085**. For a generated sprite, an illustrative 200-token text prompt and 1,000 billable image-output tokens add **$0.031**. A 15-second recording adds about **$0.0011**. Browser narration adds no OpenAI speech charge. Actual image tokens depend on dimensions, quality, and generation settings. [Vision input calculator](https://developers.openai.com/api/docs/guides/image-cost-calculator) · [Image generation costs](https://developers.openai.com/api/docs/guides/image-generation)

| Complete session | Estimated OpenAI charge |
|---|---:|
| Original drawing, optionally with typed text, all race commentary and recap | $0.0085 |
| Fresh text-only pet, one generated sprite, commentary and recap | $0.0395 |
| Fresh voice-only pet, 15 seconds of audio, one generated sprite, commentary and recap | $0.0406 |

One hundred voice-created rounds under these assumptions would cost about **$4.06**. If an image consumed 4,000 output tokens instead, those same 100 rounds would cost about **$13.06**. Corrections that require a new call, rerolls, retries, or longer outputs increase spending. Kelan should compare one real round per input path against these assumptions before broad live testing.

The team will use mocks for routine development and testing and manage live usage within the agreed budget.

- Use fixed synthetic responses, generated sprites from team-owned examples, and a fake speech interface for everyday development and automated tests. Run a small live check separately to assess model quality and timing.
- Reuse the confirmed sprite on replay and on habit-only corrections. Generate after appearance confirmation. Bundle both computer pets. The cost estimate assumes up to two live highlights per race. Keep one request in flight at a time for live highlights and suppress duplicate submissions. Keep the final recap as its own required request.
- Hold $5 of the $20 for rehearsal and the demo. Track model, token counts, audio duration, error category, and aggregate spend without logging player content. Stop discretionary paid tests when the development allocation is reached. Budget alerts can supplement this check.

## Commentary and playback

Use the race log for action, ordering, and time. The model can describe a logged overtake and the action preceding it. The engine determines official ranking. Each caption carries the race identity, event time, and priority. A routine reaction selects its prepared AI line immediately. A lead change around a habit, an effective two-habit sequence, or a close finish can trigger a live line. Discard stale output after replay or when the event has been superseded.

Provide a persistent **Sound on / Mute** control from the start screen. Use the player’s Start interaction to initiate enabled audio. Display text immediately and speak short lines one at a time. Keep at most one pending highlight, replacing it with a more important or fresher event. Speak a routine line during the race when the narrator is free. If speech is busy, show its caption and skip its audio. A selected live highlight can play as soon as the narrator is free if its event is still current. At the finish, clear race chatter, show the official ranking, then read the recap when it arrives. Muting stops current and queued speech immediately. Unmuting starts with the next eligible line. Reset speech and outstanding captions on restart. If a device has no usable voice, keep captions visible and show a sound status. Verify the intended voice and mute behavior with the demo laptop’s speakers. [Browser speech cancellation](https://developer.mozilla.org/en-US/docs/Web/API/SpeechSynthesis/cancel)

## Security, data, and dependencies

- Bind the local app to loopback. Put the OpenAI key in a server-only environment variable. Keep secrets out of source control, the browser bundle, and all client-visible configuration.
- Tell players which selected inputs go to OpenAI before submission. Request microphone permission only when recording starts, stop the microphone tracks afterward, and let the player edit the transcript. Proposed initial limits are 15 seconds of audio, 2 MiB per recording, and 2 MiB per normalized drawing. Enforce format and size on the server. Choose final limits after trying the target browser’s recording format.
- Keep application data in memory and discard player media and transcripts when the session ends. Keep raw inputs and generated content out of logs. Render AI text as plain text. Validate model output against allowed traits and game effects. Request `store: false` for Responses calls. OpenAI’s endpoint and account retention policies still apply to submitted content. [OpenAI data controls](https://developers.openai.com/api/docs/guides/your-data)
- On a failed required API request, explain which operation failed and offer restart. Resiliency Baseline and Security Baseline are off by team choice. The security practices above still apply. Property-Based Testing is partial for core game calculations and data transformations.

| Prohibited library or usage proposed for this MVP | Reason | Alternative |
|---|---|---|
| OpenAI SDK in the browser with a project API key | A player could retrieve the shared credential from client code or requests. | Route browser requests through the local Express server to OpenAI. |
| Persistent browser-storage libraries such as `localforage` for player submissions | Persisting drawings, audio, or profiles beyond the session conflicts with the retention decision. | Keep player data in memory. Bundle separately authored demo fixtures. |

## Examples for the future implementation

The field names and habits below are examples. The initial habit catalog remains open for detailed game design. During Construction, the team will finalize field names and layouts using the approved component interfaces. [Requirements](aidlc-docs/inception/requirements/requirements.md#scope-boundaries-and-open-decisions) · [Component methods](aidlc-docs/inception/application-design/component-methods.md)

**Typical endpoint.** A text-only creation request can use `POST /api/pets/interpret`. The Express handler validates the input, requests a structured interpretation, and returns a reviewable draft before sprite generation. A drawing-only request would carry a validated image. A voice-only request supplies the player-reviewed transcript.

```http
POST /api/pets/interpret HTTP/1.1
Content-Type: application/json

{"description":"Mochi stops for food and chases any friend who passes her."}
```

```json
{"name":"Mochi","suggestedHabits":["food-motivated","chases-friends"],"clarifications":[]}
```

**Typical function and unit test.** A pet can be running one movement effect while another habit becomes eligible. After the current effect ends, the engine can recheck the queued chase against current positions. This pure decision can be tested without making an API request.

```ts
export function canStartChase(
  activeEffect: "pause" | "slow" | "burst" | null,
  rivalAhead: boolean,
): boolean {
  return activeEffect === null && rivalAhead;
}
```

```ts
import { expect, test } from "vitest";
import { canStartChase } from "./habits";

test("Mochi can chase after her snack if her rival is still ahead", () => {
  expect(canStartChase("pause", true)).toBe(false);
  expect(canStartChase(null, true)).toBe(true);
  expect(canStartChase(null, false)).toBe(false);
});
```

Use fast-check to vary legal pet cards and action sequences and check one-cheer consumption, bounded habit counts, and repeatability under identical inputs. Use Playwright with mocked provider calls to cover each independent creation method, combined-input clarification, original artwork preservation, captions and mute, recap, and restart after failure. A separate live rehearsal should exercise model access, microphone permission, audible speech, and actual API cost.
