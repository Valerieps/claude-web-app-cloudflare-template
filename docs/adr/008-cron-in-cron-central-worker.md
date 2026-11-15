# ADR-008: The cron lives in the `cron-central` Worker

**Status:** Accepted
**Date:** 2026-11-15
**Decision:** The app's `wrangler.jsonc` has no `triggers.crons`. `server/entry/worker.ts` exports
`runCron(env, ctx)` (calls `Group.tick()`) and `class Cron extends WorkerEntrypoint<Env>` whose
`tick()` calls it. The `cron-central` Worker (repo `Valerieps/cron-central`) holds the one schedule
and calls `Cron.tick()` through a service binding. To add an app: add it to `JOBS` and to `services`
in `cron-central`.
**Why:** Cron triggers per Worker are limited on the Workers Free plan; one Worker that fans out
keeps every app's schedule in one place. Each job still decides whether it is due, so the call
frequency can change without touching the app.
**Alternatives:** A `triggers.crons` entry per app (hits the account's trigger limit).

Supersedes the cron part of ADR-004.
