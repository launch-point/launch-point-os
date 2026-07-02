# Notion CRM Schema

Static reference. Never re-fetch this schema from Notion — 
update this file manually when fields change.

There are **three** Notion databases this workstation reads 
and writes to. They are not interchangeable — each has a 
distinct job:

- **Sales CRM (Lead CRM)** — prospects, pre-purchase
- **Client CRM** — people currently in the 90-day program
- **Alumni CRM** — people after the program ends, whether 
  or not they have a job yet

---

## Sales CRM

**Database ID:** 31d15d96-e4cc-80cb-ba9e-fc72290995c1
**Data Source ID:** 31d15d96-e4cc-80d8-9772-000baea28015
**Pull records:** notion-search with 
data_source_url: collection://31d15d96-e4cc-80d8-9772-000baea28015
**Fetch one record:** notion-fetch by page ID (name-based 
search can fail for newer records — use page ID as fallback)
**Update records:** notion-update-page with 
command: update_properties
**New records:** notion-create-pages with parent set to 
data_source_id

### Fields

| Field | Type | Notes |
|---|---|---|
| Last Interaction | Date | |
| Next Check-In | Date | Must always land on a weekday. Advance Sat/Sun to following Monday. |
| Pipeline Stage | Select | See live options below |
| Source | Select | See live options below |
| Phone Number | Text | Free text, not Notion phone type |
| LinkedIn URL | Text | Free text, not Notion URL type |
| Engagement Type | Select | See live options below |
| Referred By | Text | Name of referrer |
| Date SC Offered | Date | Date the sales call was offered — this is the cadence clock start (Day 0) for Call Offered prospects |
| Date of Sales Call | Date | |
| Date of Sale | Date | |
| Reason for "no"? | Select | Exact key string including punctuation. See live options below |
| Email | Email | |
| Preferred Channel | Select | Options: Email, Text, LinkedIn. **Reflects the channel of the most recent inbound message — not a static preference.** Recalculates only when a new inbound message arrives on a different channel than currently set. Does not recalculate on every touch. |

### Pipeline Stage — Live Options (active use only)

Retired stages exist in Notion history but are never used 
going forward. Do not surface, suggest, or write retired 
stages.

1. Lead ICP
2. Call Offered
3. Call Booked
4. Follow Up Call
5. Waiting On Decision
6. Not Enough Money
7. Not Yet
8. Client!
9. Closed

### Source — Live Options

- Phone
- LinkedIn
- Web
- Email
- LinkedIn Quiz
- Workshop

### Engagement Type — Live Options

- Quiz
- Referral
- Reached Out
- Follower
- Profile Viewer

### Reason for "no"? — Live Options

Exact field key: `Reason for "no"?` — punctuation required.

- Not Enough Money
- Not Yet
- Going A Different Direction
- In Interviews
- Try On My Own
- Spouse Not On Board
- Stay In Ministry
- Still Discerning
- Got A Job
- Not A Fit (My Call)

### Post-Call Debrief — Outcome to Field Mapping

When logging a debrief outcome in Block 2A, write Pipeline 
Stage and Reason for "no"? according to this table. Do not 
deviate or infer a different mapping.

| Debrief Outcome | Pipeline Stage | Reason for "no"? |
|---|---|---|
| Client! | Client! | — |
| Waiting On Decision | Waiting On Decision | — |
| Follow Up Call needed | Follow Up Call | — |
| Not Enough Money | Not Enough Money | Not Enough Money |
| Going a Different Direction | Not Yet | Going A Different Direction |
| Trying on their own | Not Yet | Try On My Own |
| Spouse Not On Board | Not Yet | Spouse Not On Board |
| Still Discerning | Not Yet | Still Discerning |
| In Interviews | Not Yet | In Interviews |
| Stay In Ministry | Closed | Stay In Ministry |
| Got A Job | Closed | Got A Job |
| Not a fit — my call | Closed | Not A Fit (My Call) |
| No show / cancelled | Closed | — |
| Not Yet (no specific reason given) | Not Yet | Prompt Todd for the reason, then match his answer to an existing option above. Never create a new option. |

---

## Client CRM

**Database ID:** 31d15d96-e4cc-80d1-9782-c7919bac9c62
**Data Source ID:** 31d15d96-e4cc-80d7-88df-000be89d2aa5
**Title in Notion:** Client Dashboard
**Pull records:** notion-search with 
data_source_url: collection://31d15d96-e4cc-80d7-88df-000be89d2aa5
**Fetch one record:** notion-fetch by page ID
**Update records:** notion-update-page with 
command: update_properties

Program tracker for people currently in the 90-day Interview 
Accelerator. No general outreach cadence fields — these people 
are in active weekly contact via group calls already. The one 
exception is Job Start Date, which gets written here even 
after a record would otherwise be considered frozen (see 
Trigger Logic below).

### Fields

| Field | Type | Notes |
|---|---|---|
| Location | Text | |
| Client Status | Select | See live options below |
| 90 Day Start Date | Date | |
| 90 Day End Date | Date | Auto-calculated as Start + 90 |
| Job Start Date | Date | Manually captured from Todd — see Pending Job Start Dates logic below |
| Program | Select | Live values: "IA Pro," "IA Pro + Resume Writer" |
| Program Status | Select | Field is being redesigned — do not build logic against current values yet |
| Coach Notes | Text | Free-form |
| Job Type 1 | Text | Target role |
| Job Type 2 | Text | Target role |
| Job Type 3 | Text | Target role |
| Master Resumes | File/relation | |
| Days to First Interview | Number | |
| Days to Offer | Number | |
| Email | Email | |
| Phone | Text | Free text |
| LinkedIn | Text | Free text |
| Preferred Channel | Select | Options: Email, Text, LinkedIn. Same recalculation behavior as Sales CRM — reflects most recent inbound message channel, recalculates only on a new inbound message from a different channel. |

**Win Log:** Not a field. Wins are logged as a **Notion page 
comment** on the person's Client CRM record — see Win 
Detection & Logging below.

### Client Status — Live Options

1. APPROACHING 90 DAYS
2. 60 DAYS / HALFWAY
3. FIRST 30 DAYS
4. MONTH-TO-MONTH
5. ALUMNI
6. ALUMNI/OFFER
7. POST 90 DAYS — *not in active use, ignore*
8. DISENGAGED
9. CANCELLED

---

## Alumni CRM

**Database ID:** 37415d96-e4cc-805f-b810-c7871f8310b6
**Data Source ID:** 37415d96-e4cc-8071-8142-000b998f1bf0
**Title in Notion:** Alumni Dashboard
**Pull records:** notion-search with 
data_source_url: collection://37415d96-e4cc-8071-8142-000b998f1bf0
**Fetch one record:** notion-fetch by page ID
**Update records:** notion-update-page with 
command: update_properties

Relationship cadence tracker for people no longer in regular 
program contact. Built for managing outreach timing — this is 
the only CRM with a Next Check-In / Last Outreach cadence.

### Fields

| Field | Type | Notes |
|---|---|---|
| Location | Text | |
| Client Status | Select | Shares the same option list as Client CRM. Only ALUMNI and ALUMNI/OFFER are relevant here. |
| Job Start Date | Date | Manually captured from Todd — written here in parallel with Client CRM, see below |
| Program | Select | Live values: "IA Pro," "IA Pro + Resume Writer" |
| Program Status | Select | Field is being redesigned — do not build logic against current values yet |
| Master Resumes | File/relation | |
| Days to First Interview | Number | |
| Days to Offer | Number | |
| Testimonial Reach Out | Multi-select | Tracks the testimonial-ask sequence specifically — independent of general quarterly cadence. Live options: Initial Reachout, Follow Up #1, Follow Up #2 |
| Testimonial Submitted | Checkbox | One-time milestone |
| Next Check-In | Date | Must always land on a weekday. Advance Sat/Sun to following Monday. |
| Last Outreach | Date | |
| Preferred Channel | Select | Options: Email, Text, LinkedIn. Same recalculation behavior as Sales CRM — reflects most recent inbound message channel, recalculates only on a new inbound message from a different channel. |
| Phone | Text | Free text |
| LinkedIn | Text | Free text |
| Email | Email | |

**Win Log:** Not a field. Wins are logged as a **Notion page 
comment** on the person's Alumni CRM record — see Win 
Detection & Logging below.

---

## Trigger Logic — Client → Alumni Transition

This is the full chain. The agent does not orchestrate any 
part of step 3 — it is a Notion-native automation outside 
agent responsibility.

1. **Circle "Alumni" tag applied** (person stops paying, no 
   job yet) → Client CRM's Client Status updates to **ALUMNI**
2. **Circle "Job Offer" tag applied** (person gets a job — 
   this ends the client relationship regardless of payment 
   timing) → Client Status updates to **ALUMNI/OFFER**. 
   ALUMNI/OFFER never coexists with active client status — 
   getting a job ends the engagement.
3. **Either status change automatically triggers a Notion-
   native automation that copies the record into Alumni CRM.** 
   The agent does not initiate or manage this copy.
4. The **Client CRM record is left frozen** after the copy — 
   not deleted, not updated again — **except for Job Start 
   Date, which is the one field still written to both CRMs 
   after the copy has occurred** (see Pending Job Start Dates 
   below).
5. If someone becomes a client again after being copied to 
   Alumni CRM (re-engagement), the Client CRM record can be 
   updated again — this is the only other exception to the 
   frozen-record rule.

**Becoming Alumni (without offer) ≠ having a job.** A person 
can sit in Alumni CRM with Client Status = ALUMNI, still 
actively job searching, indefinitely.

**Job Offer can recur for someone already in Alumni CRM** 
(e.g., they left without a job, found one later). Before 
firing a congratulations/testimonial-ask win action, the agent 
must confirm this hasn't already happened for this specific 
offer — never assume Status = Job Offer alone means it's new.

---

## Pending Job Start Dates (Block 2B logic)

**Trigger:** Client Status = ALUMNI/OFFER in either Client CRM 
or Alumni CRM, with Job Start Date empty.

**Flow:**
1. Surface one at a time: "[Name] got their offer — what's 
   their start date?"
2. On Todd's reply, write Job Start Date to **both** Client 
   CRM and Alumni CRM records for that person (this is the 
   one exception to the frozen Client CRM rule)
3. Calculate **Next Check-In = Job Start Date + 90 days** 
   (weekday-adjusted) and write it to the **Alumni CRM** 
   record only — Client CRM has no Next Check-In field

---

## Win Detection & Logging Flow

**Detect** — search two Circle spaces:
- Share Your Wins (space ID 1640485, chat format — use 
  list_space_ai_summaries)
- Add Your Interviews (space ID 2289335, standard posts — 
  use list_posts)

**Classify** — First Interview / Final Round / Job Offer / 
Other Win, using the classification rules below.

- **First Interview** — first-ever post in Add Your 
  Interviews, or content suggests it's a first → congratulate 
  in Circle only
- **Final Round** — post mentions founders, CEO, panel, final 
  round, or offer-adjacent language → congratulate + prime for 
  testimonial ask
- **Job Offer** — post in Share Your Wins with offer/accepted 
  language → congratulate + reach out via Preferred Channel 
  for testimonial + confirm Client Status reflects ALUMNI/OFFER
- **Other Win** — anything else notable → congratulate in 
  Circle

**Locate** — check whether the person currently has a record 
in Client CRM or Alumni CRM.

**Log** — add a **Notion page comment** to whichever CRM page 
the person currently lives in. Never both. Never neither. 
There is no Win Log field on either CRM — comments are the 
only mechanism.

---

## CRM Quick Reference — Which Database for Which Question

| Question | CRM |
|---|---|
| Is this person a prospect who hasn't bought yet? | Sales CRM |
| Is this person currently paying for the program? | Client CRM |
| Has this person stopped paying (with or without a job)? | Alumni CRM |
| Does this person have a Next Check-In date? | Sales CRM or Alumni CRM only — never Client CRM |
| Where do I log a win/testimonial comment? | Whichever of Client CRM / Alumni CRM currently holds their record |
| Where does Job Start Date get written? | Both Client CRM and Alumni CRM |