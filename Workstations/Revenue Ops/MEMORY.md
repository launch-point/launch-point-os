# Revenue Ops Memory

## Key People

| Name | Role | Notes |
|---|---|---|
| Taylor Linder | Co-owner | Finances and strategic decisions |
| Tara | EA | Manages inbox, calendar, ClickUp |
| Michelle | VA | |
| Adrienne | Resume Writer | Add-on service |
| Ricardo | Business mentor | Communicates via email only |

## CRM Configuration

- **Sales CRM Database ID:** 31d15d96-e4cc-80cb-ba9e-fc72290995c1
- **Sales CRM Data Source ID:** 31d15d96-e4cc-80d8-9772-000baea28015
- **Client CRM Database ID:** 31d15d96-e4cc-80d1-9782-c7919bac9c62
- **Client CRM Data Source ID:** 31d15d96-e4cc-80d7-88df-000be89d2aa5
- **Alumni CRM Database ID:** 37415d96-e4cc-805f-b810-c7871f8310b6
- **Alumni CRM Data Source ID:** 37415d96-e4cc-8071-8142-000b998f1bf0
- Client CRM title in Notion: Client Dashboard
- Alumni CRM title in Notion: Alumni Dashboard

## Tool Notes

- **Google Calendar ID:** EA276484-2D90-4F3E-B830-64346F182220 — Apple Calendar with Google sync. Do not connect a separate Google Calendar source.
- **Circle — Add Your Interviews space ID:** 2289335 — use list_posts
- **Circle — Share Your Wins space ID:** 1640485 — chat format, use list_space_ai_summaries; list_posts does NOT work here
- **Circle member tags:** Interview Accelerator = 168603, Alumni = 125170, Job Offer = 252382
- **Circle tag filtering:** Do not filter server-side by member_tag_ids — paginate all members (per_page: 100) and filter client-side
- **Google Drive transcript folder ID:** 1HpAkVyrqNgfYN4dAfUPb3yz2Mn7enEFO
- **Stripe:** Detection of new sales only — never use for MTD totals
- **QuickBooks:** Authoritative source for MTD net income

## Key Decisions

- Voice library is built and live — use it for all outreach drafts
- Nothing sends without Todd's explicit approval, on any channel
- Notion schema is static — stored in notion-crm-schema.md, never re-fetched
- Scheduled daily brief fires Monday–Friday at 9:00 AM
- Weekly brief fires Mondays (same trigger, extended scope)
- Call Offered cadence is auto-managed from Date SC Offered (Day 0) — Todd is never asked for these dates
- After Day 50 in Call Offered: leave Next Check-In blank, flag for workshop invites only
- pipeline-stages.md was intentionally not built as a separate file — pipeline stage definitions, live options, and win classification rules all live in notion-crm-schema.md
- Lead ICP prospects are tracked in Sales CRM but do not surface in Block 2 or Block 3 — intentional, not an oversight

## Deferred — Not Yet Built

**Lead ICP Cadence (Revenue Ops)**
Currently Lead ICP prospects are tracked in the Sales CRM
but do not surface in Block 2 interactive sections or the
Block 3 todo list. This is intentional for now — keeps the
brief focused on Call Offered pipeline.

When ready to build:
- Cadence is freeform, not fixed — outreach every 2 weeks
  if no communication from the prospect
- After 3–4 unanswered messages with no call offered →
  move Pipeline Stage to Closed
- Stage transition Lead ICP → Call Offered is triggered
  when Todd offers the call — currently managed via a Kondo
  label change, could be moved into Revenue Ops directly
- When activated, Lead ICP should surface in Block 2 and
  Block 3 alongside Call Offered — the goal is for a future
  operator to see and respond to the full pipeline, not just
  post-offer prospects