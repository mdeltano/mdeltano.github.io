+++
title = "Discord Attendance Bot for Large Communities"
description = "Discord bot that reads voice channel membership and logs attendance to a Google Sheet via batched API calls. Handles 100+ users per event across a 13-event weekly schedule."
weight = 3
date = 2025-12-01

[extra]
tags = ["javascript", "nodejs", "discord.js", "google-sheets-api"]
+++

## Overview

A Discord bot for a large community that needs a recurring, repeatable
record of who showed up to scheduled voice-channel events. It scans the
current state of designated voice channels, matches each user against a
roster stored in a Google Sheet, and writes a "1" into the column
corresponding to that event — turning what used to be manual roll call
into a single slash command.

The bot is built on `discord.js` v14 and the official Google Sheets
API, and currently tracks attendance across 13 distinct weekly events.

## What I built

- **`/attendance` slash command** with autocomplete over the configured
  event list. Operators just type `/attendance` and pick the event;
  the bot does the rest.
- **`/addevent`, `/removeevent`, `/renameevent`** — runtime
  configuration of the event-to-sheet-column mapping, so admins can
  add a new weekly event without touching the code or restarting from
  source. Adding an event automatically shifts every column to its
  right (including across the `Z → AA` boundary).
- **`/dumpusers`** — diagnostic command that lists every user
  currently in the tracked voice channels who *isn't* in the
  spreadsheet roster. Catches the "new member joined but nobody added
  them to the sheet" case before it silently drops their attendance.
- **Permissioned execution.** Every command checks that it was invoked
  from the approved channel ID before running, so the bot can live in
  a server without exposing admin actions to general users.

## Architecture

- **Command auto-loader.** [index.js](https://github.com/mdeltano/attendance_bot_js) walks the
  `commands/` directory at startup, registers each module's slash
  command with Discord via `deploy-commands.js`, and routes
  `InteractionCreate` events — both chat input and autocomplete — to
  the matching handler.
- **Event/column mapping** lives in [events.json](https://github.com/mdeltano/attendance_bot_js),
  a flat JSON file mapping human-readable event names ("Sunday KRA
  Linebattle") to sheet column letters ("V"). Keeping it as JSON means
  the `/addevent` command can rewrite it atomically and the next
  invocation picks up the new state.
- **Google Sheets integration** uses a service account
  (`credentials.json`) and the `google-auth-library` to authenticate,
  then reads column D of the roster sheet to build a `user_id → row`
  lookup. Per-event attendance writes are accumulated into a single
  `spreadsheets.batchUpdate` call instead of one request per user —
  critical for staying under Sheets API rate limits when an event has
  100+ attendees.
- **Voice channel enumeration** filters all guild channels by parent
  category ID and channel type, then fetches each channel's member
  cache to collect attendee user IDs in one pass.
- **Timeout hardening.** Wraps the Sheets fetch in an `undici` agent
  with a 60-second connect timeout, since the default tripped
  intermittently against the Sheets API under load.

## Design tradeoffs worth calling out

- **Column letter math.** The `/addevent` command shifts every column
  to the right of the inserted one — including the `Z → AA` rollover —
  with hand-rolled charCode arithmetic. It's not glamorous code, but
  it lets admins reorder the event list without ever touching a Sheets
  URL or a config file.
- **Batched writes over per-user writes.** The naive version sent one
  API call per attendee; under real load that hit quota errors and
  partial successes. Collecting all updates into one `batchUpdate`
  request fixed both reliability and latency.
- **Stateless command modules.** Each command is a self-contained
  module exporting `data` and `execute`, which keeps the bot trivial
  to extend — adding a new command is a new file in `commands/utility/`
  and a restart, with no central dispatcher to update.

## Outcome

Replaced a manual, per-event roll-call process with a single slash
command. The bot reliably tracks attendance for 100+ users across 13
recurring weekly events, with admin tooling that lets the community
manage its own event schedule without developer involvement.
