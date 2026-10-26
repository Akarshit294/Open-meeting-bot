# CLAUDE.md — open-meeting-bot

## What this repo is

An open-source, self-hosted meeting bot for real-time AI agents, speaking a subset of
Recall.ai's bot API. `intent.md` says what we build and why. `plan.md` says what is done and
what is next.

Status: Phase 0 (research). There is no product code yet.

## Read first

1. `intent.md`
2. `plan.md`: the status table, the Decisions, the Do NOT list, and your task.

## Roles

The owner tells each session its role. If you were not told, ask.

**Planner**

1. Owns `plan.md`: writes task specs and keeps the status table, decisions, questions and log.
2. Reviews each coder's report and changes against the task's Acceptance.
3. Does not write the code of a task.

**Coder**

1. Takes one `ready` task from `plan.md` and builds only that task.
2. Works on the task's branch, named in its spec (`t<phase>.<n>-<slug>`, for example
   `t0.1-prior-art`).
3. Does not edit `plan.md`.
4. Does not commit until the planner has reviewed the work and the owner says so.
5. Ends with the report described at the end of `plan.md`. Anything found outside the task
   goes in the report, unfixed.

## Rules

1. **This repo is public.** Nothing from a private codebase or company: no copied code, no
   company or customer names, no server details, no keys. Everything here comes from public
   docs and our own tests.
2. **No secrets or sign-in data in git:** keys, `.env` files, browser profiles, cookies.
3. **No captured meeting audio or video in git.** Save captures under `captures/`, which git
   ignores. Test sound that we commit is generated (tones, text-to-speech).
4. **The bot is a visible, named guest that people let in.** It never hides, never acts as a
   person, and never gets around being let in.
5. **Evidence for platform claims.** Meet, Zoom and Teams change their pages without notice.
   A claim about how one behaves carries the date and the evidence: a log line, a capture, a
   screenshot.
6. **No destructive git commands** (`reset --hard`, `stash`, `clean`, `checkout -- .`). Another
   session may have work in the tree. Stage files by name, never `git add -A`.
7. **Third-party code.** Before copying any, check that its license works with Apache-2.0, and
   keep its copyright notice. Say so in the report.

## Layout

```
intent.md        what we build, and why
plan.md          tasks, status, decisions (the planner's file)
CLAUDE.md        this file
LICENSE          Apache-2.0
docs/research/   one note per research task (format in its README)
spikes/          throwaway Phase 0 scripts, one folder per task (made by the first spike)
captures/        local captures from test calls (git ignores it)
```

## Commands

None yet. The language and tools are chosen after T0.1 (decision D8 in `plan.md`). This section
is filled in then.

## Writing

Plain English and short sentences. Numbered steps for anything done in order. A note says what
was seen, with its evidence, before what it means.
