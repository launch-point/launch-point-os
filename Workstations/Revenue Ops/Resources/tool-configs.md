# Tool Configs

Connector-specific quirks, failure modes, and operating rules. 
Read this before using any of these tools for the first time 
in a session, or when troubleshooting a connector failure.

---

## Kondo (LinkedIn DMs)

**Critical requirement:** app.trykondo.com must be open in an 
active browser tab. Fails silently without it — no error, 
just empty/wrong results.

**Permissions:** Set to "Always allow."

**Search:** Use list_chats with a prospect name query. Verify 
results carefully — list_chats can return false matches based 
on message content, not just contact name.

**Read history:** Use read_chat with a chats parameter as a 
list of objects containing profileUrn and threadKey.

**Full history:** Use load_chat sparingly — it's slow and 
processes one conversation at a time. Don't bulk-pull.

**No direct message permalink** is available via the API. Use 
the person's LinkedIn URL for Kondo references instead.

**Failure handling:** If Kondo isn't returning expected 
results, first check that app.trykondo.com is open in an 
active tab before assuming the data doesn't exist. Flag and 
skip all Kondo references for the session if it can't be 
resolved.

---

## Quo (SMS)

**Required first step every session:** Run list-inboxes before 
attempting any Quo lookup. This returns the inbox phone number 
needed for fetch-messages calls.

**Per-prospect lookup:** fetch-messages, using the inbox phone 
number from list-inboxes plus the prospect's number.

**Failure handling:** If list-inboxes fails, flag exactly: 
"⚠️ Quo is disconnected — reconnect at Settings → Connected 
Apps" and skip all Quo references for that session. Do not 
retry repeatedly or guess at a workaround.

---

## Notion

**Schema is static.** Never re-fetch the Notion schema at the 
start of a brief or session — it's documented in full in 
notion-crm-schema.md. Only re-fetch live record data, never 
the schema itself.

**Date fields require two properties** when writing a date-only 
value (no time component):
- date:[Field Name]:start
- date:[Field Name]:is_datetime: 0

**Record creation:** notion-create-pages with parent set to 
the relevant data_source_id (see notion-crm-schema.md for IDs 
per database).

**Record updates:** notion-update-page with 
command: update_properties.

**Record lookup:** Prefer notion-fetch by page ID over 
name-based search — name search can fail for newer records 
that haven't fully indexed yet.

**`Reason for "no"?` field** — exact key string including the 
curly quotes and question mark. Typos in this key will silently 
fail to write.

---

## Circle

**Add Your Interviews** (space ID 2289335) — standard post 
format. Use list_posts with sort: latest.

**Share Your Wins** (space ID 1640485) — chat format, not 
standard posts. Use list_space_ai_summaries with a date_from 
parameter. list_posts does NOT work on this space — it will 
return empty or fail silently.

**Member tag IDs:**
- Interview Accelerator = 168603
- Alumni = 125170
- Job Offer = 252382

**Tag filtering — important quirk:** list_community_members 
does NOT reliably filter server-side by member_tag_ids. Do not 
rely on a server-side filtered call. Instead, paginate all 
members (per_page: 100, roughly 5 pages for ~449 members), 
then filter client-side using each record's member_tags array.

**Profile fields:** Access via list_profile_fields. Returned 
under flattened_profile_fields as a flat key-value dict on 
member records.

**Date fields** (e.g., 90_day_start_date) are stored as 
MM/DD/YYYY strings in Circle. Parse before writing to Notion 
date fields.

**Duplicate numeric fields exist** — prefer the number versions 
(of_days_to_first_interview, of_days_to_first_offer) over older 
text-format versions of the same data.

---

## Google Calendar ("Todd's Work")

**Calendar ID:** EA276484-2D90-4F3E-B830-64346F182220

**Important:** This is Apple Calendar — Google Calendar syncs 
into it. Do not attempt to connect or query a separate Google 
Calendar source; it will produce duplicate or conflicting data.

**Sales calls filter:** fullText: "Quick Call with Todd" — 
reliably isolates sales calls from other event types.

**Follow-up calls filter:** title contains "Follow Up with Todd" 
(partial match — captures "Follow Up with Todd and [Name]" 
format). Note: capital U, no hyphen — do not use "Follow-up".

**Workshop filter:** title contains "Ministry to Marketplace".

**Pull method:** Two parallel list_events calls (primary 
calendar + second source) with a 30-day window, pageSize: 50.

---

## Calendly

Used for routing form answers and booking form answers on 
same-day calls. Cross-reference against calendar events to 
build call prep cards — Calendly alone won't tell you which 
calendar event it corresponds to.

---

## Kit.com

**New subscriber detection:** list_subscribers sorted by 
created_at descending, then get_subscriber per ID to pull full 
field-level data. Tags alone are not sufficient for ICP scoring 
— you need the field-level quiz answers.

**ICP scoring rules** live in icp-rules.md, not here.

---

## QuickBooks

**MTD net income:** profit-loss-quickbooks-account with 
periodStart = first of current month, periodEnd = today.

**This is the authoritative source for MTD net income.** Do 
not use Stripe for MTD totals under any circumstance.

---

## Stripe

**Use only for:** detecting new individual sales in the last 
24 hours, via list_payment_intents.

**Never use for MTD aggregation** — there's no reliable 
timestamp data in the response for that purpose. QuickBooks is 
the source of truth for MTD revenue.

---

## Google Drive (Call Transcripts)

**Folder ID:** 1HpAkVyrqNgfYN4dAfUPb3yz2Mn7enEFO (Meet 
Recordings)

**File naming pattern:** "Quick Call with [Name] and Todd 
Linder - [YYYY/MM/DD HH:MM TZ] - Notes by Gemini"

**Used for:** Follow-up call prep only. Not used for first 
sales call prep — there's no transcript yet at that stage.

**Search method:** By prospect name.

---

## Granola

**Backup transcript source** when Google Drive returns 
nothing for a given prospect.

**Search method:** By prospect name and approximate call date.

**If both Google Drive and Granola fail:** flag exactly "No 
transcript found for [Name]'s original call — check Meet 
Recordings or Granola manually." Do not guess at call content.

---

## Gmail

Used per-prospect to check the most recent email activity for 
follow-up context. No special configuration quirks — straight 
search by prospect email address.