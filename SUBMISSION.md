# NextStep — CTRL-V submission draft

## Short description

NextStep turns confusing letters into plain-English explanations and manageable next steps. Paste text, upload a photo or use your phone camera, then review what it means, check quoted evidence and prepare a reply.

## The problem

Important information is often buried in complicated letters. People facing literacy or cognitive barriers, unfamiliar terminology or digital exclusion can struggle to understand what is being asked of them. A letter may technically be accessible while the action it requires is still hard to find.

## Why it matters

Understanding is part of access. If someone cannot make sense of a course offer, service notice or request for information, they may struggle to act on it. We aim to reduce that burden while keeping the user in control. These are intended benefits; we have not yet measured outcomes with representative users.

## How it works

1. Paste text, upload a document photo or use a phone camera.
2. For photos, AI transcribes the text. The user reviews and corrects it before continuing.
3. AI produces a plain-English explanation and small action steps.
4. Each action links to a quote from the source. The server checks the quote exists and any labelled deadline is contained within it.
5. The user can tick steps, read the explanation aloud and edit/copy a draft reply. Nothing is automatically sent.

The prototype includes readable controls, larger text, a contrast toggle, keyboard navigation, visible focus and reduced-motion support. There is no account requirement. Source checks add a way to inspect the explanation; they do not guarantee that AI interpretation is correct.

## Technologies used

React, TypeScript, Vite, Vercel Node.js Functions (deployment target), OpenAI Chat Completions with a vision-capable model (default gpt-4.1-mini), browser Canvas for image preparation, Web Speech API for text-to-speech, Lucide icons and Node.js tests.

## Innovation

NextStep combines photo-to-text review, practical action planning, inspectable source quotes and reply drafting in one simple journey. Instead of stopping at a summary, it helps users move towards their next action. Uncertainty and the original wording stay visible.

## Technical status — update before submitting

Implemented: frontend, two serverless API handlers, quote/date validation, accessible controls and prepared example. Production build and 22 automated tests pass. Browser checks cover the example flow, evidence, checklist, reply editing/copy and photo preview.

Deployed publicly on Vercel with the API key configured securely. Live text analysis and photo transcription have been verified with fictional documents. The prepared example remains deliberately labelled and is not a live AI response. The API retries once if an AI response fails source validation, then rejects any response that still fails.

## Project link

https://nextstep-access.vercel.app/

## Demo script (about 2 minutes 30 seconds)

0:00–0:20 — “Being sent information is not the same as being able to use it. A confusing letter can become a barrier to a course, a service or a next step. We built NextStep to help make that information usable.”

0:20–0:45 — Show paste, upload and camera options. “You can paste the text or use a photo. With a photo, NextStep reads it first and gives you a chance to check the words.” Use `public/example-letter.jpg` for a live photo walkthrough only once the API is connected. Otherwise explicitly call the prepared flow a demonstration.

0:45–1:15 — Show the summary and action list. “Now we can see what it means and what to do next. The language is simple, and the actions are small.”

1:15–1:40 — Open an evidence quote. “Every action points back to the original wording. The server rejects quotes it cannot find; dates absent from their quote are omitted and flagged for checking. This does not eliminate AI mistakes, but it gives the user something they can check.”

1:40–2:05 — Tick an action, change text size and demonstrate read aloud if device speech works. “You can take it one step at a time, choose larger text or listen to the explanation.”

2:05–2:25 — Edit/copy the reply. “When it is time to respond, start with an editable draft. You check it and choose where to send it.”

2:25–2:40 — “NextStep is a working prototype aimed at making understanding part of access. Our next step is testing it with the people it is designed to help.”

## Before pressing submit

- Confirm the hackathon's deadline and eligibility requirements.
- Deploy, configure the AI key and check both real text and photo flows with fictional content.
- Make the project accessible to judges and test the public link in a signed-out browser.
- Record the demo and upload it to the submission's required destination.
- Replace status/link placeholders and avoid unsupported claims about retention, accuracy, measured impact or WCAG compliance.
