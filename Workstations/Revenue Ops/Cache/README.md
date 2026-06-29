# Cache

This folder holds the local Excel cache of the Sales CRM.

## File

`sales-crm-cache.xlsx`

## Setup (one-time, done by Todd)

1. Export the Sales CRM from Notion as CSV
2. Open in Excel and save as `sales-crm-cache.xlsx`
3. Drop the file in this folder

From that point forward, `Scheduled/crm-snapshot-task.md` 
manages all updates automatically. Do not manually edit 
this file after the initial seed — all writes go through 
the snapshot task or the daily brief's Notion write + 
cache update flow.

## Column Map

| Column | Field |
|---|---|
| A | Page ID (join key — never change) |
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
| O | Last Edited Time (delta comparison key) |

Row 1 = headers. Cell A1 = Last Refreshed timestamp 
(written by snapshot task after each run).

## Accuracy Note

A missed or stale record in this file = a missed prospect 
in the brief = a potential $1,500–$1,800 lost sale. If 
anything looks wrong, run a manual full refresh in Cowork:

```
Run the CRM snapshot task now as a full refresh — 
pull every Sales CRM record from Notion and overwrite 
the cache completely.
```
