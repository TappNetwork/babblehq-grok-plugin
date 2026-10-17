---
name: babblehq-import-schedule
description: For BabbleHQ coaches and club admins, bring a season schedule CSV or a GameChanger stats CSV export into BabbleHQ. Always previews first, shows what would be created, and saves only after an explicit yes. Use when someone shares a schedule spreadsheet or a GameChanger export and wants it in BabbleHQ.
---

# Import a schedule or GameChanger stats

Uses the BabbleHQ connector: `list_my_organizations`, `preview_schedule_import`, `confirm_schedule_import`, `preview_game_changer_import`, `confirm_game_changer_import`.

A preview saves nothing. A confirm saves for real. Only the person who made a preview can confirm it, and a preview expires after 15 minutes. GameChanger comes in as a CSV export only. There's no live sync.

## Season schedule CSV

1. Get the club slug from `list_my_organizations`.
2. The CSV needs a header row. `team_slug`, `title`, and `starts_at` are required. Optional columns: `type` (`practice`, `game`, or `other`; blank becomes `practice`), `ends_at`, `arrives_at`, `opponent`, `location_name`, `location_url`, `uniform_color`, `rsvp_enabled`, and `description`. Use full ISO 8601 date-times with a time zone offset (for example `2026-04-11T09:00:00-04:00`) so times are not misread.
3. If the file the member gave uses different column names, map them to these, show the mapping, and ask before you continue. Don't invent rows, dates, or opponents.
4. Call `preview_schedule_import` with `organization` and `csv` (the CSV text).
5. Show the returned `events` as a compact table (date, time, team, type, title or opponent, location) with a count. Point out anything odd, such as games without an opponent or duplicate times.
6. Ask: "Create these N events?" Call `confirm_schedule_import` with `organization` and `preview_id` only after a clear yes.
7. If more than 15 minutes have passed, or the confirm says the preview is gone, run the preview again and show it again before confirming.
8. Report how many events were created. Confirming the same preview twice returns the same event ids and doesn't duplicate them.

## GameChanger stats CSV

1. Get the club slug and the team slug. Ask which team if it isn't clear.
2. Call `preview_game_changer_import` with `organization`, `team`, `csv`, and optionally `label` (such as "Spring season") and `filename`.
3. Show the `matched` players and, separately, the `unmatched` rows. Unmatched rows won't attach to anyone on the roster. Suggest that the member fix names or jersey numbers in BabbleHQ or in the CSV, then preview again.
4. Call `confirm_game_changer_import` with `organization` and `preview_id` only after a clear yes. The same 15-minute and same-person rules apply.

## Errors

- A parse error usually names the missing column. Say which one and how to fix it.
- "This action is unauthorized." means this account can't import for that club.
