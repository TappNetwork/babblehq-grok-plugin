---
name: babblehq-missing-rsvps
description: For BabbleHQ coaches and club admins, list the rostered players who haven't RSVPed for an upcoming practice or game and draft a short reminder the coach can send. Use when someone asks who hasn't responded, who's coming, or wants to nudge families about an RSVP.
---

# Chase missing RSVPs

Uses the BabbleHQ connector: `list_my_organizations`, `list_upcoming_events`, `get_event`, `list_missing_rsvps`.

`list_missing_rsvps` is staff-only. BabbleHQ has no messaging tool, so this skill drafts the reminder and the coach sends it from wherever the team already talks.

## Steps

1. Get the club slug from `list_my_organizations` and find the event with `list_upcoming_events` (narrow with `team`, `type`, `from`, and `to`). If more than one event fits, ask which.
2. Call `list_missing_rsvps` with `organization` and `event_id`. It returns the players with no RSVP (`first_name`, `last_name`, `player_id`, `roster_spot_id`).
3. Call `get_event` for the counts so far (`rsvp_counts`) and anyone who answered no or maybe, if the coach wants the full picture.
4. Report it in one block: event and time, yes/no/maybe counts, then the names still missing.
5. Offer a reminder draft. Keep it to two or three sentences: the event, the time and place, and a request to RSVP in BabbleHQ. Use first names only, and don't shame anyone. For example:

   > Quick reminder for Saturday's 9:00 game at North Field: if you haven't RSVPed yet, please do it in BabbleHQ today so we can set the lineup. Thanks!

6. Tell the coach the draft isn't sent. They send it themselves.

## Don'ts

- Don't RSVP on a family's behalf to close the gap.
- Don't promise texts, emails, or push reminders from BabbleHQ. The connector can't send messages.
- If the tool says "This action is unauthorized.", the member isn't staff on that team. Say so plainly.
