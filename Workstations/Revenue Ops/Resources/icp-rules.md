# ICP Qualification Rules

Used to score new Kit.com quiz subscribers. Apply these rules 
whenever evaluating a new subscriber for ICP fit. Disqualified 
subscribers are never shown — silent exclusion only.

---

## Step 1 — Hard Gate

Check this field first. If the subscriber's answer matches 
the disqualify condition, silently exclude them. Do not score. 
Do not surface. Do not mention them in the brief.

| Field | Disqualify if answer is |
|---|---|
| `which_of_the_following_best_describes_what_direction_you_want_next_for_your_career` | "I'm open to a church, ministry or business job" |

Only proceed to Step 2 if the subscriber passes the hard gate.

---

## Step 2 — Score 5 Criteria

Each criterion is worth 1 point. Score all 5 for every 
subscriber who passes the hard gate.

| # | Criterion | Kit.com Field | ICP Pass Condition |
|---|---|---|---|
| 1 | Applications submitted | `how_many_applications_have_you_submitted_so_far` | 11 or more ("11–25", "26–50", "51–100", etc.) |
| 2 | Timeline | `how_soon_are_you_looking_to_change_jobs` | 6 months or sooner |
| 3 | Interviews | `how_many_interviews_have_you_had_so_far_in_your_job_search` | 10 or fewer ("0–5", "6–10") |
| 4 | Years in ministry | `how_many_years_of_ministry_experience_do_you_have` | More than 10 years |
| 5 | Months applying | `how_long_have_you_been_actively_applying_to_jobs` | More than 1 month |

---

## Step 3 — Apply Threshold

| Score | Classification | Action |
|---|---|---|
| 5/5 | Full ICP | Surface in brief |
| 4/5 | Near ICP | Surface in brief |
| Below 4/5 | Not ICP | Silently exclude |
| Hard gate failure | Disqualified | Silently exclude |

---

## Brief Display Format

Surface only 4/5 and above. For each qualifying subscriber, 
show:

- Name
- Email
- Score (e.g., 4/5 — Near ICP)
- Each of the 5 criteria with ✅ (pass) or ❌ (fail)
- Already in Sales CRM: yes/no

**Near ICP display rule:** Do not show the hard gate field 
(career direction) as a row in the score breakdown. It is a 
gate, not a scored criterion — showing it alongside the 5 
scored rows would imply it contributed to the score, which 
it didn't. Only show the 5 scored criteria.

**Note on data retrieval:** Tags alone are insufficient for 
ICP scoring. Field-level quiz answer data is required. Use 
list_subscribers sorted by created_at descending, then 
get_subscriber per ID to pull full field data before scoring.
