# Skills Protocol — The Full Skill-Building Reference

*Extracted from `agent-and-os-best-practices.md`, Section 8. This file is a
companion to the main best practices document — cross-references to numbered
sections (Section 2, Section 4, Section 6, Section 7, Section 9, Section 11,
Section 13, Section 14, Section 14A, Section 16) refer back to that document.
Consult this file whenever actually designing, writing, or auditing a skill.*

---

**Source: `How_Anthropic_Employees_ACTUALLY_Use_Claude_Skills_-_Austin_Marchese`, `How_Claude_Code_s_Creator_Starts_EVERY_Project_-_Austin_Marchese`, `The_Field_Guide_to_Building_Products_with_AI.md`**

## What a Skill Is

A skill is a documented playbook for a repeatable task. Once written, the agent runs it
reliably every time without re-explanation. A prompt tells an agent to do something.
A skill is the exact play to run — and the agent knows how to run it every single time.

The best analogy: a prompt is telling a basketball player to dribble. A skill is the
pick-and-roll play — a specific sequence of moves that produces a reliable result.

Skills are living documents. They get better as you use them and add to them. Do not
try to make a skill perfect on day one.

## How Skills Get Created

Every skill enters the system through one of two methods. Both are valid. Neither
produces a finished, production-ready skill on the first pass — both produce a
first draft that moves to Claude Code for refinement before being installed.

**Method 1 — Idea-first.** You know the skill you want before any session happens.
You have source material for it — a transcript, a course, a document, an existing
process description. Feed that material to Claude in Claude Code and say: *"Based
on this, build me a skill for [task]."* Claude uses the skill creator skill (built
into Claude Code by default) to generate the skill file from the source material.
This is how voice-principles.md could become a voice-check skill, or how a sales
methodology document could become a cadence management skill.

**Method 2 — Process-first.** You run through a task with Claude in a live
session, get a result you're happy with, and know you'll need it again. At that
point — in the session, not later — say: *"Create a skill for what we just did."*
Claude packages the process into a skill file immediately. This is the most
common way skills get created in practice, because the process is already
proven — you just saw it work.

In both methods, Claude may also recognize the pattern before you do and prompt
you — see Section 6's Skill-Recognition Rule. Either way, the output is a
first-draft skill file that moves to Claude Code for testing and refinement
before installation. Do not treat a session-drafted skill as live until it has
completed the install protocol below.

## The Three Levels of Skill Maturity

Per `The_Field_Guide_to_Building_Products_with_AI.md`: every skill exists at one
of three maturity levels. Most skills start at L1 and most never progress
further — the ones that do compound indefinitely.

- **L1: Personal shortcut.** Used by one person, handles one recurring task.
  **This is where every Launch Point skill currently sits, correctly** — Todd
  is the sole user of every installed skill right now. Being installed in a
  shared folder (`00_Resources/Skills/`) doesn't make a skill L2; only actual
  use by more than one person does.
- **L2: Team standard.** The skill is shared and consistently used by more
  than one person — Tara uses the same DM framework, Michelle follows the same
  onboarding skill. This is the same shift Section 13 describes as Layer 2
  (Organizational Coordination) — skill maturity and OS layer move together.
  A skill graduates to L2 the moment a second person starts relying on it the
  same way Todd does, not before.
- **L3: Organizational asset.** Maintained, versioned, improved with an active
  feedback loop — gotchas added from real failures (Rule 2), quality grades
  feeding improvements (Type 2 verification), reviewed periodically (Section
  16's skills audit). Almost no skill reaches L3 by accident; it takes
  deliberate maintenance over time.

**Where Launch Point is now:** every skill is L1. L2 becomes relevant once team
members start using shared skills — which the OS isn't ready for yet, per
Section 13's Layer 1 gating. Don't treat a skill as L2 just because it's
installed correctly; treat it as L2 only once someone besides Todd is actually
using it.

**The L1→L2 trigger — how Claude knows to upgrade a skill's maturity level:**
Claude cannot detect this from the filesystem alone — deploying a skill in a
shared location is access, not use. The actual signal is conversational: watch
for Todd saying anything that indicates a second person is now relying on a
skill the same way Todd does — "Tara's using this for her DMs now," "Michelle's
running onboarding herself," "I set this up so the team can use it." When that
signal appears, two things happen: (1) flag the skill's maturity level as
upgraded to L2 in its own file or a skills registry note, and (2) treat this as
a MEMORY.md-worthy fact per Section 7 — who's using what should be recorded the
same way any other decision or status change is. Do not upgrade a skill's
maturity level based on where it's deployed or who has technical access to it —
only based on confirmed, stated use.

*This L1→L2 signal is one specific use case of a much larger deferred capability —
see Section 14A, "The Launch Point Brain (Deferred)," for the full scope, which
extends well beyond skill maturity into architectural continuity for this
planning space itself.*

## The Four Skill Types

Every skill belongs cleanly to one type. Skills that straddle multiple types confuse
the agent.

**Type 1: Utility Skills**
Small, reusable, single-purpose. Often layered inside larger skills.
- *Launch Point examples:* Draft message in Todd's voice. Shorten text without
  compressing meaning. Pull Notion record for a given prospect.

**Type 2: Verification Skills**
Check the final output before it goes anywhere. Anthropic's team ranks verification
as having the most measurable impact on output quality of any skill type — worth
dedicated build time to get right. Boris Churnney's own stated claim: giving
Claude a way to verify its work will "2 to 3x the quality of the output." This
isn't a soft preference — it's a concrete, attributed multiplier, and it's why
verification is worth deliberate build time rather than an afterthought.

**There are two distinct kinds of verification, and most builders only build the
first:**

- **Verify for correctness** — did Claude get the facts right? Are the numbers
  accurate? Did the action actually complete? This is binary and objective —
  pass or fail. *Launch Point examples:* Did the CRM record actually update? Is
  the call date in the brief correct?
- **Verify for quality** — does the output meet the bar Todd actually wants,
  beyond just being factually correct? This is the verification type most people
  skip, and it's the one that actually raises output quality rather than just
  catching errors.

**Quality verification uses a letter grade, every time: A, B, C, or F.** No
grade below F gets used — there's no partial credit below failing. When a
quality verifier produces anything below an A, it doesn't just report the
grade. It asks Todd: *"What would have made this an A?"* Todd's answer becomes
a gotcha on the skill that produced the output (Rule 2), so the same gap
doesn't recur. This is how quality verification feeds directly into the OS's
compounding improvement loop (Section 16) rather than being a one-off check.

- *Launch Point examples:* Brand voice check, graded A–F against
  voice-principles.md, with Todd's feedback captured as a gotcha when it's not
  an A. DM quality check before sending — does this sound like something Todd
  would actually send? Approval gate check before any message sends
  (correctness — pass/fail, did all required fields get checked). ICP scoring
  before a lead is prioritized (graded, not pass/fail).

**A third pattern worth naming: judgment simulation.** Rather than grading
against a fixed rubric, a verification skill can simulate feedback from a
specific real person — feeding Claude that person's public writing, Slack
history, or prior conversations, then asking "what would [person] say about
this?" Anthropic's own example: a verification skill that simulates a
manager's feedback weekly, so output has already passed that person's
standards before they see it. For Launch Point, this could plausibly apply to
simulating Taylor's or Ricardo's perspective on a business decision — not yet
built, but worth flagging as a distinct pattern from the A–F grading above,
since it encodes one specific person's judgment rather than a fixed standard.

**Skill-driven verification** is the recommended build pattern for all three
kinds above (correctness, quality, judgment simulation): rather than building
a standalone verifier from scratch, add a verification component to an
existing skill so it produces the objective output — pass/fail, A–F, or a
simulated review — directly as part of running the skill. Two paths to get
there: audit your existing skills to find ones that could be tweaked into a
verifier this way, or build a new verifier from scratch when no existing
skill fits. Prefer the first when possible — it's less work and keeps the
verification logic close to the skill it's checking.

**Type 3: Data Enrichment Skills**
Pull external data into the session to enhance the output.
- *Launch Point examples:* Pull recent LinkedIn activity for a prospect before drafting
  a DM. Pull CRM snapshot for a sales call lead. Pull Circle activity before a client
  session.

**Type 4: Orchestration Skills**
Chain steps and other skills together into a complete workflow. An orchestration skill
calls utility skills, enrichment skills, and verification skills in sequence.
- *Launch Point examples:* Morning briefing skill (pulls CRM cache → summarizes pipeline
  → surfaces priority actions → presents for approval). Sales call brief (enriches
  CRM record + pulls LinkedIn activity + generates call brief for Todd's review).

**How orchestration skills call other skills:** reference the other skill by name
in the orchestration skill's instructions. If the referenced skill is installed in
the same environment, the model invokes it automatically. This means updating a
utility skill propagates automatically to every orchestration skill that calls it —
fix the sub-skill once, everything that depends on it improves. This is the primary
compounding mechanism at the skill level, and it is why building utility skills
first (not after) is non-negotiable.

**Build order is non-negotiable:** decompose any complex workflow into its
sub-utility skills first. Build those. Test those. Then build the orchestration on
top. An orchestration skill written before its sub-skills exist is not an
orchestration skill — it's an over-specified prompt that will fail when its
dependencies don't exist. If building from scratch, ask: "Can I break this into
sub-utility skills?" If yes, do that first.

## The Audit Skill Pattern — A Specialized Verification + Orchestration Combination

**Grounding note:** this pattern's core justification comes from
`The_Field_Guide_to_Building_Products_with_AI.md`'s three building blocks
for a Compound system: persistent memory (updated after every correction),
feedback capture (every edit saved as a lesson), and automated pipelines
(work that runs and surfaces output without Todd watching). What follows
below is this pattern's mechanics built from those three blocks directly —
not reverse-engineered from any specific existing implementation. Where a
specific mechanic (thresholds, file structure, approval format) isn't itself
stated in the source material, it's marked as **proposed**, not established
— meaning it's Claude's best design given the three building blocks, still
to be tested against real use per Section 5's "babysit early runs" principle,
not a rule to treat as settled.

Some skills run repeatedly with variable output — an Application processing
one client run after another, an autonomous Agent firing once per trigger
event, a Workstation's scheduled task running the same shape every day.
When something runs repeatedly like this, individual corrections aren't
enough on their own: a mistake caught and fixed once doesn't tell you
whether it's a one-off or a recurring pattern. The **audit skill pattern**
is a combination of Type 2 (Verification) and Type 4 (Orchestration) built
specifically to detect drift across repeated runs, not within a single run
— this is feedback capture (building block 2) applied at the cross-run level.

**When a tier needs this pattern:** the real trigger isn't whether Todd is
present — it's whether the thing **runs more than once.** These are two
different failure modes, and Todd's presence only catches one of them:

- **Content drift** — a wrong fact, bad phrasing, an incorrect record. Todd
  catches this live in a Workstation session, and Section 16's existing
  session-audit-and-CLAUDE.md-update loop handles it. This is why a genuinely
  one-off, ad hoc Workstation conversation doesn't need an audit log — there's
  nothing to compare it against.
- **Process drift** — the right output reached the slow way, an inefficient
  routing decision, an extra unnecessary step. This is much harder to catch in
  the moment, even with Todd present, because a successful end result doesn't
  reveal how it got there. A single good outcome tells you nothing about
  whether the path was efficient or accidental — only comparing it against
  prior runs of the same task does.

**So the audit skill pattern applies to anything that runs repeatedly,
regardless of tier:** Applications (once per client or per unit of work),
autonomous Agents (once per triggered event), **and** Workstation scheduled
tasks (same skill, run again and again, where a routing inefficiency in run
one will silently repeat in every run after it unless something is comparing
runs against each other). What does **not** need this pattern is a genuinely
one-off Workstation session — a single ad hoc conversation that will never
run in that exact shape again. There's no second run to compare against, so
there's no drift to detect.

**Practical implication:** any scheduled task (Section 9) is, by definition,
something that runs repeatedly — so every scheduled task is a candidate for
this pattern, not just Applications and Agents. When a scheduled task is built,
ask whether it's worth the overhead of an audit log given how often it runs and
how much a routing inefficiency would compound — a daily brief running five
times a week for a year is a strong candidate; something that runs once a
quarter may not be worth the maintenance overhead yet.

**The two-file structure (proposed, not established):**

Built directly from building block 1 (persistent memory) and building block
2 (feedback capture) — the Field Guide doesn't specify a two-file split, but
splitting "what happened" from "what's been learned" follows naturally from
treating those as two distinct kinds of memory: a historical record vs. a
living, current state.

- **`{name}_audit_log.md`** — append-only, one entry per run. Records what
  happened: quality issues found, consistency issues across handoff points,
  human interventions (corrections, questions, frustrations, qualified
  approvals), what was applied vs. deferred. Never edited after being written
  — only appended to.
- **`{name}_audit_memory.md`** — a living document, not append-only. Tracks
  confirmed patterns (seen across multiple runs), proposed skill drafts,
  resolved patterns with their resolution date, and one-off observations
  still being watched for recurrence.

**How it runs (proposed):** the audit skill is not standalone — it's a phase
inside a larger orchestration skill, triggered automatically at a defined
point after a run completes. This is building block 3 (automated pipelines)
directly: the audit runs while Todd isn't watching, and surfaces its findings
for review rather than requiring Todd to remember to check. It reads the
prior audit log and audit memory, compares this run's findings against them,
and only proposes an actual file edit once a pattern has been confirmed — a
single occurrence is logged only, not acted on; a repeat occurrence triggers
a proposal.

**A starting promotion threshold (proposed — needs real-use validation
before treating as fixed):**

| Finding type | Suggested threshold for a proposal |
|---|---|
| Quality check failure (same field, same pattern) | 2+ runs |
| Consistency break (same handoff point) | 2+ runs |
| Human intervention (a correction Todd made) | 1 run — corrections always warrant a look |
| Human suggestion explicitly about the workflow | 1 run |
| Human frustration or repeated clarification | 2+ runs |
| New capability gap the workflow can't currently handle | 1 run, if clear |

These numbers aren't derived from source material — they're a reasonable
starting point given the general principle (corrections matter immediately;
quality patterns need repetition to confirm). Adjust them once a real audit
skill has run enough times to show whether 2 is the right bar or whether it
should be different for a specific implementation.

**The approval gate (proposed mechanics, grounded in an established
principle):** every proposed change — whether an edit to an existing file or
a brand-new skill draft — should be presented individually with an explicit
approve/skip choice per item. Nothing gets written to an active file without
that explicit approval. New skill drafts should never be saved directly to
an active location — they go to a staging folder first, per Section 9's rule
that nothing gets scheduled or trusted until it's been tested. This
per-item approval mechanic isn't itself specified in the source material,
but it follows directly from Section 11's approval-gate principle, applied
to self-improvement of the OS's own skill files — the mechanic is proposed,
the underlying rule it implements is not.

Following the Field Guide's lessons.md pattern, keep proposed-but-unapproved
changes visibly separate from confirmed, applied ones within the same audit
memory file — a "Pending Review" section distinct from the confirmed pattern
list — so Todd can see at a glance what's live versus what's waiting on a
decision.

**Global rules for any audit skill built on this pattern (proposed, follows
from the principles above):** never overwrite the append-only log — always
append. Never apply a file edit without explicit per-item approval. Never
edit the source materials being audited — read only. Never save a proposed
skill anywhere but the staging folder. Never propose an edit from a single
occurrence unless it was an explicit correction Todd made in the run itself.
If the log or memory file doesn't exist yet, create it — don't error out.

**Before building the first real implementation of this pattern:** test
these proposed mechanics against an actual run, the same way any new build
gets validated (Section 5's Verification Protocol, including the
fresh-session test). Don't treat the threshold table or file structure above
as settled until they've been used at least once.

## Skills Are Folders, Not Just Markdown Files

Per `How_Anthropic_Employees_ACTUALLY_Use_Claude_Skills_-_Austin_Marchese`: a
skill is a folder with components inside it, not a single markdown file. The three
most important components:

- **The instruction file** — the markdown file describing what the skill does and
  when it fires (the description field — see Rule 3 below)
- **Scripts** — code that runs to complete deterministic steps. Giving Claude a
  script for a deterministic step is always better than asking Claude to produce
  consistent output through language alone. This is Rule 4 below applied at the
  folder level.
- **Assets and data** — reference files, schema files, example outputs, lookup
  tables the skill needs when it runs

Not every skill needs all three. A simple utility skill may be a single markdown
file. Complex skills — especially data enrichment and orchestration — almost always
benefit from scripts handling their deterministic steps.

## Setup Prompts — The Three Components That Keep Skills Usable

Per `How_Anthropic_Employees_ACTUALLY_Use_Claude_Skills_-_Austin_Marchese`: beyond
the folder components above, every skill should be evaluated against three "setup
prompt" components. The source material's framing is exact: you will quickly
forget how a skill works, so build for yourself one, two, three years from now —
not for today.

**1. config.json — first-run memory.** If a skill needs specific values to run
(an API endpoint, a folder path, a default setting), create a config.json file.
On first run, if a value is missing, the skill asks for it; once provided, it's
stored so future runs don't ask again. The skill remembers its task setup going
forward.

**2. Ask user question tool — structured input.** When a skill needs input,
use the ask user question tool inside the skill's instruction file to present
structured multiple-choice options instead of free-form text. This enforces a
clean, consistent interface.

**3. Arguments field — declared inputs.** Declare an arguments field at the top
of the skill's instruction file. This reminds whoever calls the skill — including
Todd, much later — exactly what input it needs to run properly.

*Note: Launch Point's existing `tool-configs.md` already applies a related
instinct — explicit failure-handling instructions per tool (e.g., "If Kondo
isn't returning expected results, first check app.trykondo.com is open... flag
and skip"). That pattern is a precursor to formal skill gotchas (Rule 2 below)
and should inform how these three components get written once skills are
formally built.*

Run a usability check on every skill before calling it production-ready: does it
need a config.json? Should any input be a structured question instead of free
text? Is the arguments field declared at the top?

## The Five Rules for Building Skills

**Rule 1: Start with a few lines and one gotcha.** Do not try to build the complete
skill before you've used it. The best skills started with minimal instructions and grew
as real failure points were discovered.

**Rule 2: Add gotchas only from real failures.** A gotcha section is a running list of
things Claude should not do within this skill. These must come from actual observed
failures — not pre-emptive guesses. Add them as you find them, not all at once.

**Rule 3: Write the trigger description as a routing condition, not a summary.**
Claude reads skill descriptions to decide which skill to use. The description field
is not "what this skill does" — it is "when this skill fires." It should name the
situation and the human language that signals it.

*Example from Anthropic's own front-end design skill:*
> "Use this skill when the user asks to build web components, pages, or applications."

Write your trigger the same way.

**Rule 4: Separate deterministic from non-deterministic work.**
If a step always produces the same output from the same input (pulling a CRM record,
formatting a date, calculating a follow-up deadline), encode it in a script or a
structured template. AI handles the parts that require judgment and language.
Scripts handle the parts that must be consistent.

**Rule 5: Break orchestration skills into sub-skills.**
If a complex workflow can be decomposed into utility skills, decompose it. Sub-skills
are independently useful. When you update a sub-skill, every orchestration skill that
calls it automatically improves. This is how the OS compounds.

## Global, Workstation, Agent, and Application Skills

Skills are scoped to the level they're actually needed at — no broader, no narrower:

- **Global skills** apply across any workstation, agent, or application (e.g., voice
  check, session audit). Live in `00_Resources/Skills/`. Any tier can call them.
- **Workstation skills** apply only within a specific function's Workstation (e.g.,
  Revenue Ops cadence management, Client Delivery onboarding checklist). Live in
  `[Function Name]/Workstation/Resources/`.
- **Agent skills** apply only within a specific agent (e.g., the Post-Call Routing
  Agent's enrollment logic). Live in `[Function Name]/Agents/[Agent Name]/Resources/`.
- **Application skills** apply only within a specific application (e.g., a Career
  Compass research thread skill). Live in
  `[Function Name]/Applications/[Application Name]/Skills/`.

When deciding scope: start as narrow as possible. If a skill would genuinely be
useful outside the tier it was built for, promote it — move the file, update the
reference in the relevant CLAUDE.md. Don't pre-emptively make something global
because it might be useful elsewhere someday. Build it narrow, promote it when
the need is proven.

**Why narrow scoping matters, concretely:** a skill built for one function
clutters context for tiers that don't need it — Remy Gasill's own example is
a referral skill for a specific contact that has no place in a head-of-
marketing's context, even though it's a perfectly good skill for the function
it was built for. This is the actual cost of over-scoping a skill as global:
not just redundancy, but context noise for every tier that inherits it
unnecessarily.

## How a Skill Moves from Claude Code to a Workstation

A skill is always written and tested in Claude Code first (see Section 2). "Installing"
it means a literal file placement:

1. Write and test the skill file in Claude Code until it runs reliably on real inputs
2. If it's a **global skill**, the file goes in `00_Resources/Skills/`
3. If it's a **workstation skill**, the file goes in
   `[Function Name]/Workstation/Resources/`
4. Reference the skill in the relevant `CLAUDE.md` so the agent knows it exists and
   when to use it — a skill file sitting in a folder with no reference in CLAUDE.md
   will not be found or used
5. If the skill should run automatically rather than on request, attach it to a
   scheduled task in Cowork (see Section 9)

A skill is not "live" until both the file exists in the right folder and CLAUDE.md
references it.
