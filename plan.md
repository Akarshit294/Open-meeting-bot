# open-meeting-bot — plan

This file belongs to the planner session. Coders read it and do not edit it.

## How to use

1. Read `intent.md` first: what we build and what we leave out.
2. The status table below is the truth about what is done and what is next.
3. A task can be started when its status is `ready`. A `ready` task has a full spec below.
   Tasks marked `spec later` wait for what earlier tasks find out.
4. Coder: take one `ready` task. Build only that task, on its branch. When done, write the
   report (format at the end of this file) and stop.
5. Planner: review the report and the changes against the task's Acceptance, update this file,
   and write the spec for the next task.
6. Nothing is committed until the planner has reviewed it and the owner says so.

## Status

| Task | What | Status | Depends on |
|---|---|---|---|
| T0.1 | Read the open-source meeting bots | ready | — |
| T0.2 | Join a Meet call as a guest | spec later | T0.1 |
| T0.3 | Each person's audio out of Meet | spec later | T0.2 |
| T0.4 | A web page as camera and voice | spec later | T0.2 |
| T0.5 | Present a web page | spec later | T0.2 |
| T0.6 | Who joined and who left | spec later | T0.2 |
| T0.7 | Test calls without people | spec later | T0.2, Q1 |

## Decisions (settled)

| # | Decision | Date |
|---|---|---|
| D1 | The name is `open-meeting-bot`. | 2026-10-26 |
| D2 | The license is Apache-2.0. | 2026-10-26 |
| D3 | The repo is local only. No remote until the owner says so. | 2026-10-26 |
| D4 | The API is a subset of Recall.ai's bot API: same paths, bodies and events, so a client switches by changing its base URL. | 2026-10-26 |
| D5 | Google Meet first, then Zoom, then Teams. | 2026-10-26 |
| D6 | The bot is a visible, named guest that people let in. | 2026-10-26 |
| D7 | Not in v1: recording, transcripts, calendars, data after the call, each person's video. | 2026-10-26 |
| D8 | The language and browser library are chosen after T0.1. If nothing argues otherwise: TypeScript and Playwright. | 2026-10-26 |

## Do NOT (settled — reopen only with the owner)

1. Do not put anything from a private codebase or company in this repo: no copied code, no
   company or customer names, no server details, no keys. Everything here comes from public
   docs and our own tests.
2. Do not commit secrets or sign-in data: keys, `.env` files, browser profiles, cookies.
3. Do not commit captured meeting audio or video. Save captures under `captures/`, which git
   ignores. Test sound that we commit is generated (tones, text-to-speech).
4. Do not make the bot hide, act as a person, or get around being let in.
5. Do not build what D7 leaves out.
6. Do not run destructive git commands (`reset --hard`, `stash`, `clean`, `checkout -- .`).
   Another session may have work in the tree.

## Open questions (for the owner)

1. **Q1 — A Google account for tests.** Can you make a Google account only for testing? It
   would host test calls and let the bot in, so agents can test without you. Needed by T0.7.
   Until then, the owner hosts the test calls of T0.2 to T0.6 and lets the bot in.

## Phase 0 — Learn (no product code)

Goal: find out how a browser bot does each of the six things in `intent.md` on Google Meet,
and what it costs, before we design anything. The scripts of this phase are throwaway (one
folder per task under `spikes/`). What matters is the note each task writes in
`docs/research/`.

Riskiest first: after joining (T0.2), each person's audio (T0.3) comes next. If it fails, the
fallback is one mixed stream, and that changes what v1 can promise.

### T0.1 — Read the open-source meeting bots

- **Status:** ready
- **Branch:** `t0.1-prior-art`
- **Depends on:** nothing
- **Why:** Others have built parts of this. How they join Meet and how they get each person's
  audio can save weeks, and it decides our language and browser library (D8).
- **Read:**
  1. Attendee — https://github.com/attendee-labs/attendee
  2. Vexa (Apache-2.0) — https://github.com/Vexa-ai/vexa
  3. ScreenApp's open-source meeting bot. Find its repo from ScreenApp's post
     https://screenapp.io/it/blog/recall-ai-alternative-open-source-meeting-bot (the post
     says the bot can stand in for Recall.ai's API; check that claim).
  4. Any other active open-source meeting bot you find on the way. Say why it matters.
- **Do:** For each project, answer these. Give a source for each answer: a file path and line,
  or a link to its docs. Write "not found" when the source does not show it.
  1. License, main language, date of the last commit.
  2. Platforms, and how the bot joins each one: browser automation (which library) or a
     native SDK.
  3. Meet: does the bot join as a guest or signed in? How does it tell that it was let in,
     refused, removed, or that the call ended?
  4. Meet audio: one mixed stream, or one per person? How is a stream matched to a person's
     name? What audio format does it send out?
  5. Camera and voice: can a web page, or any stream, be the bot's camera and microphone? How
     is it done?
  6. Screen share: can the bot present a page or a stream? How?
  7. Who joined and who left: how does the bot know?
  8. Running: one browser per bot? One container per bot? CPU and memory per bot, if stated.
  9. API: REST endpoints, webhooks, WebSockets. Is any part shaped like Recall.ai's API?
  10. Known problems: open issues about Meet page changes, bots blocked or removed, audio
      problems.
- **Output:** `docs/research/T0.1-prior-art.md`, with:
  1. One section per project, with the ten answers.
  2. A comparison table.
  3. A recommendation, with reasons: build on one of them, borrow parts, or start fresh; and
     which language and browser library.
- **Acceptance:**
  1. Every answer has a source or says "not found". No guess is written as a fact.
  2. Answers 3 and 4 (Meet joining and Meet audio) are given for every project that supports
     Meet. They decide the rest of Phase 0.
  3. If the recommendation reuses code, it names the exact files and their license.
- **Tests:** none. This is research.
- **Do not:** copy code into this repo; run any of these bots against a real call.

### T0.2 — Join a Meet call as a guest (spec later)

A script opens a Meet link, sets the name, turns camera and microphone off, asks to join, and
logs each state: waiting, let in, refused, removed, call ended. The owner hosts and lets it in.

### T0.3 — Each person's audio out of Meet (spec later)

In the joined page, find how Meet delivers audio (how many streams, how each maps to a person),
and save each person's audio to its own file, labelled with their name. Measure the delay.
Fallback if one stream per person is not possible: one mixed stream.

### T0.4 — A web page as camera and voice (spec later)

Give Meet a page's picture and sound in place of a camera and microphone (`getUserMedia`).
People in the call see the page and hear it. Measure the delay.

### T0.5 — Present a web page (spec later)

Give Meet a second page's picture as the screen (`getDisplayMedia`), start presenting, and stop
when asked.

### T0.6 — Who joined and who left (spec later)

Read who is in the call, with names and an id that stays the same, and log each join and leave
as it happens.

### T0.7 — Test calls without people (spec later)

A test host account (Q1) that hosts calls and lets the bot in, and guest browsers that play
known audio, so agents can run T0.2 to T0.6 alone.

## Phase 1 — One bot on Meet (spec after Phase 0)

One bot process. Given a meeting link, a name, a camera page and where to send events, it does
the six things in `intent.md` on Google Meet.

## Phase 2 — The Recall.ai-compatible API (spec after Phase 1)

Take the exact payloads from Recall.ai's public docs. Build the HTTP API, the webhooks and the
WebSockets around the Phase 1 bot. One browser per bot.

## Phase 3 — Run many bots (spec after Phase 2)

A Docker image, start time, CPU and memory per bot, limits, logs.

## Phase 4 — Zoom and Teams (later)

## Report format (coders)

1. Task id and branch.
2. What you did, in steps.
3. Evidence: commands and their output, log lines, numbers, screenshots.
4. Files changed.
5. Acceptance: each line of the task's Acceptance, met or not, with its evidence.
6. Found but not fixed: anything outside the task.
7. Questions for the planner.

## Log

- 2026-10-26 — Repo started, local only. `intent.md`, `plan.md`, `CLAUDE.md` and `LICENSE`
  written. T0.1 is ready.
