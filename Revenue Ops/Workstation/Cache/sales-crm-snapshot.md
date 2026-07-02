# Sales CRM Snapshot

**Last refreshed:** 2026-07-01 08:53:46
**Source:** Notion Sales CRM (collection://31d15d96-e4cc-80d8-9772-000baea28015)
**Method:** Delta update — see critical note below on methodology change

---

## ⚠️ Critical Finding — 2026-07-01

**Notion's bulk query tools (query_data_sources SQL mode, query_database_view) are not available on this workspace's current Notion plan** — both returned "requires a Business plan or higher with Notion AI" errors. These are the tools this task was designed around for full/delta refreshes.

Fallback method used today: fetched all 35 existing cache records individually by Page ID (no changes, no deletions — see Delta Log below), plus targeted notion-search lookups.

**While verifying, discovered the cache is missing a significant number of real, active Sales CRM records that exist in Notion but were never in the xlsx cache.** These were referenced by name in this log's own 2026-06-30 "Active Pipeline Summary" section (Call Offered / Waiting On Decision / Not Enough Money prospects), but a check of the actual xlsx file showed they were never written there. Spot-checked via notion-search — all confirmed real, existing Notion pages.

**Recovered and added to cache today (10 records, full data verified via notion-fetch):**

| Name | Page ID | Stage | Date SC Offered | Last Interaction |
|---|---|---|---|---|
| Jay Vanderbur | 36b15d96-e4cc-8081-a9db-eb9494b17e29 | Call Offered | 2026-05-25 | 2026-05-25 |
| Thomas Blackmon | 36615d96-e4cc-80cc-96f2-cf1ed4418a5b | Call Offered | 2026-05-20 | 2026-05-20 |
| Josiah Kenniv | 38315d96-e4cc-8100-b54e-cdb8a66e4dfa | Not Enough Money | — | 2026-03-17 |
| Josiah Kenniv (duplicate person/entry) | 36115d96-e4cc-80e5-80cd-ed4c1da26560 | Call Offered | 2026-05-15 | 2026-05-18 |
| Gerald Bargaineer II | 36015d96-e4cc-802b-b512-e24b410b9237 | Call Offered | 2026-05-14 | 2026-05-18 |
| Bryce Gross | 36015d96-e4cc-80d6-b722-e618864f21f5 | Call Offered | 2026-05-13 | 2026-05-18 |
| Jessica Mendez | 36015d96-e4cc-8074-8845-c2e59ae07fea | Call Offered | 2026-05-13 | 2026-05-18 |
| Ben Beswick | 36015d96-e4cc-8089-85e0-f923830c1e0f | Call Offered | 2026-05-13 | 2026-05-18 |
| Eric Dorville | 31f15d96-e4cc-8104-b725-e99639d700f0 | Lead | — | — |
| Eric Dorville (duplicate person/entry) | 35e15d96-e4cc-80f8-b718-fd7c508f24b9 | Call Offered | 2026-05-11 | 2026-05-18 |

**Confirmed to exist in Notion but NOT yet recovered (ran out of safe search budget — hit Notion rate limits mid-check):** Joshua Trammell, Bo Coburn (2 entries), Richard Rosas (2 entries), Jason Wilkins (2 entries), Nate Meyst (2 entries), Austin Mankin (2 entries), Jennifer Lara (1 additional beyond Jennifer Durrett already in cache), plus Ryan Ventura, Brad McKeehan, Timothy Cruz (existence unconfirmed — search was rate-limited before these could be checked).

**This means the cache is very likely still missing 10+ additional real pipeline records beyond what was recovered today.** Given "a missed record = a missed prospect = a potential $1,500–$1,800 lost sale," this needs attention beyond today's automated run.

**Recommendation for Todd:** Either (a) upgrade the Notion plan to Business+Notion AI so the bulk query tools work and a true full refresh is possible, or (b) manually export/share the full current CRM record list so it can be reconciled against this cache directly. Until one of those happens, delta updates via individual-fetch can only catch changes to records *already in the cache* — they cannot discover records that were never added.

---

## Delta Update Log — 2026-07-01

**Records checked (existing 35 cache rows, individually fetched):** 35
**Records changed:** 0
**Records deleted from Notion:** 0
**Records recovered/added (see critical finding above):** 10
**Total cache rows now:** 47

---

## Prior Log — 2026-06-30 (for reference)

**Records updated:** 0
**Records added:** 8 (Call Booked stage, from LinkedIn outreach — Katie Wickstrum, Bill Finnell, Jonah Sinclair, Dave Thomas, Clarke David Brogger, Benjamen Pegg, Jennifer Durrett, Tim Chaptman)
**Records removed:** 0

Note: the 2026-06-30 log's "Active Pipeline Summary" section listed 15+ Call Offered/other-stage names that were never actually present in the xlsx cache — this is the source of the critical finding above.

---

## ⚠️ Flags as of 2026-07-01

**Overdue Next Check-Ins (from records now in cache):**
- Jay Vanderbur — Call Offered, due 2026-06-01 (overdue)
- Thomas Blackmon — Call Offered, due 2026-06-01 (overdue)
- Bryce Gross — Call Offered, due 2026-09-07 (not yet due)

**Call Booked records with call dates already past (carried from 2026-06-30, unresolved — need post-call debrief):**
- Katie Wickstrum — call date 2026-06-17
- Bill Finnell — call date 2026-06-24
- Jonah Sinclair — call date 2026-06-23
- Dave Thomas — call date 2026-06-24
- Clarke David Brogger — call date 2026-06-25

**Data integrity:** See Critical Finding above — recommend full manual reconciliation before relying on this cache as complete.
