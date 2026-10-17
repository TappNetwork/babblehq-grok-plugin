---
name: babblehq-take-attendance
description: For BabbleHQ coaches and club admins, record who showed up to a practice or game, marking each player present, absent, late, or excused. Use when a coach at the field says "take attendance", "everyone's here but Sam", or "mark Jordan late".
---

# Take attendance

Uses the BabbleHQ connector: `list_my_organizations`, `list_upcoming_events`, `get_event`, `record_attendance`.

`record_attendance` is staff-only and writes one player at a time.

## Steps

1. Find the event. "Tonight" or "today" means the event on today's date in the coach's time zone. Use `list_upcoming_events` with `from` and `to` set to that day. If more than one event fits, ask which.
2. Call `get_event`. Check `can_take_attendance`. If it's false, stop and say this account can't take attendance for that event.
3. The `attendance` list is the roster: `roster_spot_id`, `player_name`, `jersey_number`, and the current `status`.
4. Turn what the coach said into a status for each player: `present`, `absent`, `late`, or `excused` (`unknown` clears a mark). "Everyone but Sam and Riley" means everyone else is `present`. If a name matches more than one player, or matches none, ask before writing anything.
5. Before writing, show the plan in one short list (for example "14 present, Sam absent, Riley late") and get a yes. Batches are easy to get wrong at the field.
6. Call `record_attendance` once per player with `organization`, `event_id`, `roster_spot_id`, `status`, and an optional `notes` (up to 1000 characters, such as "left at 7:15").
7. Report what was saved and list any failures by name.

## Don't change the event from here

If the coach also says practice moved or starts later, don't call `update_event` as part of attendance. A change to the time, place, or title notifies the whole team. Confirm the exact change and get a clear yes first, and treat it as a separate step.
