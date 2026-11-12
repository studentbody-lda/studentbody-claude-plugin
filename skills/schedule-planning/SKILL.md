---
name: schedule-planning
description: Plan and apply schedule changes on Student Body - holiday breaks, skipped dates, a new weekly class or series, a one-off session, a moved or cancelled session, a new term - showing the whole plan before anything is saved. Use when the person mentions holidays, férias, feriados, Christmas, Easter, August, a break, a new timetable, moving or cancelling a class, or adding a date or a series.
---

# Schedule planning

Schedule changes are where a good assistant earns trust: the person describes the change in a sentence, you
work out every session it touches, show the whole plan as one table, and save it only after they agree. Each
write tool needs `confirmed: true`, which you set only after the person has seen exactly what will be saved.

## How Student Body models a schedule

- A **class** (`list_classes`, `get_class`) is the thing students see. It is a draft until published.
- A **series** (`list_series`) is a repeating pattern on a class: one weekday, a time, a venue, a start date and
  an optional end date, with a pattern of `weekly`, `biweekly`, `monthly` or `nth_weekday` (with
  `weekOfMonth`, where -1 is the last one of the month). A class that runs on three weekdays has three series.
  Sessions are generated eight weeks ahead.
- A series keeps a list of **skipped dates** (`skip_series_dates`, `unskip_series_date`). A skipped date is
  never generated, and an existing unbooked session on it is cancelled. A date whose session has bookings
  **refuses the whole request**: nothing in that call is skipped. So a booked date is either cancelled first
  (`cancel_session`, a deliberate step the person chooses) and then skipped, or left out of the skip list and
  kept.
- A **session** is one dated occurrence (`get_schedule`, `create_session`, `update_session`,
  `cancel_session`). A one-off, an extra date, or an exception a series cannot express is a session on its own.
- `cancel_session` refunds and notifies every booked student and cannot be undone. It requires
  `expectedBooked`, the booking count the schedule showed moments ago; the server refuses a stale count.

## Steps

1. **Scope.** Call `whoami`. Confirm whose schedule (their own, or a studio they manage) and the date range.
   For a holiday break, confirm the first and last day off, inclusive, in the person's words ("from the 22nd to
   the 2nd" means both days).
2. **Read before you plan.** `list_classes`, then `list_series` for each class in scope, then `get_schedule`
   for the window. You need the series ids, the existing skipped dates, and the booking count on every
   session the plan touches. Public holidays in Portugal are in `references/portuguese-holidays.md`; ask
   before assuming a holiday closes the studio, and ask which country if the person is not in Portugal.
3. **Build the plan as one table.** One row per change: the class, the series or session, the date and time,
   what happens (skip, move, add, cancel), how many students are booked, and what students will be told.
   Group skips per series so each `skip_series_dates` call is visible as one row. Flag every booked session
   separately: "Tuesday 23 December has 4 students booked. Cancel and refund them, keep the class that day, or
   move it?" A kept booked date must stay out of the skip list, or the server refuses the whole skip.
4. **Show it, then stop.** Present the table and ask for a yes. If the person changes a row, show the table
   again. Do not save a partial plan because one row is undecided; settle every row first.
5. **Save in a safe order.** Non-destructive writes first: `create_series`, `create_session`, `update_series`,
   `update_session`. Then the cancellations the person chose, each with the `expectedBooked` you read in step
   2 and a reason students will read. Then the skips, with the kept booked dates left out. If any call fails,
   stop, report which rows were saved and which were not, and do not retry a cancellation on your own.
6. **Report.** Repeat the table with a saved / not saved column, and say what students were notified of.

## Common plans

- **Holiday break.** Every series in scope gets the dates inside the break; booked sessions are listed for a
  decision. Offer a status announcement on the profile or studio ("Classes pause 22 December to 2 January")
  through `update_my_profile` or `update_studio`, shown first like any text.
- **New term or a new weekly class.** A draft class if it does not exist (`create_class_draft`), then one
  `create_series` per weekday, then `publish_class` once the class passes its own check. A series on a draft
  class generates sessions students cannot see yet.
- **Move a class for a few weeks.** Prefer `update_session` on the affected sessions over changing the series,
  unless the move is permanent; a series change with `applyToFuture` moves everything ahead.
- **End a series.** `update_series` with an `endDate` keeps history and stops new dates. Clearing an end date
  later is not supported, so confirm the date.
- **Private lessons and appointments.** Appointment classes have open slots, not series; changing them is a
  console task. Say so and point to the console's Schedule page rather than improvising.

## What not to do

- Never set `confirmed: true` on a plan the person has not seen in full.
- Never put a booked date in a skip list the person has not decided on, and never pass an `expectedBooked`
  you did not read from `get_schedule` in this conversation.
- Do not create duplicate series: check `list_series` for an existing pattern on the same weekday and time.
- Do not guess a venue or capacity; the class's defaults apply when you omit them.
