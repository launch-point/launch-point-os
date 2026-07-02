# CRM Snapshot Task

**Schedule:** Monday–Friday at 8:45 AM
**Workstation:** Revenue Ops
**Mode:** Scheduled — runs before the daily brief
**Purpose:** Keep the local Excel cache in sync with the 
Sales CRM in Notion so the daily brief reads from a fast, 
complete, accurate local file instead of querying Notion 
record by record.

---

## On Wake

Read the Excel cache at:
`Cache/sales-crm-cache.xlsx`

Check the "Last Refreshed" timestamp in cell A1 of the 
cache file. This determines whether today runs a full 
refresh or a delta update.

---

## Monday — Full Refresh

On Mondays, always run a full refresh regardless of the 
Last Refreshed timestamp.

1. Pull **every record** from the Sales CRM via notion-search 
   with data_source_url: 
   collection://31d15d96-e4cc-80d8-9772-000baea28015
2. Paginate through **all results** — do not stop at the 
   first page. Keep fetching until no more records are 
   returned.
3. For each record, write the following fields to the 
   Excel cache:

| Column | Notion Field |
|---|---|
| A | Page ID |
| B | Name |
| C | Pipeline Stage |
| D | Date SC Offered |
| E | Last Interaction |
| F | Next Check-In |
| G | Reason for "no"? |
| H | Preferred Channel |
| I | Phone Number |
| J | Email |
| K | LinkedIn URL |
| L | Source |
| M | Engagement Type |
| N | Referred By |
| O | Last Edited Time |

4. Overwrite all existing rows in the cache completely.
5. Write today's date and time to cell A1 as the Last 
   Refreshed timestamp.
6. Log: "Full refresh complete — [N] records written. 
   [Date/time]."

---

## Tuesday–Friday — Delta Update

1. Read the Last Refreshed timestamp from cell A1 of the 
   cache file.
2. Pull only records from the Sales CRM where 
   `last_edited_time` is newer than the Last Refreshed 
   timestamp.
3. Paginate through all results until no more changed 
   records are returned.
4. For each changed record:
   - Find the matching row in the cache by Page ID (Column A)
   - If found: update all fields in that row
   - If not found: add as a new row at the bottom
5. Check for records in the cache that no longer exist in 
   Notion (deleted records) — remove those rows.
6. Update the Last Refreshed timestamp in cell A1.
7. Log: "Delta update complete — [N] records updated, 
   [N] records added, [N] records removed. [Date/time]."

---

## Error Handling

**If the cache file is missing or unreadable:**
Run a full refresh immediately, regardless of day of week. 
Log: "⚠️ Cache file missing or unreadable — ran emergency 
full refresh."

**If Notion is unreachable:**
Log: "⚠️ Notion unreachable — cache not updated. Brief 
will read from last known cache. Last refresh: [timestamp]."
Do not crash. The brief will proceed with the existing 
cache and flag the staleness.

**If a record in Notion is missing a field:**
Write an empty string to that cell. Never skip the row 
entirely — a partial record is better than a missing one.

**If pagination returns inconsistent results:**
Re-fetch that page before proceeding. If it fails twice, 
log the page number and continue with remaining pages. 
Flag in the log: "⚠️ Page [N] returned inconsistent 
results — may have missed records. Full refresh recommended."

---

## Accuracy Notes

- This task runs at 8:45 AM — 15 minutes before the brief. 
  Do not proceed to the brief until this task completes.
- Page ID (Column A) is the join key between the cache and 
  Notion. Never use Name as a join key — names can change 
  or duplicate.
- Last Edited Time (Column O) is the delta comparison key. 
  Never use any other field for this purpose.
- A missed record in this cache = a missed prospect in the 
  brief = a potential $1,500–$1,800 lost sale. Accuracy 
  is the top priority. Speed is secondary.
