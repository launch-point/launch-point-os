# Sales Ops Daily Brief

**Schedule:** Monday–Friday at 9:00 AM
**Workstation:** Revenue Ops
**Mode:** Scheduled — runs after the CRM snapshot task
**Prerequisite:** `Scheduled/crm-snapshot-task.md` must 
complete before this task starts. If the snapshot task 
failed or the cache is stale, flag it in the Opening and 
proceed with the cached data — never skip the brief.

## On Wake

Load Revenue Ops CLAUDE.md and all Resources files before 
fetching any data. Check that `Cache/sales-crm-cache.xlsx` 
exists and has a Last Refreshed timestamp from today. If 
the timestamp is from a prior day, flag: "⚠️ CRM cache 
may be stale — snapshot task may not have completed. 
Proceeding with last known data from [timestamp]."

Then execute Block 1 in full before stopping.

---

## Block 1 — Fetch and Deliver (all at once)

Run these fetches in parallel where the tools allow.
Deliver the complete block in a single response.

1. **Opening** — Pull today's calendar (sales calls,
   follow-up calls), check Circle for wins in last 72
   hours, check Stripe for new sales in last 24 hours,
   pull QuickBooks MTD. Write 3–5 sentences: number of
   calls today, wins, follow-ups due, MTD revenue, any
   notable pipeline movement.

2. **Today's Sales Calls** — Calendar events matching
   "Quick Call with Todd" scheduled for today only.
   Exclude any event with "Onboarding" in the title —
   those are client onboarding calls, not sales calls.
   For each qualifying event: pull Calendly routing
   form answers, Kondo DM history, Quo SMS history,
   Kit.com subscriber record if exists, Sales CRM record
   from cache. Deliver full prep card per call. If no
   sales calls today, omit this section entirely — do
   not list upcoming calls from other days.

3. **Today's Follow-Up Calls** — Calendar events with
   "Follow Up with Todd" in title scheduled for today
   only (format: "Follow Up with Todd and [Name]" —
   capital U, no hyphen). Search Google Drive Meet
   Recordings for original transcript (Granola as
   backup). Per call: original call date, objections
   from transcript, any conversation since via
   Kondo/Quo/Gmail, suggested focus. If no follow-up
   calls today, omit this section entirely.

4. **Pipeline Movement — Last 24 Hours** — Compare
   today's cache against yesterday's Last Edited Time
   values. Any record updated since yesterday's brief
   is pipeline movement. Write conversationally. Omit
   if nothing changed.

5. **Client & Alumni Wins — Last 72 Hours** — list_posts
   from Add Your Interviews (space 2289335) and
   list_space_ai_summaries from Share Your Wins (space
   1640485). Classify each win. For every win without
   exception, confirm whether the person has a record
   in Client CRM or Alumni CRM during the parallel
   fetch — complete this lookup before delivering the
   brief, never flag it as pending or unconfirmed in
   the delivered output. Log the win as a page comment
   on whichever record holds them — never both, never
   neither. Surface action and channel per win. For
   Job Offer wins, confirm this hasn't already been
   congratulated/testimonial-asked before treating it
   as new.

6. **New ICP Quiz Subscribers — Last 24 Hours** —
   list_subscribers from Kit.com sorted by created_at
   descending. Score each against icp-rules.md. Show
   only 4/5 or better. For each qualifying subscriber,
   confirm whether they are already in the Sales CRM
   cache before delivering the brief — complete this
   lookup before delivering, never flag it as pending.

7. **Revenue Snapshot** — MTD net income from QuickBooks.
   Flag any new Stripe sale in last 24 hours.

8. **Workshop Invite List** — Only if a "Ministry to
   Marketplace" event is within 2 days on calendar. Pull
   all Call Offered, Not Enough Money, Not Yet prospects
   from the cache with preferred channel. Omit entirely
   if no upcoming workshop.

---

## Block 2A — Lead Pipeline (Interactive, cache + Notion writes)

Read all prospect data from `Cache/sales-crm-cache.xlsx`.
Write all updates directly to Notion AND update the
corresponding row in the cache immediately after each
confirmed write. Work through items one at a time. Wait
for a response before moving to the next item.

9. **Post-Call Debrief** — Check calendar for any "Quick
   Call with Todd" or "Follow Up with Todd" events from
   yesterday with no outcome logged in the cache (Pipeline
   Stage unchanged from pre-call state). Present numbered
   outcome list:

   1. Client! — they bought
   2. Waiting On Decision
   3. Follow Up Call needed
   4. Not Enough Money
   5. Going a Different Direction
   6. Trying on their own
   7. Spouse Not On Board
   8. Still Discerning
   9. In Interviews
   10. Stay In Ministry
   11. Got A Job
   12. Not a fit — my call
   13. No show / cancelled

   On Todd's reply: update Pipeline Stage and Reason
   for "no"? per the mapping table in
   notion-crm-schema.md. Update Last Interaction to
   yesterday. Log context in Follow-up Notes. Ask for
   Next Check-In only for Not Yet (no specific reason) /
   Not Enough Money / Waiting On Decision / Follow Up
   Call. Auto-advance Call Offered cadence without asking.
   Write to Notion immediately. Update cache row
   immediately after Notion write confirms.

10. **Missing and Overdue Next Check-In Dates — Sales CRM**

    Read from cache. Surface ALL records in these stages
    that meet either condition:

    **Condition A — Missing:** Next Check-In is blank
    **Condition B — Overdue:** Next Check-In date is
    in the past as of today

    Stages to check: Call Offered, Waiting On Decision,
    Not Enough Money, Not Yet

    **Call Offered exceptions — do not surface:**
    - Records where Date SC Offered is more than 50 days
      ago AND Next Check-In is blank — blank is correct
      for post-Day-50 prospects
    - Records where Date SC Offered is more than 50 days
      ago AND Next Check-In is stale — clear the date
      in Notion and cache without asking Todd

    **Open with a full summary table** of everyone being
    surfaced this session:

    | Name | Stage | Next Check-In | Days Overdue / Missing |
    |---|---|---|---|

    Then work through them one at a time. For each:
    - Pull last 1–2 interactions from Kondo/Quo/Gmail
    - Show day count from Date SC Offered (Call Offered
      records only)
    - Surface: "[Name] is in [Stage] — Next Check-In
      is [missing / X days overdue]. Last touch was
      [channel, date]: [one-line summary]. When do you
      want to check back in?"
    - On Todd's reply: write Next Check-In to Notion
      immediately. Update cache row immediately after
      Notion confirms.

    **Never leave this section incomplete.** Every record
    in the summary table must be resolved before moving
    to Block 2B.

---

## Block 2B — Client & Alumni Touchpoints (Interactive, Client CRM + Alumni CRM)

Work through these one at a time. Wait for a response
before moving to the next item. Write Notion updates
immediately on each reply.

11. **Pending Job Start Dates** — Client CRM or Alumni CRM
    records with Client Status = ALUMNI/OFFER and no Job
    Start Date. One at a time: "[Name] got their offer —
    what's their start date?" On reply: write Job Start
    Date to **both** Client CRM and Alumni CRM records
    for that person, then calculate Next Check-In =
    Job Start Date + 90 days (advance to Monday if
    weekend) and write it to the **Alumni CRM** record
    only.

12. **Missing Next Check-In Dates — Alumni CRM** — All
    Alumni CRM records with no Next Check-In date set
    or Next Check-In date in the past. Open with summary
    table, then work one at a time. Surface: "[Name] has
    no Next Check-In date set / Next Check-In was
    [date] — when do you want to check back in?" Write
    to Notion immediately on reply. This never applies
    to Client CRM — that database has no Next Check-In
    field.

---

## Block 3 — Todo List (deliver all at once after Block 2B)

Read from cache. Pull all prospects due for a touch today
based on cadence day count from Date SC Offered. Pull all
alumni due for a touch today from Alumni CRM. Client CRM
records are not included. Combine into a single table —
prospects first, then alumni.

Columns: Name | Type | Stage/Status | Reason | Channel |
Last Contact

If Todd replies with names: draft outreach using
Resources/voice-principles.md as the guide. Present draft
and wait for explicit approval before sending. Do not send
anything without approval. After sending is confirmed:
update Last Interaction in Notion AND cache immediately.

---

## Monday Addition

On Mondays, append the weekly brief after Block 3:
- Last week's numbers (prior Mon–Sun): quiz takers,
  ICP-qualified leads with names, Calendly forms
  submitted, sales calls booked, follow-up calls,
  Stripe sales
- This week's calls and outreach priorities
- MTD net income from QuickBooks
