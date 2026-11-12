---
name: monthly-review
description: Review a teacher's or studio's month on Student Body and turn the Insights figures into a short, honest read with three things to try next. Use when the person asks how their classes, month, week, or season went, what to keep an eye on, whether a class is working, what students wanted but could not get, or asks for a report, balanço, resumo, or informe of their teaching.
---

# Monthly review

You are reading Student Body's Insights for the person who teaches or runs the studio. The figures come from
the connector's `insights_*` and `studio_*` tools. Your job is a reading they can act on, not a dashboard in
prose. Three rules sit above everything else:

1. **A null is "not measured", never zero.** A class booked on another system has no attendance until a host
   marked someone present. Say "we don't have attendance for Tuesday" rather than "Tuesday had 0".
2. **Name the base under every small number.** "Two of three students came back" is a fact; "67% returning"
   on three students is a story. Under ten, give the count, not the percentage.
3. **Counts, never names.** The tools return no student's name or contact, and you never guess at one.

Before the first figure, read the resource `studentbody://guidelines/insights`, which defines every term the
tools use. `references/figures.md` in this skill is the longer glossary, one definition per figure; open it
when a figure is ambiguous or the person asks what a number means.

## Steps

1. **Who and which month.** Call `whoami`. If the person belongs to a studio and has not said which hat they
   are wearing, ask once: their own teaching, or the studio. Default the month to the last complete calendar
   month; take any period they name.
2. **The month's own report first.** For a teacher, call `insights_monthly_report` with the month. It carries
   the lines that moved (followers, saves, sessions, bookings, fill, returning students, the strongest, cooling
   and looked-at windows, the opportunities acted on). For a studio, call `studio_insights` with the studio
   and the month's dates.
3. **Then the readings behind it, only as needed.** `insights_classes` ranks each class grow / maintain /
   opportunity with its reason. `insights_attendance` gives the weekday pattern with how many measured
   sessions each average rests on. `insights_want_to_join` and `insights_interest` say what students wanted
   and could not get. `insights_cohorts` says who came back. A teacher at several studios gets
   `insights_across_studios`. Studios have `studio_demand` and `studio_retention`. Do not call all of them by
   default: two or three readings that explain the month beat eight that repeat it.
4. **Mind the windows.** `insights_summary` holds two readings with different windows, labelled `overview`
   (all time unless you pass dates) and `narrative` (the most recent complete week only). Never add or compare
   figures across windows. When the narrative and the portfolio disagree, show both and say which numbers
   each rests on.
5. **Write the read.** Use the shape below. Plain words, the person's language, short paragraphs.
6. **Offer, do not act.** If a reading points at a change (add a Tuesday session, skip a dead August slot),
   describe it and offer to prepare it. Changes go through the schedule-planning skill and are always shown
   before they are saved.

## The shape of the read

**Headline** (one or two sentences): what the month was, in the terms the person uses. "A steady October:
six classes a week, the same students coming back, and Thursday evening still half empty."

**What moved** (three to five bullets): the lines that changed against the month before, each with its base.
Lead with what the person can influence. Followers and saves are interest; bookings and fill are money and
room; returning students are the health of the practice.

**What to watch** (one to three bullets): a class ranked "opportunity", a weekday whose measured sessions are
falling, a window students look at but do not fill, demand the person could not take. Say what the figure
rests on and what would make it clearer ("mark attendance on the Thursday class for a month and this
becomes measurable").

**Three things to try** (numbered): concrete, small, dated. "Open a second Tuesday 18:30 from 2 December;
four people are waiting." "Move Thursday to 19:00 for four weeks and compare." "Ask the Friday class to book
on Student Body so its fill is measured." Each should be something the connector can prepare or the person
can do in the console this week.

**Not measured this month** (one line, when it applies): what the figures could not see and why.

## What not to do

- Do not average over unmeasured sessions, read a missing privacy-floored cell as zero, or call a held-back
  count "no demand".
- Do not report revenue for a class that books elsewhere, or "attendance" for a class whose attendance was
  never marked: the glossary's "words to avoid" apply.
- Do not invent causes. "Fill dropped after the price change" needs the person to have told you about a price
  change. Offer hypotheses as questions.
- Do not pad. A month with little data gets a short read and a suggestion that makes next month measurable.
