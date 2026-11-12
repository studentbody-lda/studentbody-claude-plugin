# Student Body for Claude

Student Body is a booking and discovery app for movement classes in Portugal. This plugin gives Claude four
skills for the instructors and studios who use it, on top of the Student Body connector, so you can run your
classes from the assistant you already talk to.

## What it does

- **Monthly review.** Ask how your month went. Claude reads your Insights through the connector and writes a
  short, honest read: what moved, what to watch, and three things to try, with every small number given as a
  count rather than a percentage, and "not measured" where Student Body has no figure.
- **Schedule planning.** Describe a holiday break, a new term, a moved class or a one-off date. Claude works out
  every session it touches, shows the whole plan as one table, and saves it only after you agree. Booked
  sessions are always listed for your decision and never cancelled as a side effect.
- **Help desk.** Ask how anything works. Claude answers from the published instructor and studio guides, with
  the exact console path, in English, Portuguese or Spanish.
- **Onboarding import.** Paste a timetable, a spreadsheet or a screenshot. Claude maps it to venues, draft
  classes and weekly series, shows the result, creates it, and publishes only what passes Student Body's own
  check.

## Use it

Install the plugin from the Claude directory (claude.ai → Customize → Plugins), then connect the Student Body
connector from the plugin's Connectors tab and sign in with your Student Body account. Claude applies the skills when a task fits; you can also ask for one by name, for
example "run my monthly review" or "help me plan the Christmas break". In Claude Code the skills appear as
`/studentbody:monthly-review`, `/studentbody:schedule-planning`, `/studentbody:help-desk` and
`/studentbody:onboarding-import`.

Every change is shown to you before it is saved. The connector never returns a student's name, contact or
payment details, and has no tools for money, exports or team invites.

## Data

The skills send your requests to Claude as usual and read and write your own Student Body data through the
connector at mcp.studentbody.pt, signed in as you. The plugin stores nothing itself and sends nothing to any
other service. The guides and field references bundled in the skills are copies of Student Body's public
documentation. Student Body's privacy policy: https://www.studentbody.pt/en/privacy

## Support

support@studentbody.pt, or the guides at https://www.studentbody.pt/en/guides.
