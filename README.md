# BabbleHQ plugin

Run your club's schedule, RSVPs, and attendance from your assistant.

This plugin connects Grok Bot and Cursor to [BabbleHQ](https://babblehq.com) through BabbleHQ's hosted MCP server. Your assistant can see your clubs and upcoming events, RSVP for your player, see who hasn't answered, record attendance, and bring in a schedule or GameChanger stats. It acts as you and can only do what your account can do.

## Install

From the marketplace (after listing): search for **BabbleHQ**, choose **Install**, then sign in when asked.

To try it locally in Cursor, copy this folder to `~/.cursor/plugins/local/babblehq` and reload Cursor.

## Connect

The plugin adds one MCP server:

```json
{
  "mcpServers": {
    "babblehq": {
      "type": "http",
      "url": "https://babblehq.com/mcp"
    }
  }
}
```

Sign-in uses OAuth in the browser. When the assistant connects, BabbleHQ opens a sign-in page. Stay signed in to BabbleHQ in that browser and choose **Authorize**. If you use Google or Apple to sign in, you don't need a password.

If your assistant can't open a browser, you can create a token under **Connect an assistant** in BabbleHQ (the user menu) and set it as an `Authorization: Bearer …` header on the `babblehq` server in your own MCP config. Never paste a token into a chat.

To stop access, disconnect BabbleHQ in your assistant. Revoking a token in BabbleHQ stops that token, but it does not remove a browser approval.

## What it can do

| Who | Ask | Tools |
| --- | --- | --- |
| Any member | "What clubs am I in?" "What's on this week?" | `list_my_organizations`, `list_upcoming_events`, `get_event` |
| Parent | "RSVP yes for Saturday's game." | `rsvp` |
| Coach or admin | "Who hasn't responded for Saturday?" | `list_missing_rsvps` |
| Coach or admin | "Take attendance for tonight. Everyone's here but Sam." | `record_attendance` |
| Coach or admin | "Add a practice on Tuesday at 6." | `create_event` |
| Coach or admin | "Move Thursday's practice to 6:30." | `update_event` |
| Coach or admin | "Import this season schedule." "Import this GameChanger export." | `preview_schedule_import`, `confirm_schedule_import`, `preview_game_changer_import`, `confirm_game_changer_import` |

What the tools can change:

- `rsvp` saves an RSVP for you or for a player you're responsible for.
- `record_attendance` marks a player present, absent, late, or excused.
- `create_event` and `update_event` change the schedule. A change to an event's time, place, or title notifies the team.
- An import preview saves nothing. A confirm creates the events or saves the stats. A preview lasts 15 minutes, and only the person who made it can confirm it.

If the assistant says "This action is unauthorized.", your account isn't allowed to do that. The connection is still working.

## Skills

- `babblehq-week-and-rsvp`: your week at a glance, plus RSVP for your player.
- `babblehq-missing-rsvps`: who hasn't answered, plus a reminder draft you send yourself.
- `babblehq-take-attendance`: mark the roster at the field, and confirm before saving.
- `babblehq-import-schedule`: preview a season CSV or a GameChanger export, then save it only after you say yes.

## Try it

1. Connect the plugin and approve access in the browser.
2. Ask: "What do my BabbleHQ teams have this week?"
3. Check that the list matches your schedule in BabbleHQ, with times in your time zone.
4. Disconnect BabbleHQ and ask again. The assistant should say it isn't connected, not make up a schedule.

## Support

Visit [babblehq.com](https://babblehq.com).

## License

MIT
