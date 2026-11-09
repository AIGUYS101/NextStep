# NextStep

**Big words. Clear next steps.**

An accessibility-focused CTRL-V hackathon prototype. NextStep helps people understand confusing letters and messages, identify supported actions and dates, and prepare an editable reply.

## For hackathon judges

Live prototype: https://nextstep-access.vercel.app/

The implementation claims in the submission can be reviewed directly:

| Claim | Source | What to inspect |
| --- | --- | --- |
| Strict structured AI output | [server/schema.ts](server/schema.ts) and [server/ai.ts](server/ai.ts) | `json_schema`, `strict: true`, required fields and `additionalProperties: false`; the schema is passed to the provider request. |
| Server-side source-quote check | [server/validation.ts](server/validation.ts) | `validateAnalysis` checks each supported action quote against the submitted text, after Unicode, whitespace and case normalisation. A quote that cannot be matched is rejected. |
| Deadline handling | [server/validation.ts](server/validation.ts) | A displayed deadline must occur within its quote; unsupported dates are omitted and flagged. This does not guarantee the AI interpretation is correct. |
| Failure recovery | [api/analyze.ts](api/analyze.ts) | One bounded retry for malformed or ungrounded output. Missing optional dates do not discard the explanation; incomplete individual actions are omitted with a visible warning. |
| 22 regression tests | [tests/validation.test.ts](tests/validation.test.ts) | Source grounding, malformed actions, null dates, request validation, strict schema configuration and mocked AI handler paths. |
| Photo-to-text review | [api/extract.ts](api/extract.ts) and [src/main.tsx](src/main.tsx) | A photo is transcribed first; the user can edit the extracted text before requesting an explanation. |

### Reproduce the checks

With Node.js 22 or later installed, run these commands from the folder containing `package.json`:

```sh
npm ci
npm test
npm run build
```

**No API key is needed for the 22 automated tests.** Provider requests in handler tests are mocked. They test implementation behaviour, not real-world model accuracy. Live analysis requires a server-side OpenAI API key and is separate from the test suite.

The source package contains no production API key or uploaded user documents. `.env.example` lists the configuration names only.

## Included

- Paste text or use the device clipboard normally.
- Upload a JPEG, PNG or WebP photo, drag and drop, or request a phone camera using a native capture input. Desktop browsers may show a file picker.
- Client-side resize/compression before sending images.
- Photo transcription with an explicit editable review before analysis.
- AI plain-English explanation, action checklist, source quotes, questions and reply draft.
- Server checks that every action's quote occurs in the input text and that each deadline occurs in its quote. This verifies quoted text, **not the correctness of the interpretation**.
- Browser read aloud, larger text, contrast toggle, reduced-motion support, keyboard controls, focus indicators, labelled inputs and a skip link.
- Clearly labelled, fictional example with prepared output, available without a key.
- No document database, document logging or automatic reply sending. No analytics.

## Deploy to Vercel

1. Extract this folder and put its contents into a new GitHub repository (keep `package.json`, `vercel.json`, `src/`, `server/` and `api/` at the repository root).
2. In Vercel, choose **Add New → Project**, import the repository and use the **Vite** framework preset. If you keep a containing `nextstep/` folder, select that as the Root Directory.
3. Build command: `npm run build`. Output directory: `dist`. Install command: `npm ci`. Node.js: 22 or later.
4. Add **OPENAI_API_KEY** as a sensitive environment variable for Production (and Preview if used). Do not paste the key into source code, chat or a `VITE_` variable. The OpenAI account needs API billing/credit; a ChatGPT subscription does not supply API credit.
5. Optionally add **OPENAI_MODEL=gpt-4.1-mini**. The chosen model must support images and Chat Completions strict JSON Schema output.
6. Deploy. If a key is added or changed later, redeploy.
7. Open the Vercel URL. Try the prepared example, then paste a fictional letter to test live AI. Finally upload `public/example-letter.jpg` and check its transcription before generating the explanation.

Alternatively, from this folder with Node.js installed:

```sh
npm ci
npx vercel
```

Follow Vercel's login/project prompts, set the environment variable in the project settings, then deploy to production with `npx vercel --prod`.

## Local development

```sh
npm ci
npm run dev
```

Vite alone serves the interface and prepared example, **not** the `/api` functions. For live local API testing, install/run Vercel CLI with `npx vercel dev`; link your project and retrieve environment variables using Vercel's normal workflow. Keep all `.env*` secret files untracked.

```sh
npm test
npm run build
```

## Architecture

React + TypeScript + Vite frontend. Vercel Node.js serverless functions call OpenAI using a server-only key. Browser → `/api/extract` → editable text → `/api/analyze` → explanation. Server output validation rejects missing fields and unsupported source quotes; date badges not present in their quote are omitted and flagged for checking. One automatic retry handles malformed or ungrounded model responses; a second invalid response is still rejected. Native speech synthesis handles read aloud; voice availability and processing depend on the device.

`server/` contains shared code outside `api/`, so only the two intentional API handlers become functions. API responses use `Cache-Control: no-store`. A same-origin browser check is included but is **not** authentication or a rate limiter.

## Verification performed

- TypeScript checks and production bundling pass.
- 22 automated tests: source grounding, invented quotes, unsupported deadlines, malformed output, text/image limits, image signatures, methods/origins, missing key, and mocked provider success/failure paths.
- Browser checks: prepared example, source quote expansion, checklist ticks, editable reply and clipboard copy, image selection and preview.
- Live OpenAI text analysis and photo extraction have passed on the public Vercel deployment using fictional documents. Browser testing also confirmed a live explanation renders with actions and an editable reply.
- Mobile camera capture, device voices, screen readers and real-user accessibility testing still need verification. Responsive CSS is implemented; no formal WCAG conformance claim is made.

## Prototype limits and responsible deployment

Use fictional documents for judging. Users explicitly consent before live content is sent to OpenAI. The app does not deliberately store documents or log request bodies, but providers may retain API data and hosting request metadata under their own policies. This is not a zero-retention guarantee.

A public production service needs durable rate limiting or Vercel Firewall rules and an appropriate AI budget. The prototype has neither authenticated users nor a durable abuse limiter. Set provider spend controls before sharing widely. A privacy review, accessibility audit and representative user testing are needed before use with sensitive real documents.

AI can misread photos and misinterpret wording. The app asks users to review text, shows original quotes and flags uncertainty; it does not replace professional advice. Relative dates are copied, not calculated. HEIC, PDFs and multiple pages per request are not supported. Translation is not included in this build.

## Submission materials

See `SUBMISSION.md` for draft copy and a short demo script. Replace deployment placeholders and only claim live AI demonstration once verified.

AI output now uses strict JSON Schema with required action fields. A missing optional deadline is treated as no date. An incomplete suggested action is omitted with a visible warning while preserving the useful summary. Photo review now has an explicit “Explain this letter” button.

Latest regression scope: 22 automated checks cover strict provider schema, omitted/null dates, information-only letters and incomplete action recovery. The latest browser photo-upload check could not be completed because browser upload permission was declined; this is not evidence that the application upload failed.
