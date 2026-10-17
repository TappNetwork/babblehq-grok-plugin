---
name: babblehq-week-and-rsvp
description: Show a BabbleHQ member what their clubs have coming up and RSVP yes, no, or maybe for their player. Use when someone asks what's on this week, when the next practice or game is, where to be and when, or wants to RSVP for their kid.
---

# Week view and RSVP

Uses the BabbleHQ connector: `list_my_organizations`, `list_upcoming_events`, `get_event`, `rsvp`.

The connector acts as the signed-in member. It only sees the clubs, teams, and events that member can see.

## Show the week

1. Call `list_my_organizations`. Each club has a `slug`; every other tool takes that slug as `organization`.
2. If the member is in more than one club and didn't say which, check each club rather than guessing. Leave out clubs with nothing in range.
3. Call `list_upcoming_events` with `organization`, plus `from` and `to` as ISO 8601 times for the range they asked about. Default to today through the next 7 days in the member's time zone. Add `team` (team slug) or `type` (`practice`, `game`, or `other`) only if they asked for it.
4. List events by day. For each one show the title, type, team, start time, arrive-by time (`arrives_at`) if set, opponent, location, and uniform color. Show times in the member's local zone, not UTC. Show the member's own RSVP (`my_rsvp`) if there is one.
5. Keep it short. Don't invent details that aren't in the event.

## RSVP for a player

1. Find the event. If it's ambiguous ("Saturday" with two games), ask which one.
2. Call `get_event` with `organization` and `event_id`. For a parent, the `attendance` list holds only their own players, with `player_id` and `player_name`. If they have more than one player on that team, ask which one, or whether the answer is for all of them.
3. Confirm the event, player, and answer back in one line, then call `rsvp` with `organization`, `event_id`, `player_id`, `response` (`yes`, `no`, or `maybe`), and an optional `comment` (up to 255 characters, such as "Running 10 minutes late").
4. Staff RSVPing for themselves can leave out `player_id`.
5. If `rsvp_enabled` is false on the event, say RSVPs are off for that event and don't call `rsvp`.
6. Report the saved answer from the response.

## Errors

- "This action is unauthorized." means this account isn't allowed to do that. A coach can't RSVP for a player they aren't responsible for. Tell the member plainly; the connection is still working.
- If the connector isn't connected or sign-in has expired, say so and ask the member to reconnect BabbleHQ. Don't say anything was saved.
