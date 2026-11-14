---
name: onboarding-import
description: Bring an existing timetable onto Student Body - from a pasted schedule, a spreadsheet, a screenshot of a timetable, or a studio's website - as venues, draft classes and weekly series, then publish what is complete. Use when a new instructor or studio wants to add their classes, import or migrate a schedule, set up their timetable, or asks to "put my classes on Student Body".
---

# Onboarding import

A new teacher or studio arrives with a timetable somewhere else: a spreadsheet, a PDF, a screenshot of their
booking page, a message. Your job is to turn it into Student Body's shape (venues, classes, series) with as
few questions as possible, show the whole result before anything is saved, create everything as drafts, and
publish only what passes the server's own publish check. `references/class-fields.md` lists every field the
tools accept, with the allowed values, straight from the server's schemas; read it before you map columns.

Student Body connects directly to bsport, YOGO and Acuity from the console (`Settings → Sources`). If the
person uses one of those, say so first: a live connection keeps the schedule in sync, which an import cannot.
Use this skill for everything else, or when they want to start with a few classes by hand.

## Steps

1. **Who is importing.** `whoami`. A teacher imports their own classes; a studio member with the right
   capability imports the studio's: pass `studio` to `create_venue`, `create_class_draft` and `create_series`
   (the other writes scope through the class or session id). Then `list_my_venues` and `list_classes`
   so you never create a venue or class that already exists; `list_classes` also shows the class type keys
   already in use. For a teacher, `get_setup_checklist` shows where they are on the console's "Get set up"
   checklist (bio, classes, publish, schedule, go live); the import covers the middle three.
2. **Read the source.** Extract every distinct class with: name, weekday(s), start and end time, venue,
   teacher (studio imports), duration, level if stated, price if stated, how students book today. Resolve
   patterns: "every other Saturday" is `biweekly`; "first Sunday of the month" is `nth_weekday` with
   `weekOfMonth` 1. Keep the person's class names as written; do not add adjectives.
3. **Ask the few things a timetable never says**, in one message, with sensible defaults offered:
   - How students book: on Student Body (`native`), on their own page (`link`, with the URL), or by message
     (`contact`, with the WhatsApp number or email). Default to what they do today.
   - The price students see for a class that books elsewhere (`set_class_display_price`, in euros; 0 means
     free). A class that books on Student Body takes its price in the console's Payments settings, not here.
   - Level (`BeginnersWelcome`, `AllLevels`, `Intermediate`, `Advanced`) and capacity when the source is
     silent. Offer `AllLevels` and ask for the room's size.
   - The class type key for anything that is not obvious; the list is in `references/class-fields.md`.
4. **Show the plan as tables.** Venues to create (name, address, city, setting). Classes to create (name,
   type, duration, level, booking method, price, venue). Series per class (weekday, time, pattern, start
   date, end date if any). Anything you could not read, marked as a question. Wait for a yes; revise and show
   again if they change anything.
5. **Create in order, each with `confirmed: true` after the yes.** Venues (`create_venue`; the server
   geocodes the address and dedupes by name, address and city), then classes (`create_class_draft` with
   every field you have; the answer lists what is still missing), then series (`create_series`, one per
   weekday), then display prices for classes that book elsewhere.
6. **Publish what is complete.** For each draft, call `publish_class` without extra fields first; it refuses
   with a labelled list when something is missing. Fill what you can from the source and ask for the rest.
   Report which classes are live and which stay as drafts, and why.
7. **Hand over.** For a teacher, call `get_setup_checklist` again and say what is left, step by step, with
   its console link. A missing bio you can draft (`update_my_profile`, under the copy guidelines,
   `studentbody://guidelines/copy`); a studio's description the same way (`update_studio`). Photos, the
   Payments settings for classes that book on Student Body, and going live are the console's: going live is
   the person's own switch, so never say it is done until the checklist shows it.

## Mapping rules

- One class, several weekdays: one class, one series per weekday. The same name at two venues: two classes,
  unless the person says it is one class that moves.
- A workshop or retreat with a date: `isEvent: true` and a `create_session` for its date, no series.
- Private lessons or 1:1 slots: `isAppointment: true`; slots are set in the console, not as series. Say so.
- Descriptions: use the source's text as `shortDescription` if it is one or two sentences; otherwise draft
  under the copy guidelines and mark it as a draft the person should read. Never invent credentials,
  outcomes or student numbers.
- Languages: set `instructionLanguages` from the source or the person's answer, and `contentLanguage` to
  the language the texts are in.

## What not to do

- Do not publish a class the person has not seen as it will appear.
- Do not create venues, classes or series that `list_my_venues` or `list_classes` already show; reuse them as
  they are. Do not propose changes to an existing venue or class (its setting, its name, its texts) unless the
  person asks: the import adds what is missing and leaves the rest alone.
- Do not set a booking method the person did not confirm: `native` makes a class bookable and paid on Student
  Body, which has consequences they must choose.
- Do not import students, bookings or payments: the connector has no tools for them by design.
