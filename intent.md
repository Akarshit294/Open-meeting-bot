# open-meeting-bot — intent

## In one line

An open-source, self-hosted meeting bot for real-time AI agents. It joins a video call as a
named guest, shows a web page as its camera and voice, presents a web page as a screen share,
and streams each person's audio back, live. It speaks a subset of Recall.ai's bot API, so a
client can switch to it by changing one base URL.

## Why

1. An AI agent in a call must be seen, be heard, and hear each person, while the call is
   happening. That needs live media in both directions, not a recording.
2. Hosted bot APIs such as Recall.ai do this well, but they are closed and priced per bot-hour.
3. The open-source bots we know of focus on recording and transcripts. Task T0.1 checks this.
4. This project builds the parts an agent needs, from Recall.ai's public docs and our own
   tests. No code comes from Recall.ai.

## What it does (v1)

1. **Join.** Open a meeting link, join as a guest with a given name, and wait to be let in.
   Report when it is let in, refused, removed, or when the call ends.
2. **Camera and voice from a web page.** Load a page the client names. What the page draws is
   the bot's camera. What the page plays is the bot's voice. Inside that page, the microphone
   (`getUserMedia`) gives the call's mixed audio, as on Recall.ai.
3. **Present a web page.** Share a second page as a screen share when asked, and stop when
   asked.
4. **Each person's audio, live and separate.** Raw audio for each person, with the person's id
   and name, over a WebSocket.
5. **Who joined and who left.** An event for each join and leave, with the person's id and
   name.
6. **Bot status.** Joining, waiting to be let in, in the call, call ended, done, failed — by
   API and by webhook.

## The API subset

The same paths, request bodies and event names as Recall.ai's bot API, for these parts only:

| Recall.ai API | Used for |
|---|---|
| `POST /api/v1/bot/` — `meeting_url`, `bot_name`, `output_media.camera`, `recording_config.realtime_endpoints` | 1, 2, 4, 5 |
| `GET /api/v1/bot/{id}/` (`status_changes`) and `GET /api/v1/bot/?status=…` | 6 |
| `POST /api/v1/bot/{id}/leave_call/` | 1 |
| `POST` and `DELETE /api/v1/bot/{id}/output_media/` (`screenshare`) | 3 |
| WebSocket event `audio_separate_raw.data` | 4 |
| Webhook events `participant_events.join` and `participant_events.leave` | 5 |
| Bot status webhook events (`bot.*`) | 6 |

The exact payloads are taken from Recall.ai's public docs in Phase 2 (see `plan.md`).

## Not in v1

1. Recording, transcripts, calendars, and data after the call.
2. Each person's video.
3. A hosted service. This is software you run yourself.
4. Zoom and Teams. Google Meet comes first. The others follow once Meet works.

## Principles

1. **The bot is always a visible, named guest that people let in.** It never hides, never
   acts as a person, and never gets around being let in.
2. **Evidence over assumptions.** Meeting platforms change their pages often. Every claim
   about how a platform behaves carries the date and the evidence: a log line, a capture, a
   screenshot.
3. **One bot is one browser.** A crash or a stuck call affects one bot only.
4. **Measure the cost.** CPU, memory and delay for each bot are measured, not guessed. For
   reference, Recall.ai's larger bot (`web_4_core`) has 2.25 CPU cores and 5.25 GB of memory,
   with camera and screen share at 1280x720, 15 fps.

## How we know v1 works (on Google Meet)

1. A client of Recall.ai's bot API (the subset above) works with this project by changing
   only its base URL and API key.
2. The bot joins with the given name, a person lets it in, and every status change is
   reported.
3. People in the call see the camera page and hear its sound.
4. The client gets each person's audio as a separate stream with the right name, within a
   delay target set in Phase 0.
5. The client is told when each person joins and leaves.
6. A page is presented and stopped when asked.
7. It runs from one Docker image, with a known CPU and memory cost for each bot.

---

Not affiliated with Recall.ai. "Recall.ai" names the API this project is compatible with.
