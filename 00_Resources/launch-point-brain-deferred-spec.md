# The Launch Point Brain — Deferred Design Spec

*Extracted from `agent-and-os-best-practices.md`, Section 14A. This file is a
companion to the main best practices document — cross-references to numbered
sections (Section 2, Section 7, Section 8, Section 11, Section 12) refer back
to that document. Open this file only when Todd is ready to initiate this
build; it adds no value to any other planning session.*

---

**Status: designed at the concept level only. Not built. Not scheduled. Revisit
when Todd initiates.**

## What This Is

A persistent ingestion layer with two related but distinct branches, both deferred
together so they're designed consistently rather than retrofitted later:

**Branch 1 — Architectural memory for this planning space.** Captures context from
ClickUp task activity, Slack conversations, and team call recordings, and makes
that context available to future planning sessions here, specifically for
building and architecting decisions. This is the general-purpose realization of
Section 14's Knowledge Layer, applied to architectural continuity rather than
client-facing coaching knowledge. It originated as a narrow flag under Section 8
(the L1→L2 skill-maturity signal) but is much larger in scope than that.

**Branch 2 — Sales call recordings for Revenue Ops.** Todd records all sales
calls. These recordings serve two distinct purposes, both in scope:
(1) **coaching/improvement** — grading calls, extracting what worked and what
didn't, feeding that back as gotchas or improvements to Revenue Ops skills (the
Amul Aasar pattern from Section 8's verification discussion — a skill that
simulates structured feedback, applied here to Todd's own sales performance); and
(2) **prospect-context enrichment** — pulling relevant detail from a specific
prospect's call into future interactions with that same prospect (pre-call prep,
follow-up drafting). This branch belongs to Revenue Ops specifically, not to the
planning space — it's function-owned, distinct from Branch 1's OS-wide scope.

The problem both branches solve: right now, context that exists in these sources
has to be manually carried forward by Todd — into this planning space for
architectural decisions, or into Revenue Ops sessions for call prep and coaching.
`launch-point-os-context.md` and `MEMORY.md` hold some of this, but manually.
Both branches would make that capture automatic.

## The Four Sources, and Why They're Not Equally Simple

| Source | Branch | Signal Type | Reliability | Build Complexity |
|---|---|---|---|---|
| **ClickUp** | 1 — Architectural memory | Structured — task assignment, status change, completion, abandonment | High — unambiguous, closer to Section 12's Make.com pattern (condition-based, deterministic) | Lower — likely a Make.com trigger or scheduled sync, not language processing |
| **Slack** | 1 — Architectural memory | Conversational — team discussion, decisions mentioned in passing | Medium — requires judgment to separate signal from noise | Medium — needs a summarization/extraction skill, not just a data pull |
| **Team call recordings** | 1 — Architectural memory | Conversational, long-form | Medium-low — highest ambiguity, longest content to process | Highest — requires a Google Drive → GitHub or Make.com ingestion pipeline, transcript parsing, and a decision about what gets extracted vs. discarded |
| **Sales call recordings** | 2 — Revenue Ops (coaching + enrichment) | Conversational, long-form, but structurally more predictable than team calls (sales calls tend to follow a similar shape — discovery, objections, close) | Medium — more predictable structure than team calls, but still requires judgment for what counts as "worked" vs. "didn't work" | High — same ingestion pipeline complexity as team recordings, plus a verification/grading skill (Section 8, Type 2) needs to be designed on top of it |

**Decision confirmed:** all sources across both branches are designed together
as one unified system, not built in sequence — a deliberate choice to design the
full shape before building any piece, so the pieces are consistent with each
other from the start rather than retrofitted later.

## Where the Captured Context Lives — Recommendation

**Branch 1 (architectural memory) recommendation: a dedicated Architecture
Decisions Log — not folded into MEMORY.md.**

Reasoning: MEMORY.md (Section 7) is explicitly scoped to per-function facts —
contacts, pipeline status, decisions specific to one workstation, agent, or
application. Branch 1 is cross-cutting and specific to the planning space
itself, not to any one function. Folding it into root MEMORY.md would violate
Section 7's own scoping rule and mix two different categories of content.

**Naming note:** the proposed "Architecture Decisions Log" here is a different
thing from the "Current Decisions" subsection every MEMORY.md already has
(Section 7). MEMORY.md's Current Decisions is small, per-function, and updated
in place — old entries get replaced, not retained. The Architecture Decisions
Log would be the opposite by design: standalone, OS-wide, and deliberately
historical/append-only, closer to the audit-log pattern in Section 8 than to
MEMORY.md's update-in-place behavior. Don't conflate the two when this gets built.

The Architecture Decisions Log would function as a fourth business context file
— alongside CLAUDE.md, MEMORY.md, and `launch-point-os-context.md` — but
populated automatically from Branch 1's three sources rather than manually
updated. This planning space would read it at the start of a session the same
way it currently references `launch-point-os-context.md` for build status,
per this project's own instructions ("consult the business context files for
what's settled... don't re-derive them").

**Branch 2 (sales call recordings) recommendation: lives inside Revenue Ops,
not the Architecture Decisions Log.** Per Section 3's function-first structure,
this belongs in `Revenue Ops/Workstation/Resources/` or `Revenue Ops/Agents/`
depending on how the coaching/enrichment mechanism ends up triggered
(session-based review vs. autonomous post-call processing — see Section 2's
Decision Rule, to be applied once this is actually designed). The
coaching/grading output likely feeds Revenue Ops' MEMORY.md (per-prospect
enrichment) and skill gotchas (coaching feedback), not the Architecture
Decisions Log — Branch 2 has nothing to do with architectural planning
decisions, so mixing it into that log would be the same scoping mistake
Section 7 already warns against.

**Neither recommendation has been approved — both are starting points for
Todd's review when this item is picked back up, not locked decisions.**

## What Needs to Be Decided Before This Gets Built

This list exists so the next session that picks this up doesn't have to
rediscover these questions from scratch. Split by branch, since they're
different builds with different owners.

**Branch 1 — Architectural memory (ClickUp, Slack, team calls):**
- Confirm or revise the Architecture Decisions Log recommendation above
- For Slack: which channels get ingested — all team Slack, or specific
  channels? Does this apply to DMs or only shared channels?
- For team call recordings: which calls count as "team calls" for this branch —
  internal meetings only? (Sales calls are explicitly Branch 2, not this list —
  see below.)
- What's the extraction unit — does Claude summarize each source into discrete
  "facts" or "decisions," or does it maintain something more like a rolling
  narrative log?
- Who approves what gets written to the Architecture Decisions Log — does this
  need an approval gate the way outward-facing messages do (Section 11), or is
  internal architectural memory lower-stakes since nothing leaves the OS?
- Build environment: per Section 2, this is unambiguously a Claude Code build
  (ingestion pipelines, scheduled syncs, possibly a Make.com trigger for
  ClickUp) — not something this planning space can execute, only design

**Branch 2 — Sales call recordings (Revenue Ops, coaching + enrichment):**
- **Client consent and data handling.** Sales calls are, by definition,
  conversations with prospects — this is exactly the client-data question
  flagged generically above, now concrete and no longer avoidable once this
  branch is designed for real. Confirm what consent or disclosure is already
  in place for recording sales calls, and whether feeding those recordings
  into an AI grading/enrichment pipeline requires anything additional. See
  Section 10A (Data Sensitivity) for the tier framework this question fits
  inside.
- Does the coaching/grading skill run on every call automatically, or only on
  calls Todd flags for review? (Ties to Section 9's scheduled task vs. Section
  2's session-initiated distinction.)
- What does "graded" actually mean here — the same A–F quality scale from
  Section 8's Type 2 verification skills, applied to sales performance? If so,
  what's being graded — the call outcome, Todd's technique, both?
- For the enrichment purpose: what specifically gets pulled forward into a
  future interaction with the same prospect — direct quotes, a summary,
  specific objections raised? This needs to be concrete before a skill can be
  built around it.
- Trigger and environment: is this a Workstation-tier review Todd initiates
  after a call, or an Agent-tier process that runs automatically once a
  recording lands — apply Section 2's Decision Rule once scoped in detail
