# Cadence Table

## Scope

Call Offered stage only. The cadence clock starts on 
**Date SC Offered** (Day 0) — the date the sales call was 
offered to the prospect. All day counts are absolute from 
that date, not relative to the last touch.

Lead ICP cadence is intentionally not included here — see 
MEMORY.md under "Deferred — Not Yet Built" for the planned 
approach when that gets built.

---

## Channel Priority

**When Preferred Channel is set in the Sales CRM:** use it. 
This field reflects the most recent inbound message channel 
and is the authoritative source for how to reach this person.

**When Preferred Channel is not set:** fall back to this 
waterfall in order:
1. Quo (SMS) — if they're active on text
2. LinkedIn DM — if that's the primary engagement channel
3. Email
4. Default to LinkedIn DM if unclear

---

## Call Offered Cadence

| Day | Action | Notes |
|---|---|---|
| 0 | First touch — confirm Calendly link received | Sent same day the call is offered |
| 2 | Light follow-up | |
| 4 | Follow up again | |
| 7 | Send resource + quiz link if not yet completed | Check Kit.com subscriber record to confirm quiz completion before sending |
| 10 | Invite to next Ministry to Marketplace workshop | Only send if a workshop is upcoming — check calendar first |
| 17 | Job search check-in | |
| 18 | Invite to connect via text (Quo) | Only if not already connected via Quo |
| 25 | Check LinkedIn for updates + send resource + Quo invite if not yet connected | Two actions: resource send + Quo invite if still not connected |
| 40 | Personal message referencing their specific situation | Pull context from Kondo/Quo/Gmail before drafting — this one needs to feel specific, not templated |
| 50 | Low-pressure check-in — easy out, door open | Last scheduled touch |
| 50+ | No scheduled outreach | Flag for workshop invites only — do not initiate other outreach |

---

## Auto-Calculation Rules

- **Next Check-In is always auto-calculated** for Call Offered 
  prospects — never ask Todd for this date
- Calculate from Date SC Offered + the next cadence day that 
  hasn't been completed yet
- **All Next Check-In dates must land on weekdays.** If the 
  calculated date falls on Saturday or Sunday, advance to the 
  following Monday
- After a confirmed touch, immediately write the next cadence 
  day's date to Next Check-In in the Sales CRM — do not wait 
  for Todd to confirm
- After Day 50: leave Next Check-In blank. The prospect drops 
  off the regular cadence and surfaces only when a workshop 
  invite is relevant

---

## Post-Call Debrief Integration

When a debrief outcome is logged in Block 2A and the result 
is **Follow Up Call needed**, the prospect stays in Follow Up 
Call stage — the Call Offered cadence clock does not restart. 
Next Check-In for Follow Up Call is set manually by Todd, not 
auto-calculated.

When a debrief outcome moves the prospect to **Not Yet, Not 
Enough Money, or Waiting On Decision**, the Call Offered 
cadence clock stops entirely. Next Check-In switches to 
manual — always ask Todd for the date. Never auto-calculate 
for these stages.
