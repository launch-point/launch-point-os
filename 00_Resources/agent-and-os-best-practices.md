# Launch Point — Agent & OS Best Practices

*This document is the authoritative guide for designing, building, and improving every
part of the Launch Point AI operating system. Consult it before making any architectural
recommendation, designing any agent, writing any skill, or starting any build.*

**Scope note:** Launch Point OS covers everything related to the career coaching
business — every function, workstation, agent, and application described here exists
to serve that business. It is not assumed to be the only project in this GitHub
account or Claude environment. Anything unrelated to the career coaching business
(a side project, a different revenue stream, an unrelated agent or application) sits
entirely outside this structure as its own separate project — see Section 2, "Two
Different Questions About Repos," for the full reasoning.

*This document is a synthesis, not a replacement, for the source material below. Treat
it as the fast path to the right principle — but the full files carry detail, examples,
and nuance that didn't make it into this summary.*

**When to go to the source file instead of stopping at this document:** if a section's
guidance feels insufficient, thin, or like more detail would help you give a more
accurate answer or do more accurate work — don't guess or improvise from what's written
here. Go open the source file named in that section's `Source:` line and search it
directly. This applies any time the summary isn't enough, not just for edge cases.

### Source File Reference Table

| Source File | What It Covers |
|---|---|
| `Become_AI_Native_in_less_than_60_mins_-_Greg_Isenberg` | What AI-native operations look like, the consultant/agent shift, systems thinking |
| `Building_AI_Agents_that_actually_work_-_Greg_Isenberg` | The four components of an agent, agent design, what makes agents work |
| `How_Claude_Code_s_Creator_Starts_EVERY_Project_-_Austin_Marchese` | Plan mode, the six build steps, verification, Boris Churnney's actual workflow |
| `How_Anthropic_Employees_ACTUALLY_Use_Claude_Skills_-_Austin_Marchese` | The four skill types, the five skill-building rules, how Anthropic's own team writes skills |
| `I_got_a_private_lesson_on_Claude_Cowork___Claude_Code_-_Greg_Isenberg` | Cowork vs. Claude Code mechanics, Boris's parallel-session workflow, the three-environment model |
| `The_Field_Guide_to_Building_Products_with_AI.md` | The three-layer sequence, the knowledge layer, function vs. judgment work |
| `CLAUDE.md` | Current live root behavioral rules — the actual file this document's guidance produces |
| `MEMORY.md` | Current live facts, decisions, and active projects |
| `launch-point-os-context.md` | Build status, project backlog, what's live vs. planned |
| `voice-principles.md` | Authoritative voice guide for any drafted message or content |

---

## How to Use This Document

This guide is organized by decision type — not by tool. Find the section that matches
what you are trying to figure out, read it fully, and work from its principles.

**Who this document is for, and why that matters:** this document — and its
three companion files (`skills-protocol.md`, `launch-point-brain-deferred-spec.md`,
`file-organizer-skill-spec.md`) — is written for **human reference, not AI
execution.** It exists so Todd, or a future team member, or a future Claude
session in this same planning space can understand *why* a rule exists before
trusting it, extending it, or challenging it. No agent reads this file
mid-task. This is the design record, not the instruction set.

**Live execution files are the opposite, deliberately.** CLAUDE.md, MEMORY.md,
and skill files are what Claude actually reads during a session or a run —
they should be behavioral only, no reasoning, kept short (Section 6's 200-line
rule). Reasoning in a live file is dead weight Claude has to parse past to
find the actual instruction. Reasoning in *this* file is the entire point.

**This means the two file types get audited differently.** Don't apply
CLAUDE.md's length discipline to this document — a long section here isn't
automatically bloat the way a long CLAUDE.md section is. The failure mode to
watch for here is *findability*, not length: can a human locate the reasoning
they need quickly? That's why Section 8, 14A, and 17 became companion files —
not because reasoning is bad, but because a 400-line section buried in the
middle of a shorter document made its reasoning harder to find, not because
the reasoning itself was the problem.

**Sections:**
1. [What It Means to Be AI Native](#1-what-it-means-to-be-ai-native)
2. [The Three Environments — What Goes Where and Why](#2-the-three-environments--what-goes-where-and-why)
3. [How the Launch Point OS Is Structured](#3-how-the-launch-point-os-is-structured)
4. [The Four Components of Any Workstation, Agent, or Application](#4-the-four-components-of-any-workstation-agent-or-application)
5. [How to Start Any Build](#5-how-to-start-any-build)
6. [CLAUDE.md — Rules for Writing Behavioral Instructions](#6-claudemd--rules-for-writing-behavioral-instructions)
7. [MEMORY.md — Rules for Writing Context and Facts](#7-memorymd--rules-for-writing-context-and-facts)
8. [Skills — Where the Full Protocol Lives](#8-skills--where-the-full-protocol-lives)
9. [Scheduled Tasks — When and How to Use Them](#9-scheduled-tasks--when-and-how-to-use-them)
10. [MCPs — Tool Connection Rules](#10-mcps--tool-connection-rules)
10A. [Data Sensitivity — What Can Go Where](#10a-data-sensitivity--what-can-go-where)
11. [Approval Gates — The Non-Negotiables](#11-approval-gates--the-non-negotiables)
12. [Make.com and Claude Code — Roles and Integration](#12-makecom-and-claude-code--roles-and-integration)
13. [The Three-Layer Sequence — Build Order Matters](#13-the-three-layer-sequence--build-order-matters)
14. [The Knowledge Layer — How the OS Gets Smarter Over Time](#14-the-knowledge-layer--how-the-os-gets-smarter-over-time)
14A. [The Launch Point Brain — Deferred, See Separate Spec](#14a-the-launch-point-brain--deferred-see-separate-spec)
14B. [The Idea Inbox (Deferred — Independent of 14A)](#14b-the-idea-inbox-deferred--explicitly-independent-of-14a)
15. [Function vs. Judgment Work — What AI Should Own](#15-function-vs-judgment-work--what-ai-should-own)
16. [OS Maintenance — Keeping It Clean Over Time](#16-os-maintenance--keeping-it-clean-over-time)
17. [Handoff Protocol — The File Organizer Skill](#17-handoff-protocol--the-file-organizer-skill)

**Companion Files:** three sections got large enough to warrant their own
file, following the same "consult when needed" model as the project's other
reference files. Each is a full extraction, not a summary — the main document
keeps a short pointer in its place.

| File | Extracted from | Consult when... |
|---|---|---|
| `skills-protocol.md` | Section 8 | Designing, writing, or auditing any skill |
| `launch-point-brain-deferred-spec.md` | Section 14A | Todd is ready to initiate the Launch Point Brain build |
| `file-organizer-skill-spec.md` | Section 17 | Actually building the file organizer skill |

---

## 1. What It Means to Be AI Native

**Source: `Become_AI_Native_in_less_than_60_mins_-_Greg_Isenberg`, `The_Field_Guide_to_Building_Products_with_AI.md`**

Being AI native is not about using AI tools. It is about restructuring how work gets
done so AI is the default operator and humans are the managers and decision-makers.

**The three-sentence definition:**
> An AI native organization is one where people manage agents, agents can read and write
> to the company, and the company gets smarter over time.

Just using ChatGPT or Claude in chat mode does not make an operation AI native. The
shift is from asking AI questions to giving AI goals. Chat is question to answer. An
agent is goal to result.

**The operating principle:** Identify every repeatable task in the business. Ask: can AI
do this with a human approval gate? If yes, build it that way. Only keep humans in the
loop at genuine decision points.

**The systems thinking shift:** The goal is not to do the task. The goal is to design
the system that can consistently produce the task — repeatedly, reliably, and with
compounding improvement over time. Instead of reviewing a message draft, build a system
that drafts messages against voice principles and delivers them for approval. Instead of
one deliverable, a pipeline.

**Context is the competitive moat.** The more context an agent has — about the business,
the person, the history, the goal — the better its output. Investing in context files
(CLAUDE.md, MEMORY.md, resource files, voice principles) and skill files is the
highest-leverage activity in the entire OS build. Context compounds. Skills compound.
Prompt tweaks don't.

Skills encode process the same way context files encode knowledge — once written, a
skill runs reliably forever and improves with every correction. A workstation with
strong context files and no skills is a well-briefed employee who has to be
re-explained every task. A workstation with both is one that can execute
independently. Both layers are required.

**For Launch Point specifically:** Todd's OS is AI native when the morning reactive block
runs itself and Todd's attention goes entirely to leading calls, building relationships,
and coaching clients. Every build decision should move toward that state.

---

## 2. The Three Environments — What Goes Where and Why

**Source: `I_got_a_private_lesson_on_Claude_Cowork___Claude_Code_-_Greg_Isenberg`, `MEMORY.md`**

The Launch Point OS runs across three environments. Each one has a distinct role.
Putting the right work in the right environment is the most important architectural
decision in the OS. Get it wrong and you either build something that doesn't run
reliably or add unnecessary complexity to something that didn't need it.

The way to think about each environment is through a human analogy.

---

### The Three Environments as People

**Claude Chat (this Planning Space) — The Consultant**

Think of this as a trusted advisor who has full visibility into your business —
the context, the history, the strategy, the decisions. You come here to think,
design, plan, and decide. The consultant helps you figure out what to build and
how to build it.

**What "never executes" means specifically:** this space never builds out or ships
an agent, a skill, a workstation, or any piece of OS architecture. Architecture
decisions get designed here and handed to Claude Code to build. That boundary is
absolute.

It does not mean this space is hands-off every connected tool. A quick Slack
message, a Notion lookup, or a small one-off update during a planning conversation
is fine — those are incidental actions in service of the conversation, not OS
build-outs. The line is: building or modifying the system itself always happens
in Claude Code. Using a tool in passing while planning does not.

*This space is where the OS gets designed. Claude Code and Cowork are where it runs.*

---

**Cowork — The Scheduled Assistant**

Think of Cowork as an assistant you meet with regularly or who emails you on a
schedule. Two modes:

**Mode 1 — Scheduled delivery.** The assistant sends you something at a preset time
without you asking. The daily brief shows up at 9 AM. The pre-call prep lands before
a sales call. You didn't request it that morning — you set that expectation once and
it runs. You read it, make decisions, delegate what comes out of it.

**Mode 2 — On-demand session.** You walk over to the assistant's desk and say "let's
work through this." That's a workstation session. You initiate it, you work through
it together, and you delegate what comes out of it. The assistant produces work for
you to review. You decide what happens next.

In both modes, the work starts because Todd either showed up or a clock fired.
The assistant does not wake up on their own because something happened in the world.
They produce output for Todd to act on. They do not act independently.

---

**Claude Code — Where Agents Live**

Think of Claude Code as an employee who is always on. You don't schedule meetings
with them and they don't wait for you to walk over. They are watching the systems,
watching their inbox, and when something happens that falls in their job description,
they handle it — without being asked.

A call gets booked: they prep the brief. A form submits: they route it and start the
research. A prospect responds to a DM: they draft a reply. They work through it, then
they ping you for approval before anything goes out. You review, give feedback if
needed, they revise, then ship. But they initiated everything. You never had to ask.

**This is what "agent" means in this document, specifically.** An agent is the
autonomous, event-triggered thing that lives in Claude Code — not the Cowork
workstation, even though the workstation is also AI doing work on your behalf.
Reserving the word "agent" for this autonomous layer keeps the vocabulary precise:
when this document says "agent," it always means Claude Code's autonomous layer,
never a Cowork workstation. See "Function, Workstation, and Agent — The Three
Tiers" below for the full model.

**Note on terminology:** the source material (both Remy Gasill's and Boris
Churnney's transcripts) uses "agent" generically — any AI system with
context, memory, skills, and tools, regardless of trigger type. This
document's tier-specific definition (Agent = autonomous, event-triggered,
Claude Code only) is a deliberate Launch Point refinement, not something the
source material itself distinguishes. Citing these transcripts as direct
support for the tier-specific definition would overstate the alignment — the
underlying four-component model is well-grounded in the source material; the
tier-specific vocabulary on top of it is our own addition.

Claude Code is also where all building happens — new workstations, new skills, new
agents, new integrations. Even when what's being built is a workstation that will
run entirely in Cowork, the build itself always happens in Claude Code.

---

### Function, Workstation, Agent, and Application — The Four Tiers

These words get used loosely in everyday conversation, but they mean distinct
things in this OS, and mixing them up is what caused real confusion in earlier
drafts of this document. Keep them separate.

**Tier 1 — Function.** The actual business concern: Revenue Ops, Client Delivery,
Content & Marketing, and Admin & Ops. (Finance-related work currently sits inside
Admin & Ops rather than as its own function — revisit only if it outgrows that.)
A function is not software. It's the area of the business itself — it would
exist whether or not any AI touched it at all.

**Tier 2 — Workstation.** The Cowork-resident expression of a function. This is
"walking up to the employee's desk" — either because a scheduled meeting is
happening (a scheduled task firing) or because you initiated it ("let's work on
this together," a workstation session). A workstation is always Cowork. It is
never the autonomous, event-triggered layer. If something runs because you showed
up or a clock fired, it's workstation work, full stop.

**Tier 3 — Agent.** The Claude Code-resident expression of a function. Operates
independently, triggered by something happening in the world, and reports back to
you for approval before anything ships. An agent is always Claude Code. It is
never something you "open a session" with.

**Tier 4 — Application.** A deliverable-grade build that's too large and too
self-contained to be one agent. A hypothetical example: a client-facing
research and reporting tool with its own skills, its own multi-step
orchestrator, its own client-facing intake app, and multiple approval gates
of its own. It belongs to a function (say, Client Delivery — it's part of
what clients receive) but it isn't a single agent's job, it's a whole
sub-system with internal structure of its own. Applications live in the
`launch-point-os` repo like everything else — see "One Repo for Everything
Launch Point" below for the full rule.

**A function can have a workstation, agents, applications, or any combination —
and over time, the goal is for every function to have at least a workstation and
an agent layer.** Revenue Ops today has a workstation (the daily brief, DM
drafting) and a growing agent layer (post-call Kit sequence routing). Client
Delivery has a workstation and an application (Launch Link). None of these is
more "real" than the others. They're siblings under the same function, not the
same thing as each other, and none of them *is* the function — the function is
the umbrella; workstation, agent, and application are its possible expressions.

This resolves a question that comes up constantly when designing new work: "is
this Revenue Ops?" or "is this Client Delivery?" The right question is actually
"is this the *Revenue Ops function*, and if so, does this specific piece belong
in the workstation, an agent, or an application?" Function tells you whose job
this is. Workstation vs. agent vs. application tells you which tier and which
environment that specific piece runs in — and that's answered by the Decision
Rule below, asked fresh for every new piece of work, not inherited from whatever
the rest of the function happens to use.

### One Repo for Everything Launch Point — No Exceptions

This is a hard rule, not a case-by-case judgment call: anything having to do
with Launch Point, the career coaching business — every function, every
Workstation, every Agent, every Application — lives under the
`launch-point-os` repo. There is no deployment-based exception. An
Application having its own hosted interface, its own independent pipeline,
or genuinely separable code does **not** move it to a separate repo. It
still lives in `launch-point-os`, nested under its function's
`Applications/` folder, per the standard function-first structure.

**The only reason a repo is ever separate is that the work isn't Launch
Point at all.** If Todd builds a side project, a different revenue stream,
or anything that isn't part of the career coaching business, it is not a
Launch Point Application under any function, and it does not nest anywhere
in this structure. It is its own project, with its own repo, from the
start — even if it shares the same GitHub account. That's the only
question that determines repo placement: does this belong to the career
coaching business? Yes → `launch-point-os`. No → its own repo, full stop.

*Note: an earlier version of this document described a second, deployment-
based exception allowing a Launch Point Application to have its own repo.
That exception has been removed — it does not reflect how this OS is
actually organized. If any reference elsewhere still implies an Application
might warrant a separate repo, that reference is stale and should be
corrected to match this rule.*

---

### The Approval Gate Is What Makes Autonomy Safe

The autonomous employee analogy only works because there is a mandatory approval gate
before anything leaves the OS. Without it, an autonomous agent is a liability.
With it, the autonomy is the feature — Claude Code handles everything up to the
decision point, and Todd only touches what requires a human judgment call.

This is non-negotiable across all three environments. See Section 11 for full
approval gate rules.

---

### The Decision Rule

Apply this in order. Stop at the first match.

| What is this? | Tier / Environment |
|---|---|
| Designing, planning, or deciding what to build | **Claude Chat** (this space) |
| Runs at a fixed time daily or weekly, no external trigger | **Workstation** — Cowork, scheduled task |
| Starts when Todd opens a session and initiates it | **Workstation** — Cowork, on-demand session |
| Starts because something happened in an external system | **Agent** — Claude Code |
| Building a skill, workstation, agent, app, or integration | **Claude Code** (build environment, regardless of tier the output belongs to) |
| Needs to run without Todd present and without a preset time | **Agent** — Claude Code |

---

### What Lives Where — By Environment

**Claude Chat (Planning Space)**
- Architectural decisions and design sessions
- Drafting OS files (CLAUDE.md, MEMORY.md, skill files) for review before they're
  installed
- Deciding what to build next and in what order
- Any question that needs a thought partner before execution begins

**How Claude Chat hands work off to Claude Code — Boris-style, not a rigid
template.** When something here is ready to move from planning to execution,
name what's ready and where it goes, then trust Claude Code to read the
file's own context rather than pre-formatting every detail. This mirrors
Boris Churnney's actual pattern: he tags Claude on a GitHub issue or PR with
a short instruction, and Claude reads the surrounding repo context to know
what to do — no bespoke handoff document per fix. *"This is ready — [file/
decision]. It goes in [path]. Take it to Claude Code and have it build/merge
this."* Note: this is a Launch Point convention informed by Boris's real
pattern, not a rule dictated by any single source file.

**Cowork — Workstations**
- Daily scheduled tasks: morning brief, CRM cache pre-load, pre-call prep
- Workstation sessions: each function's `Workstation/` folder (Revenue Ops, Client
  Delivery, Content & Marketing, Admin & Ops)
- Session-based drafting: DM responses, follow-up messages, email drafts Todd requests
  during a working session
- Conversational reporting: anything Todd wants to talk through or get a summary of
  in real time
- Session-initiated skills: session audit, voice check, ICP scoring

*Every workstation is, by definition, entirely Cowork. There is no such thing as
"part of a workstation runs in Claude Code" — that part is a separate agent, not
an extension of the workstation. See "Function, Workstation, and Agent" above.*

**Claude Code — Builds and Agents**
- All skill building — every skill file is written and tested in Claude Code, then
  installed into the relevant workstation or referenced by the relevant agent
- All workstation construction — even though the finished workstation will run
  entirely in Cowork, building it happens here
- All agent construction, since agents are Claude Code-native by definition
- Event-triggered autonomous agent work: Calendly booking → pre-call prep email
  sequence drafted for prospect (Agent tier, approval-gated per Section 11), form submission →
  a client-intake Application's orchestrator, no-show → re-engagement draft
- The Revenue Ops post-call agent: reads the call transcript, detects the outcome,
  drafts the Kit sequence enrollment, and surfaces it for approval — this agent
  serves the Revenue Ops function but is not part of the Revenue Ops workstation
- A deliverable-grade Application's orchestrator: an autonomous agent,
  multi-step, runs through its own approval gates without Todd initiating
- An Application's client-facing intake interface, if it has one: a deployed
  web application separate from the orchestrator itself
- Any workflow that starts because Make.com fires a webhook

---

### The Output Delivery Pattern

Regardless of which environment produces the output, it always surfaces in one of
two places for Todd's review:

- **Cowork** — when Todd is present and the output is conversational or part of a
  working session
- **Slack** — when the output is autonomous and Todd needs to see it and approve
  before anything goes out

Claude Code never delivers output directly to a lead, client, or public channel.
It always routes to Slack or Cowork first.

---

### Applied to Launch Point — Concrete Examples

| Workflow | Tier | Why |
|---|---|---|
| Deciding what to build next | Claude Chat | Design and planning work |
| Drafting a new skill file for review | Claude Chat | OS design before installation |
| Daily sales brief | Workstation (Cowork) — scheduled | Fixed time, no external trigger |
| CRM cache pre-load | Workstation (Cowork) — scheduled | Fixed time, feeds the brief |
| Sales call brief on call mornings | Workstation (Cowork) — scheduled | Fixed time, internal-facing, delivers to Todd only |
| Pre-call prep email sequence | Agent (Claude Code) — Revenue Ops | Triggered by Calendly booking; outward-facing to prospect; approval-gated per Section 11 |
| DM drafting during morning block | Workstation (Cowork) — session | Todd initiates it |
| Writing and testing a new skill | Claude Code — build | All skill building lives here |
| Building a new workstation | Claude Code — build | All builds live here, even Cowork-bound ones |
| Calendly booking → call prep draft | Agent (Claude Code) | External event triggers it |
| A deliverable-grade Application's orchestrator | Agent (Claude Code) | Runs without Todd, event-driven |
| An Application's client-facing intake interface | Claude Code — build | It's an application |
| Post-call outcome → Kit sequence enrollment | Agent (Claude Code), drafts then waits for approval | External event triggers it; serves the Revenue Ops function but is a separate agent, not part of the workstation |
| Session audit at end of day | Workstation (Cowork) — session | Todd initiates it |

---

### The Traps to Avoid

**Trap 1: Building in Claude Code because it feels important.**
If the workflow runs on a fixed schedule or starts when Todd opens a session, it
belongs in Cowork regardless of how complex it is. The daily brief is powerful and
it lives entirely in Cowork.

**Trap 2: Expecting Cowork to respond to external events.**
Cowork cannot wake itself up because a prospect responded to a DM or a booking
came in. If the trigger is external and the response needs to be immediate and
autonomous, that's Claude Code territory.

**Trap 3: Building skills anywhere other than Claude Code.**
Skills are always built and tested in Claude Code first. They are then installed
into the appropriate workstation or agent. A skill written directly in Cowork without
being built and tested in Claude Code will not be reliable.

**Trap 4: Skipping the planning space.**
Anything complex enough to involve multiple components, a new integration, or a new
agent should be designed here first. Starting a Claude Code build without a plan
produces rework. The planning space exists to prevent that.

**Trap 5: Calling an agent — or an application — "part of the workstation."**
A workstation is always Cowork — that never changes. When a function grows an
autonomous piece (like Revenue Ops' post-call Kit routing) or a deliverable-grade
build (like Client Delivery's Launch Link), that piece is a separate agent or
application, not an extension of the workstation reaching into another
environment or a different tier. They're siblings under the same function, not
one thing spanning multiple tiers. Call the workstation a workstation, the agent
an agent, and the application an application — see "Function, Workstation,
Agent, and Application" above. When designing a new piece of work for an
existing function, don't assume it inherits whatever tier the rest of that
function's work happens to use — ask the Decision Rule fresh, and label the
result correctly.

---

## 3. How the Launch Point OS Is Structured

**Source: `CLAUDE.md`, `MEMORY.md`, `launch-point-os-context.md`, `Building_AI_Agents_that_actually_work_-_Greg_Isenberg`**

The OS is a folder structure of markdown files, organized by function first. Every
function gets its own top-level folder, and inside it, its possible expressions —
Workstation, Agents, Applications — sit as siblings. This means everything related
to a single function lives together, instead of being scattered across separate
top-level trees the way "Workstations" and "Agents" would be if filed as siblings
to each other instead of to the function.

Everything in this structure is designed in Claude Chat and built in Claude Code
(see Section 2). What differs is where the finished thing runs day to day: a
function's Workstation runs in Cowork; its Agents and Applications run in
Claude Code — but all of it, code included, lives inside the `launch-point-os`
repo (see Section 2, "One Repo for Everything Launch Point").

### The Folder Structure

```
Launch Point OS/
├── CLAUDE.md                    ← Root behavioral rules for the entire OS
├── MEMORY.md                    ← Root facts and context that change over time
│
├── 00_Resources/
│   ├── voice-principles.md      ← Authoritative voice guide — read before any message
│   ├── launch-point-context.md  ← Full business context (program, clients, journey)
│   ├── Skills/                  ← Global skills, shared across every function
│   └── [other reference files]
│
├── Revenue Ops/                 ← Tier 1 — the function
│   ├── Workstation/             ← Tier 2 — Cowork
│   │   ├── CLAUDE.md
│   │   ├── MEMORY.md
│   │   └── Resources/
│   └── Agents/                  ← Tier 3 — Claude Code
│       └── Post-Call Routing/
│           ├── CLAUDE.md
│           ├── MEMORY.md
│           └── Resources/
│
├── Client Delivery/
│   ├── Workstation/
│   │   ├── CLAUDE.md
│   │   ├── MEMORY.md
│   │   └── Resources/
│   ├── Agents/
│   │   └── Launch Link Outreach/    ← UNDER DEVELOPMENT
│   │       ├── CLAUDE.md            ← actively being built,
│   │       ├── MEMORY.md            ← not yet fully live. See
│   │       └── Resources/           ← Section 3's Worked Example for design detail
│   └── Applications/            ← Tier 4 — deliverable-grade builds
│       └── Launch Link/          ← UNDER DEVELOPMENT — the Lovable-built
│           ├── CLAUDE.md         ← client-connection tool
│           ├── MEMORY.md
│           └── Resources/        ← see Section 3's Worked Example
│
├── Content & Marketing/
│   ├── Workstation/
│   └── Agents/
│
└── Admin & Ops/
    ├── Workstation/
    └── Agents/
```

*Note: this entire function-first structure is the target state, not the current
live structure. The live OS will need to be reorganized to match it — every
existing workstation folder moves under its function, and `Agents/` and
`Applications/` subfolders get created as each function grows into them. This is
a deliberate rebuild, not an incremental patch. **Sequencing: Revenue Ops moves
first.** Once that migration is proven, the same pattern repeats for Client
Delivery, Content & Marketing, and Admin & Ops — one function at a time, not
all at once.*

### Every Function Gets the Same Three Possible Subfolders

- **`Workstation/`** — singular, since a function has at most one workstation.
  Contains the Cowork-resident CLAUDE.md, MEMORY.md, and Resources for that
  function's session-based and scheduled work.
- **`Agents/`** — plural, since a function can grow multiple autonomous agents
  over time. Each agent gets its own subfolder with its own CLAUDE.md, MEMORY.md,
  and Resources, following the same Four Components model (Section 4) as a
  workstation — the only difference is which environment runs the result.
- **`Applications/`** — plural, since a function can have more than one
  deliverable-grade build. Each application gets its own subfolder, structured
  the same way — including its code, since Applications live entirely inside
  the `launch-point-os` repo, no exceptions.

A new function doesn't need all three from day one. Most functions will start
with just a `Workstation/` and grow `Agents/` and `Applications/` as the OS
matures — but the folders exist as placeholders so there's never a "where does
this go" question when the first agent or application for that function gets built.

### Worked Example — When an Application and an Agent Pair Together

**Status: Launch Link is under active development — same status as Career
Compass, not a Deferred Design Spec.** This section previously treated Launch
Link as conceptual-only; that was incorrect. Both the Application (the
Lovable-built client-connection tool) and its paired Agent (autonomous outreach
drafting) are actively being built, not just proposed.

Launch Link (Client Delivery) is the working example of a function having an
Application and an Agent that depend on each other rather than standing alone.
The Application is the actual tool — a Lovable-built product Todd navigates
directly to do client connection work. The Agent is a separate, autonomous
piece that watches for triggers coming out of that Application (e.g., a
connection opportunity identified) and drafts the outreach communication —
then stops and waits for Todd's approval before anything sends, per Section 11.
The two are siblings, not nested inside each other, even though one effectively
depends on the other's output to know when to act.

**The general principle — evergreen, not tied to Launch Link's specific build
status:** build the Application's CLAUDE.md and the Agent's CLAUDE.md as two
separate identity statements, each describing only its own job. The Application
doesn't need to know how its Agent drafts messages, and the Agent doesn't need
to know how the Application's interface works — only what trigger it's
watching for.

**Remaining build-status questions** (tracking, not "should this be built" —
that's already decided):
- What exactly counts as a "connection opportunity" — something the
  Application surfaces explicitly, or something the Agent has to infer?
- What does the outreach draft actually look like — the same approval-gate
  format as other Revenue Ops/Client Delivery outreach, or does Launch Link's
  context require something different?
- Current build sequencing: confirm whether the Application or the Agent is
  further along, so this document's status note stays accurate as development
  continues.

### Naming Agent and Application Subfolders

Name the subfolder for what it does, not for the function it belongs to — the
function name is already the parent folder, so repeating it is redundant (the
same redundancy that exists today in `Workstations/Revenue Ops/Revenue Ops
Resources/`, which this restructure also corrects). `Revenue Ops/Agents/Post-Call
Routing/`, not `Revenue Ops/Agents/Revenue Ops - Post-Call Routing/`. Each
agent's or application's own CLAUDE.md states its full identity in its first few
lines, which is where disambiguation actually needs to live — the folder name's
only job is to be quickly scannable in a listing.

### Creating the Root CLAUDE.md, and New Workstations, Agents, and Applications

**On format: none of the source material mandates a specific CLAUDE.md
structure.** The explicit guidance from Anthropic's own team is the opposite —
"there's no special format, it's just a text file, format it however you
want." The structures below are **deliberate Launch Point conventions**, not
rules pulled from the transcripts. They're used because applying the same
structure every time makes every file predictable to build, read, and hand
off — consistency is the reason, not external authority. Once chosen, apply
it consistently; don't vary the shape from build to build without a real
reason to.

**Creating the Root CLAUDE.md** (grounded directly in the live file — this is
not a new invented structure, it describes what already exists; seven items):
- **Identity** — one paragraph: what the OS is, what it explicitly is not
  ("not a chatbot"), what it does, and the north star metric stated directly
  so every downstream decision can be weighed against it
- **Memory System** — the read-MEMORY.md-at-start rule, the "remember this"
  rule, and the CLAUDE.md-vs-MEMORY.md decision test stated as a live
  behavioral instruction ("when unsure, ask Todd"), not as reasoning —
  reasoning for this test lives in Section 6/7 of this document, not in the
  live file itself
- **Resources table** — same pattern as every other tier: file → when to
  read it. Root's real table includes voice-principles.md,
  launch-point-context.md, agent-and-os-best-practices.md, and MEMORY.md
  (read at start of every session)
- **Non-Negotiables** — flat bullet rules with no reasoning attached, each
  one a trigger-and-action pair (see Section 6's behavioral-vs-philosophical
  distinction). This is where Bucket 2 items land once merged — see
  `proposed-claude-md-additions.md`. **Before merging, check for overlap**:
  the live file already has its own approval-gate rule and its own
  voice-principles.md-before-drafting rule — don't create a duplicate
  alongside the existing one, consolidate into a single clear version
- **Todd's Weekly Rhythm** (or equivalent) — pure business-specific
  scheduling/context facts that inform prioritization; this is the one
  section that's genuinely unique to Launch Point rather than a
  transferable pattern
- **Routing Map** — trigger keyword → workstation path, one row per
  function's Workstation only (see "The Routing System" below — Agents and
  Applications don't get routing rows, they activate differently)
- **Creating New Workstations/Agents/Applications** — this checklist itself
  lives in the root CLAUDE.md, since it's what Claude reads to know how to
  build the next tier when asked

**Creating a new Workstation** (existing process, unchanged):
- **CLAUDE.md** with: Identity, Resources table, Workflow steps, Editorial Rules
  (always starts with "Follow voice-principles.md in 00_Resources")
- **MEMORY.md** with: header, Contacts section, Key Decisions section
- **Resources/** — empty folder
- After creating, add a row to the root CLAUDE.md's Routing Map

**Creating a new Agent:**
- **CLAUDE.md** with: Identity (must state the specific external trigger this
  agent watches for — not just what function it serves, but what event wakes
  it up), Resources table, Trigger Behavior (what happens on wake, what gets
  drafted, **and a verification step before drafting is considered
  complete** — the agent checks its own output against voice-principles.md
  or a relevant verification skill before surfacing anything to Todd; this
  is the real pattern the source material confirms: TRIGGER → AGENT EXECUTES
  → VERIFY → DRAFT TO SLACK → TODD APPROVES → SEND — skipping the verify
  step is what separates a merely-configured agent from a compounding one),
  Approval Gate Pointer (a brief confirmation that this agent inherits the
  root CLAUDE.md's approval gate rule — not a full restatement. Per Section
  6's inheritance principle, lower-tier files shouldn't repeat root rules in
  full; a one-line pointer like "follows root's approval gate — nothing
  ships without Todd's sign-off" is sufficient. Reserve fuller detail only
  for anything genuinely agent-specific about how *this* agent's approval
  moment works, e.g., what it means for a Kit enrollment specifically)
- **MEMORY.md** with: header, relevant contacts or context specific to this
  agent's job, Key Decisions section
- **Resources/** — empty folder
- Agents are not added to the root CLAUDE.md's Routing Map, since they aren't
  triggered by typing a keyword — they activate on event, so there's nothing
  to route to by name. If Todd needs to reference or check on an agent
  conversationally, that happens through the relevant function's Workstation
  session, not through a direct keyword trigger to the agent itself.

**Creating a new Application:**
- **CLAUDE.md** with: Identity (states what the application is and does — if
  the application has its own deployed interface, like Launch Link, say so
  explicitly), Resources table listing each skill with its
  **skill type** stated explicitly (utility / verification / enrichment /
  orchestration — see `skills-protocol.md` for the four types — an
  Application's skills work together as an orchestrated system, per the real
  "generate report" pattern from the source material: enrichment → drafting →
  verification, chained under one orchestration skill; naming each skill's
  type makes that relationship explicit instead of implied by file names
  alone), Workflow steps, Editorial Rules if it produces any client-facing
  content, and a
  pointer to the **Audit Skill Pattern** (see `skills-protocol.md`) if the
  application runs repeatedly with variable output —
  this is the Application tier's equivalent of the verification step
  required in an Agent's Trigger Behavior, expressed as cross-run pattern
  detection rather than a per-draft check
- **MEMORY.md** with: header, Contacts section, and Current Decisions —
  for a simple application this may be a flat list (see Section 7's
  update-in-place pattern), but a complex, actively-developed application
  may need an evolving build-spec structure instead: named subsections for
  architecture decisions, repo/folder structure, and build phases, updated
  as the build progresses rather than logged as a flat list. Match the
  structure to the application's actual complexity — don't force a simple
  application into unnecessary subsections, and don't force a complex one
  into a flat list that can't hold its real build state.
- **Skills/** — empty folder (applications get their own Skills folder, distinct
  from a workstation's Resources folder, since applications typically have
  multiple distinct skills rather than one body of resources — see
  `file-organizer-skill-spec.md`'s routing table)
- **Resources/** — empty folder
- If the application pairs with an Agent (see "Worked Example" above), build
  the Agent as a separate sibling, not as part of the Application's CLAUDE.md
- This process is new, has not yet been used to build a real application, and
  should be expected to need adjustment once the first one (Launch Link) is
  actually built against it — per Section 16's maintenance
  principle, refine it from real use, not in the abstract.

### The Routing System

The root `CLAUDE.md` routes Todd to the right function's workstation based on
trigger keywords and task type. Routing must be explicit and unambiguous. Use
trigger keywords, not slash commands (slash commands conflict with Cowork's
skill system).

| Trigger | Function | Routes To |
|---|---|---|
| `revops` | Revenue Ops | `Revenue Ops/Workstation/` |
| `clients` | Client Delivery | `Client Delivery/Workstation/` |
| `content` | Content & Marketing | `Content & Marketing/Workstation/` |
| `admin` | Admin & Ops | `Admin & Ops/Workstation/` |

This table only routes to workstations, since workstations are the only tier
Todd enters by typing a trigger keyword — agents and applications activate by
event trigger (agents) or by being invoked from within a workstation or directly
in Claude Code (applications), not by keyword routing.

When a new function or workstation is built, add its trigger to the root routing
table immediately.

---

## 4. The Four Components of Any Workstation, Agent, or Application

**Source: `Building_AI_Agents_that_actually_work_-_Greg_Isenberg`**

Every working workstation, agent, or application has exactly four components —
the underlying model is the same across tiers (see Section 2, "Function,
Workstation, Agent, and Application"). If any one is missing, it will
underperform or fail. Design for all four before building anything, regardless
of which tier you're building.

### Component 1: Identity (CLAUDE.md)
Who it is, what it owns, what it does not own. Written in CLAUDE.md. Behavioral
rules. Prescriptive and stable — does not change often.

- **One lane per workstation or agent.** Something that tries to own everything owns
  nothing well.
- Identity includes: what it does, what it never does, and what it defers.
- Rules should be behavioral: "Always do X before Y." "Never post publicly." Not
  philosophical: "Try to be helpful."

### Component 2: Memory (MEMORY.md)
What it knows and remembers across sessions or runs. Written in MEMORY.md. Facts,
context, decisions, contacts, current status. Descriptive and dynamic — updated
frequently.

### Component 3: Skills
Step-by-step playbooks for repeatable tasks. Once a skill is written, the agent runs
it reliably every time without re-explanation. Skills are how workstations, agents,
and applications get dramatically better over time — they encode process the same
way MEMORY.md encodes facts.

**The agent doesn't automatically know its skills exist.** Skills are registered in
the workstation's or agent's CLAUDE.md — specifically in the resource loading and
trigger description sections. A skill file sitting in a Resources folder with no
CLAUDE.md reference is invisible to the agent. The CLAUDE.md reference isn't a
summary of what the skill does — it's a trigger condition that names who the skill
serves and when it should fire, written in the language Todd would actually use to
invoke it. This is how the agent learns to reach for a skill at the right moment
rather than waiting to be told.

See `skills-protocol.md` for the full skill-building protocol, including the four
types, the five rules, how skills are created, and how they get installed and
referenced. Section 8 in this document has a short overview and points there.

### Component 4: Tools (MCPs)
The external systems the agent can read from and write to. Only connect what the agent
actually needs. More tools is not better — it adds noise and increases error surface.

**The key distinction between CLAUDE.md and MEMORY.md:**
- `CLAUDE.md` = behavioral rules. "Always read voice-principles.md before drafting any
  message." Prescriptive. Doesn't change unless the behavior should change.
- `MEMORY.md` = facts and context. "Current pipeline stage for [prospect] is Call
  Offered." Descriptive. Updates constantly as reality changes.

---

## 5. How to Start Any Build

**Source: `How_Claude_Code_s_Creator_Starts_EVERY_Project_-_Austin_Marchese`**

### The Core Principle: Move Slow to Move Fast

Start 80% of sessions in **plan mode** before writing a single line of code or
instruction. The problem Claude thinks it should be solving and the problem you actually
want it to solve are not always the same thing. A bad plan produces rework. A good plan
makes execution nearly automatic.

**How to enter plan mode in Claude Code:** Shift + Tab twice in the terminal.

**The interview prompt:** Before any build begins, use this:
> *"Before building anything, ask me what I'm trying to solve, who it's for, what
> success looks like, and what it should NOT do. Summarize it back to me before you
> write any code."*

This forces the agent to surface gaps in your assumptions before they become bugs.
Go back and forth until the plan is solid. Then let it build.

### Babysit Early Runs

Watch the first few executions of any new build closely. Correct fast, then let it run.
Early mistakes uncorrected become habits baked into the system. Never schedule a task
that hasn't been manually tested first.

### The Six Steps for Every Build

1. **Plan first.** Use plan mode. Interview the problem before touching the build.
2. **Minimal CLAUDE.md.** Write only what the model actually needs. Less is more.
3. **Build in verification.** Give the agent a way to check its own output.
4. **Partition sessions.** Two context windows that don't know about each other get
   better results on hard problems. Open a fresh session for a fresh perspective.
5. **Systemize inner loops.** Any repeatable task should become a skill. Two
   ways this happens: Todd asks for one directly mid-session, or Claude
   recognizes the pattern and prompts. The full rule — what signals Claude
   watches for, and the exact behavior — is the Skill-Recognition Rule
   (Section 6), which lives as a live instruction in
   `proposed-claude-md-additions.md`, not here. Either way, the first-draft
   skill moves to Claude Code for testing and refinement before it's installed
   (see `skills-protocol.md`).
6. **Build for next year's models.** Don't over-engineer for current limitations. Models
   get better continuously. Invest in context and process, not prompt micro-tuning.

### The Verification Protocol

Before any build ships, ask Claude: *"Please go back and verify all of your work so far.
Make sure you used best practices, were efficient, and did not introduce any issues."*

Give Claude a way to verify its output against a clear standard. If there is no clear
way to verify something, redesign the task before building it.

**The fresh-session test.** After building or substantially updating a
Workstation, Agent, or Application's CLAUDE.md and MEMORY.md, open a
completely new session in that same folder with no prior context and ask a
basic identity question ("who are you, what do you own") or attempt a
routine task. If the agent doesn't know something it should, the context
files are incomplete — fix them before considering the build done. This is a
cheap, concrete test the source material shows both Remy Gasill and Boris
Churnney using directly, and it's the most reliable way to catch a context
gap before Todd hits it mid-task.

---

## 6. CLAUDE.md — Rules for Writing Behavioral Instructions

**Source: `How_Claude_Code_s_Creator_Starts_EVERY_Project_-_Austin_Marchese`, `Building_AI_Agents_that_actually_work_-_Greg_Isenberg`, `CLAUDE.md`**

### What CLAUDE.md Is For

CLAUDE.md is a behavioral instruction file — not a knowledge dump. Every line should
answer the question: *what should the agent always do or never do?*

### Rules for Writing CLAUDE.md Files

**Keep it focused and under 200 lines.** The more instructions you give, the more likely
the model is to get confused and miss the ones that actually matter. Boris Churnney's
recommendation is blunt: if it's getting too long, delete it and start fresh. A middle
path: regularly prune anything that the current model no longer needs. Run: *"Update my
CLAUDE.md to remove anything no longer needed, contradictory, duplicate, or unnecessary
bloat."*

**Rules should be behavioral, not philosophical.**

| ✅ Good | ❌ Bad |
|---|---|
| "Always read voice-principles.md before drafting any message." | "Try to write in Todd's voice." |
| "Never post to LinkedIn. Todd posts all content himself." | "Be thoughtful about what gets published." |
| "Before sending any message, present the draft with the last 2–3 messages of context and wait for explicit approval." | "Make sure Todd approves things before they go out." |

**Behavioral rules have a clear trigger and a clear action.** Vague instructions leave
the agent guessing.

**Every CLAUDE.md file contains at minimum:**
1. Identity — who this is and what it owns
2. What it never does (non-negotiables)
3. How memory works (what gets written to CLAUDE.md vs. MEMORY.md)
4. Resource loading rules (which reference files to read and when — e.g., "always
   read voice-principles.md before drafting any message"). This is distinct from
   skill references, which are trigger conditions, not loading rules. A CLAUDE.md
   should also list each installed skill with its trigger description — the
   specific situation and language that should cause the agent to invoke it.
   These are two separate entries, not one: resource loading tells the agent what
   to read; skill triggers tell the agent what to do.
5. Connected tools

**The always-load folder technique, underlying item 4:** when there's too
much context to fit cleanly into one CLAUDE.md, the source material's pattern
is to keep that context in a separate folder and add one explicit instruction
to CLAUDE.md: "before doing any task, read [folder] to understand the
business." Without that explicit instruction, a folder sitting nearby is not
loaded automatically — the CLAUDE.md has to say so. This is exactly what
Root's Resources table already does (voice-principles.md,
launch-point-context.md) — naming the technique makes clear the Resources
table isn't optional decoration, it's the actual loading mechanism, and a
resource left off that table simply won't be read.

This is the minimum for a Workstation, Agent, or Application CLAUDE.md — **not
Root**, which has its own distinct structure (see Section 3, "Creating the
Root CLAUDE.md, and New Workstations, Agents, and Applications"). Root's
"Connected tools" equivalent is the global Section 10 reference table, not a
per-file list — Root doesn't own tool connections the way a Workstation does,
it just needs Todd's OS-wide rules and the routing map. Root shares Identity,
Non-Negotiables, and Memory System with this five-item list, plus its own
Resources table — but does not have a Connected Tools list (see above) and
has three additional items no other tier needs: Todd's Weekly Rhythm, the
Routing Map, and the Creating-New-Tiers checklist itself (see Section 3's
Root checklist for the complete seven-item structure). Agents and
Applications require additional items specific to their tier beyond this
five-item minimum — see the same Section 3 reference for full per-tier
requirements. In particular: an Agent's CLAUDE.md must also state its specific
trigger event and include an explicit Approval Gate Statement; an
Application's CLAUDE.md must also state its skills' Section 8 types explicitly.

**Update CLAUDE.md when the same mistake happens twice.** Not before. Boris's rule:
if Claude hasn't done it wrong yet, you don't need a rule against it yet.

### Root vs. Workstation CLAUDE.md Files

The root CLAUDE.md sets the rules that apply everywhere — across every workstation,
every session. Workstation, Agent, and Application CLAUDE.md files inherit root rules
and add function-specific behavior. Workstation files should not repeat root rules.
If a rule belongs everywhere, it belongs in root. If it belongs only in one
workstation, it belongs there.

### The Skill-Recognition Rule — Root CLAUDE.md Only, Applies Everywhere

This is a behavioral rule, not architectural reasoning — it belongs in the root
CLAUDE.md, inherited by every workstation, agent, and application, never
restated in a lower-tier file. **The rule itself lives in
`proposed-claude-md-additions.md`, ready to merge into the live root CLAUDE.md.**

**Why it exists:** without it, a skill only gets created when Todd happens to
think to ask for one — which means most repeatable processes never become
skills at all, and the OS's compounding advantage (Section 1, Section 14)
never materializes. The rule shifts the burden onto Claude to notice the
pattern, since Todd is focused on the task, not on whether the task is
skill-worthy.

**Why it's scoped to interactive sessions only:** an autonomous Claude Code
agent has no one to ask — there's no session, no Todd present to respond. The
rule can only fire where a live back-and-forth is possible.

**Status: now live in root CLAUDE.md** (merged as of this session).

---

## 7. MEMORY.md — Rules for Writing Context and Facts

**Source: `Building_AI_Agents_that_actually_work_-_Greg_Isenberg`, `MEMORY.md`**

### What MEMORY.md Is For

MEMORY.md stores facts that could change. Not behavioral rules — those go in CLAUDE.md.
Facts: contacts, pipeline status, active projects, decisions made, preferences, things
to remember.

The test: *"Could this be different tomorrow?"* If yes, it goes in MEMORY.md.

### What Gets Written to MEMORY.md

- Contacts and their roles, communication channels, notes
- Active project status and decisions made
- Decisions deferred and why
- Preferences that have been stated but could change
- Anything Todd says "remember this" about

### What Does Not Go in MEMORY.md

- Behavioral rules (→ CLAUDE.md)
- Static reference content like the interview accelerator program description
  (→ launch-point-context.md or a resource file)
- Architecture decisions that are now permanent (fold into CLAUDE.md as rules)

### Protocol for Updates

When Todd says "remember this," write it to MEMORY.md immediately and confirm.
Do not hold it until the end of the session.

When a decision is finalized and will not change, consider moving it from MEMORY.md
into the relevant CLAUDE.md as a behavioral rule. MEMORY.md stays focused on what's
current and dynamic.

### Structure

Same note as CLAUDE.md's structure (Section 3): the source material shows a
real MEMORY.md example that's much simpler than what follows — a few lines
stating "this is what you've learned over time," updated in place when
something changes. There's no mandated schema.

The six-part structure below is a **Launch Point convention, chosen for
consistency** across every MEMORY.md the same way CLAUDE.md's structure is —
not a rule from the source material. Once chosen, apply it the same way every
time so every MEMORY.md is predictable to read and update.

Every MEMORY.md should have:
1. Last updated date
2. Active Projects section
3. Key Contacts section
4. **Current Decisions** (what's currently decided and why — updated in place
   per the real source example: replace outdated entries, don't just
   accumulate. This is *not* the same thing as the Architecture Decisions Log
   in Section 14A — that's a separate, standalone, OS-wide file, deliberately
   append-only and historical. This section is small, per-function, and
   reflects only what's currently true. Don't confuse the two names.)
5. Things to Remember (standing preferences, notes from Todd)
6. Deferred items (not abandoned — explicitly parked for later)

---

## 8. Skills — Where the Full Protocol Lives

Skills are step-by-step playbooks for repeatable tasks — see Section 4 for
how they fit alongside CLAUDE.md, MEMORY.md, and MCPs as one of the four
required components of any workstation, agent, or application.

**The full skill-building protocol — four types, five build rules, the two
creation methods, maturity levels, verification patterns, and the Audit
Skill Pattern — lives in `skills-protocol.md`.** Consult it whenever actually
designing, writing, or auditing a skill. Don't rely on general knowledge for
this; the protocol reflects specific, source-grounded guidance that general
knowledge won't reproduce.

The one thing every other section needs to know without opening that file:
skills are registered in the relevant CLAUDE.md's resource loading and
trigger description sections — a skill sitting in a folder with no CLAUDE.md
reference is invisible to the agent (see Section 4).

---

## 9. Scheduled Tasks — When and How to Use Them

**Source: `How_Claude_Code_s_Creator_Starts_EVERY_Project_-_Austin_Marchese`, `Building_AI_Agents_that_actually_work_-_Greg_Isenberg`, `I_got_a_private_lesson_on_Claude_Cowork___Claude_Code_-_Greg_Isenberg`**

### Scheduled Tasks Live in Cowork

Scheduled tasks are Cowork's first mode of operation (see Section 2, "Mode 1 —
Scheduled delivery"). They do not live in Claude Code, even though the skill they run
was built and tested there. Once a skill is installed (see `skills-protocol.md`),
attaching it to a schedule is a Cowork configuration step, not a code build.

### What Scheduled Tasks Are

Scheduled tasks are cron jobs attached to a skill. They fire at a set time and run a
skill automatically, without Todd initiating anything. This is where the OS moves from
reactive to proactive — but it is still schedule-driven, not event-driven. A scheduled
task cannot fire because something happened in the outside world; that is Claude Code's
job (see Section 2). A scheduled task only fires because a clock hit a set time.

*Example:* The daily sales brief fires at 9:00 AM, pulls the CRM cache, surfaces
today's priority actions, and drops a summary into Cowork for Todd to review.

### The Two Rules for Scheduled Tasks

These are behavioral rules, not reasoning — they belong in the root CLAUDE.md.
**Both rules live in `proposed-claude-md-additions.md`, ready to merge.**

**Why Rule 1 (never schedule untested) exists:** a scheduled task runs
unattended, repeatedly, without Todd present to catch a failure the first
time it happens (see Section 5's "Babysit Early Runs"). Scheduling before
testing means every bug in the skill runs on autopilot until someone notices.

**Why Rule 2 (approval gates still apply) exists:** automation removes Todd
from the loop on *when* something runs, but Section 11's approval gate is
about *what* leaves the OS — those are separate questions. A scheduled task
being reliable doesn't make its output safe to send without review; the two
concerns don't cancel each other out.

### Use Cases for Scheduled Tasks in Launch Point

- **Daily sales brief (Revenue Ops)** — fires Mon–Fri before Todd's morning block.
  Covers pipeline, general ops, and priority actions for the day. Does not include
  call-specific prep — that is a separate skill (see Sales call brief below).
- **CRM cache pre-load (Revenue Ops)** — fires just before the daily brief to ensure
  data is current
- **Sales call brief (Revenue Ops)** — fires on call mornings (Tuesday and Wednesday
  per Todd's weekly rhythm), pulls everything Todd needs for that day's calls from
  multiple sources (CRM, Calendly, LinkedIn, etc.), delivers it to Todd in Cowork.
  This is a separate scheduled task from the daily sales brief — internal-facing,
  goes to Todd only. *Not yet built as a standalone skill — currently embedded in
  the daily sales brief. Target state is a dedicated skill and scheduled task.*
- **Alumni check-in reminder (Revenue Ops)** — fires on cadence to surface who is
  due for a touch. Drafts the outreach and presents it for approval (per Rule 2 and
  Section 11) — it does not send anything on its own.

---

## 10. MCPs — Tool Connection Rules

**Source: `Building_AI_Agents_that_actually_work_-_Greg_Isenberg`, `CLAUDE.md`**

### What MCPs Are

MCPs (Model Context Protocol connectors) are the tools an agent can use to read from
and write to external systems. They give the agent hands.

Without MCPs, an agent can only process text. With MCPs, it can check your calendar,
read a CRM record, send a message, look up a prospect's LinkedIn, or update a task.

### Rules for Connecting MCPs

**Only connect what the agent actually needs.** More connections is not better. Every
tool added increases noise and surface for errors. Connect the MCP only if the
workstation will actively use it.

**Global MCPs vs. workstation MCPs.** MCPs that are needed everywhere (e.g., Gmail,
Slack) can be global. MCPs that are only relevant to one workstation (e.g., Kondo for
LinkedIn in Revenue Ops) should be scoped there.

**Scope is about which function owns the tool, not which tier or environment uses
it.** A function's MCP scope applies to its Workstation and any of its Agents or
Applications equally (see Section 2, "Function, Workstation, Agent, and
Application"). Kit, for example, is scoped to the Revenue Ops function — both
the Workstation's Cowork-side email sequence work and the Post-Call Routing
Agent's Claude-Code-side enrollment use the same Kit connection, because both
belong to Revenue Ops as a function, not because either one is "really" Revenue
Ops and the other isn't.

**Know the quirks before you use them.** Each tool has failure modes and limitations.
For Launch Point, these are documented in `tool-configs.md` in Revenue Ops Resources.
Before writing a skill that calls a tool, check that file.

### Launch Point's Connected Tools (as of June 2026)

| Tool | Primary Use | Scope |
|---|---|---|
| Kondo | LinkedIn DMs — read and send (post-connection only) | Revenue Ops |
| Quo | SMS to prospects and clients | Revenue Ops |
| Gmail | Email (two work accounts) | Global |
| Google Calendar | Scheduling and prep | Global |
| Calendly | Booking follow-up and no-show tracking | Revenue Ops |
| Notion | CRM — prospect, client, and alumni records | Revenue Ops + Client Delivery |
| ClickUp | Content task management and EA handoffs | Content & Marketing |
| Kit | Email nurture sequences, workshop emails, and post-call outcome routing | Revenue Ops (may expand to other workstations later — reassess scope if that happens) |
| Slack | Internal team communication | Global |
| Circle | Community engagement | Client Delivery |
| Google Sheets | Master Data Sheet (Client Delivery Application builds) | Client Delivery |
| Perplexity | Deep research API (Application orchestrator use) | Client Delivery |

---

## 10A. Data Sensitivity — What Can Go Where

**Source: `The_Field_Guide_to_Building_Products_with_AI.md` (Chapter IX)**

Not all of Launch Point's data carries the same risk if mishandled. Three
tiers, adapted from the Field Guide:

| Tier | What it covers | Rule |
|---|---|---|
| **Green** | Public or non-sensitive — general business content, published materials | Any connected AI tool |
| **Yellow** | Internal/business-sensitive — pipeline data, internal strategy, most CRM content | Enterprise-tier tools only, zero data retention |
| **Red** | Regulated or client-identifying — client call recordings, personal details shared in coaching, anything client-consent-dependent | Requires explicit handling rules before automated processing — this is exactly the open question in Section 14A's Branch 2 (sales call consent) |

This tier system doesn't resolve Section 14A's open consent question — it
gives that question a framework to be answered inside, rather than being
decided from scratch. Any new Application or Agent that touches client data
should state which tier it operates in as part of its CLAUDE.md Identity
section.

---

## 11. Approval Gates — The Non-Negotiables

**Source: `CLAUDE.md`, `MEMORY.md`, `launch-point-os-context.md`**

### The Core Rule

Nothing goes to a lead, a client, or the public without Todd's explicit approval.
No exceptions. This rule is the most important non-negotiable in the entire OS.

The approval gate is not a nice-to-have check. It is the fundamental structure that
makes autonomous operation safe. Removing it to "save time" is not an optimization —
it breaks the system.

### What Requires Approval

- Every message on every channel: LinkedIn DM, Quo SMS, email, Circle post
- Every CRM update that affects a record's status
- Every content piece before it leaves the OS
- Any action that cannot be undone (booking, posting, sending)

### How the Approval Gate Works

The execution steps are a behavioral rule, not reasoning — they belong in the
root CLAUDE.md. **The four steps live in `proposed-claude-md-additions.md`,
ready to merge.**

**Why context (last 2–3 messages) is included with every draft:** a draft
shown in isolation can look correct but be wrong for the actual conversation
it's replying to — tone, timing, or a detail the prospect just mentioned.
Context is what lets Todd approve in seconds instead of having to reconstruct
the thread himself before he can judge the draft.

**Why ambiguous approval means don't send:** "looks fine" or a non-answer is
not the same as "yes, send it." Treating ambiguity as approval would quietly
erode the entire gate over time — Section 11's gate only holds if it means the
same thing every time.

**Where the draft is presented depends on which environment produced it** (see
Section 2, "The Output Delivery Pattern"): Cowork session if one is open,
Slack if the draft was produced autonomously with no session present.

### What Does Not Require Pre-Approval

- Reading from any tool (CRM lookups, calendar checks, LinkedIn reads)
- Drafting (produces text for Todd to review — no action taken)
- Summarizing or analyzing (internal to the session)
- Updating `MEMORY.md` (internal OS file)

### LinkedIn-Specific Rule

Todd writes and posts all LinkedIn content himself. Claude may draft for review. Claude
never publishes to LinkedIn. This is a hard line.

### The Approval Gate in Automated/Scheduled Tasks

Scheduled tasks can research, draft, and surface. They cannot send or post. The output
of any scheduled task is always delivered to Todd for review before anything leaves the
OS. Automation handles preparation. Todd handles release.

### When an Approval Gate Can Be Removed

An approval gate is removed only when Todd has approved the same type of output
20+ times without editing it — not before, and not because it feels routine or
low-stakes. This is a concrete, source-grounded threshold, not a judgment call
made case by case. Until that threshold is hit, every instance of that output
type still goes through the gate, no matter how many times it's already been
approved unedited.

This also connects the approval gate to Section 14's compounding pattern: every
time Todd edits a draft before approving, that edit is a lesson — captured in
MEMORY.md or the relevant skill's gotcha list, per `skills-protocol.md`'s
Rule 2. The gate isn't just a safety check; it's the mechanism that teaches
the agent what "good" looks like for that specific output, which is why the
20+ threshold exists — it's evidence the agent has actually learned the
pattern, not just a
count of approvals.

This rule is a deliberate exception to Section 14A's caution around
autonomy and Todd's standing instruction that nothing ships without approval
"for now, no exceptions." Do not apply this threshold, or treat any gate as
eligible for removal, without Todd explicitly initiating that conversation —
this section documents the rule's existence and its bar, not a standing
authorization to act on it.

---

## 12. Make.com and Claude Code — Roles and Integration

**Source: `MEMORY.md`, `launch-point-os-context.md`**

### This Is How the Autonomous Employee Wakes Up

Section 2 establishes that Claude Code is the autonomous employee — it responds to
the world, not to Todd's calendar or a clock. Make.com is the mechanism that makes
that possible. Make.com is the nervous system; it watches external systems for events
and is what actually fires the signal that wakes Claude Code up. There is no separate,
third trigger pattern here — Make.com → Claude Code is simply what "an external system
event" (Section 2's decision rule) looks like mechanically.

### They Are Complementary, Not Competing

Make.com and Claude Code handle different jobs. Confusing their roles leads to building
the wrong thing in the wrong place.

| Make.com | Claude Code |
|---|---|
| Event-driven triggers | Judgment, context, drafting |
| Routing based on conditions | Nuanced decisions that require reading context |
| Data piping between tools | Language generation and synthesis |
| Webhooks and API calls | Multi-step orchestration with approval gates |

### The Mature Pattern

Make.com detects an event → triggers Claude Code → Claude drafts output → delivers to
Slack for Todd's approval. Output lands in Slack, not Cowork, because this workflow
runs without Todd present (see "The Output Delivery Pattern" in Section 2) — Todd
isn't in a session to see it appear there.

*Example in production:* When a sales call is booked in Calendly, Make.com fires two
things: (1) it updates the Notion CRM stage to "Call Booked" — deterministic,
no judgment needed, Make.com handles this directly — and (2) it triggers the
**Pre-call prep email Agent** in Claude Code, which drafts the prospect-facing email
sequence based on the time between signup and call date, then surfaces it in Slack
for Todd's approval before anything sends. Note that this is distinct from the
**Sales call brief** (Section 9), which is a separate Workstation scheduled task that
fires on the morning of the call and delivers internal prep to Todd in Cowork — same
prospect, two different workflows, triggered differently and facing different
directions.

### When to Build in Make.com vs. Claude Code

**Build in Make.com when:** The logic is condition-based and the output is deterministic.
"If Calendly booking, then update Notion stage." No judgment required. Always the same
action.

**Build in Claude Code when:** The task requires reading context, generating language,
making a nuanced decision, or running a multi-step process with approval gates in the
middle.

**When both apply:** Make.com handles the trigger and routing. Claude Code handles the
work. Make.com fires the webhook. Claude Code runs the skill.

---

## 13. The Three-Layer Sequence — Build Order Matters

**Source: `The_Field_Guide_to_Building_Products_with_AI.md` (Chapter II)**

### The Three Layers

1. **Individual Leverage** — one person, dramatically better results. This is where
   Launch Point is now. The OS doing Todd's function work: daily brief, sales call
   brief, cadence management, message drafting.

2. **Organizational Coordination** — shared context, shared skills, consistent output
   across the team. When Tara uses the same DM framework the OS verifies against
   voice-principles.md. When Michelle follows the same onboarding skill. The team
   gets smarter together because the OS encodes the standards.

3. **Strategic Differentiation** — the OS becomes a competitive moat. Mission Control
   for clients. An AI coach trained on three years of Todd's coaching knowledge. A
   career assessment tool running autonomously. This is what makes Launch Point
   genuinely hard to replicate.

### The Guideline: Generally Don't Skip a Layer

The sequence exists because each layer's value depends on the one below it being
solid. Building Layer 3 tools on a shaky Layer 1 means the strategic tool is only
as good as the individual leverage underneath it — which isn't good enough yet.
Building Layer 2 team coordination before Layer 1 is working means encoding
inconsistent processes into shared standards, which compounds the inconsistency
rather than fixing it.

**As a general rule:** build Layer 1 before Layer 2, and Layer 2 before Layer 3.
This is not a bureaucratic gate — it's a quality-of-investment rule. The later
layers only pay off if the earlier ones are real.

**The exception is client-facing deliverables under Client Delivery.** Career
Compass and Launch Link are being built before all four workstations are Layer 1
proven — and that's legitimate, because they're not internal operational tooling
that depends on Layer 1 to work. They're the actual product clients pay for. A
coaching business that can't deliver its core product is worse off than one that
has an imperfect internal OS. Building them now is a business priority, not a
layer violation — as long as the internal OS underneath them gets solid in parallel.

### The Tier-Prerequisite Warning Rule

Regardless of layer, a more specific rule applies to the four-tier OS structure
itself: before building an Agent or Application for a function, that function
must have its Workstation built; before building an Application, the relevant
Agents should already exist or be explicitly planned. This isn't the same as
the layer sequence — it's about making sure the interactive, session-based
layer of a function exists before adding the autonomous layer on top of it.

The directive itself — Claude checking this and flagging what's missing — is a
behavioral rule, not reasoning, so it belongs in the root CLAUDE.md. **It is
now live in root CLAUDE.md** (merged as of this session).

**Why this check matters:** an Agent or Application built for a function with
no Workstation has nothing to inherit context from — no established voice, no
CRM patterns, no proven workflow to extend. Building the autonomous layer
first means building on nothing. The check exists to catch that before real
design time gets spent on a tier that isn't ready to support it yet.

**Where Launch Point is now:** Revenue Ops has a Workstation (live) and a growing
Agent layer (pre-call prep email, post-call routing — planned). Client Delivery has
a Workstation and an Application under active development (Launch Link), which
also includes a paired Agent (see Section 3's
Worked Example). Content & Marketing and Admin & Ops have Workstations only. No
function has reached Layer 2 or Layer 3.

---

## 14. The Knowledge Layer — How the OS Gets Smarter Over Time

**Source: `The_Field_Guide_to_Building_Products_with_AI.md` (Chapter VII), `How_Claude_Code_s_Creator_Starts_EVERY_Project_-_Austin_Marchese`**

### What the Knowledge Layer Is

The knowledge layer is everything the OS can read and reason over. It is what gives the
agents context that makes their output genuinely useful rather than generic.

Without a knowledge layer, every session starts from scratch and every agent output is
generic. With a strong knowledge layer, the OS knows Todd's voice, knows the pipeline,
knows what has been decided, and can give output that is specific, accurate, and
immediately usable.

### Three Principles for Building It

**Principle 1: Text-first, not tool-first.**
Markdown files in a folder structure. Plain text that any AI can read and any tool can
search. This is exactly what the Launch Point OS already does — CLAUDE.md, MEMORY.md,
voice-principles.md, and resource files are all markdown. This is correct. Maintain it.

**Principle 2: Capture is automated, not manual.**
If a human has to remember to capture something, it will not happen consistently. Build
capture into the workflow. Session audit skill, MEMORY.md update prompts, scheduled
tasks that log outputs to structured folders.

For any Application that runs repeatedly (once per client, once per unit of
work): every gate decision and every run output can be stored in a
structured `/runs/` folder in GitHub. The training corpus and the run log
become the same data store. This is the right pattern for that kind of
Application — no separate system needed.

**Principle 3: Retrieval is semantic.**
You should be able to ask "what did we decide about alumni outreach?" and get the answer
— not navigate a folder to find it. As the OS matures, the goal is for the knowledge
layer to be queryable by meaning, not just by file name.

### The Long-Term Knowledge Layer for Launch Point

| Source | Current Status | Future Use |
|---|---|---|
| voice-principles.md | Live — built from 65+ real conversations | Authoritative voice reference for all agents |
| launch-point-context.md | Live | Deep context for client journey and program |
| MEMORY.md files | Live — updated per session | Session-to-session continuity |
| notion-crm-schema.md | Live | Revenue Ops CRM queries |
| Installed skills (all tiers) | Live, currently all L1 | The compounding mechanism itself — see `skills-protocol.md`'s Three Levels of Skill Maturity |
| Application run outputs (e.g. Launch Link) | Planned — `/runs/` folder in GitHub, per Application | Training corpus for autonomous improvement |
| Circle community Q&A | Not yet captured | Future AI coach knowledge base |
| Call transcripts (Google Drive) | Not yet structured | Future AI coach knowledge base |

The Circle Q&A archive and call transcripts are the foundation for Mission Control's
AI coach. This is a Layer 3 build, but the knowledge should be captured now so it is
available when the build begins.

### The Three Stages, and Which One Launch Point Is In

The Field Guide names three stages a system moves through: **Static** (no
memory, every session starts from zero), **Configured** (persistent context —
CLAUDE.md, MEMORY.md, resource files — but the foundation only changes when
Todd changes it), and **Compound** (the system updates itself: corrections
get captured automatically, decisions get logged, patterns get noticed
without manual intervention).

**Launch Point is Configured, moving toward Compound.** The context files
exist and are strong. What's still manual is the *updating* — Boris
Churnney's practice of adding a rule to CLAUDE.md when the same mistake
happens twice is the mechanism that moves a Configured system toward
Compound, one correction at a time. This is not just maintenance — it is
compounding. Every correction baked into the file means the agent never
makes that mistake again, without Todd having to re-explain. The OS gets
smarter with every correction.

The corollary: never correct the same thing twice without adding a gotcha or a rule.
If you say it once, say it to the file.

---

## 14A. The Launch Point Brain — Deferred, See Separate Spec

**Status: designed at the concept level only. Not built. Not scheduled.**

A persistent ingestion layer covering ClickUp, Slack, and team call recordings
(architectural memory for this planning space) and sales call recordings
(Revenue Ops coaching and prospect enrichment). Full design — the two-branch
structure, the source comparison table, the file-location recommendation, and
every unresolved question — lives in `launch-point-brain-deferred-spec.md`.
Open that file only when Todd is ready to initiate this build; it adds no
value to any other session.

---

## 14B. The Idea Inbox (Deferred — Explicitly Independent of 14A)

**Status: designed at the concept level only. Not built. Not scheduled. Revisit
when Todd initiates. Explicitly does NOT wait on Section 14A — simpler, separate,
can be picked up first.**

### What This Is, and Why It's Not the Same as 14A

Section 14A (the Launch Point Brain) captures context from *ongoing systems* —
ClickUp, Slack, call recordings — to inform planning and coaching. This is a
different, more upstream problem: right now, there is no defined path for a
raw, half-formed idea to reach this planning space at all, unless Todd happens
to be in this specific thread and happens to raise it. An idea that occurs on a
walk, in a different Claude thread, or as a passing note has nowhere defined to
land — it depends entirely on Todd remembering to bring it here later.

The idea inbox would be a simple, low-tech answer to that: a running list —
not a design spec, not yet — where a stray idea gets captured the moment it
occurs, regardless of which environment Todd is in when it happens. Its only
job is to make sure an idea doesn't disappear. It is explicitly *before*
Bucket 4 (Deferred Design Spec) in maturity — an idea in the inbox has no
status field, no open-questions list, and no approval language yet, because it
hasn't been thought through enough to have any of those.

### The Graduation Path

An idea moves out of the inbox and into a proper Deferred Design Spec (Bucket 4,
Section 14A-style) once someone — Todd, or Claude prompting Todd — actually
sits down and works it through enough to generate real open questions. Until
that happens, it just sits in the list, unexamined, costing nothing.

### Why This Is Simpler Than 14A, and Can Be Built First

14A requires an ingestion pipeline — Google Drive, GitHub, Make.com, transcript
parsing, judgment calls about signal vs. noise. The idea inbox requires none of
that. At its simplest, it could be a single markdown file Claude appends to
whenever Todd says something that sounds like a stray idea rather than an
in-the-moment request — no external system integration required, no
Slack/ClickUp/call-recording pipeline needed. This is why it doesn't need to
wait on 14A's larger design: it's a much smaller, more self-contained build.

### Open Questions Before This Gets Built

- Where does the inbox file live? Candidates: a new file in `00_Resources/`
  (e.g., `idea-inbox.md`), or a section within `launch-point-os-context.md`
  since that file already tracks backlog and build status.
- How does Claude recognize a "stray idea" worth capturing, distinct from an
  in-the-moment request or a fully-formed plan? This likely needs its own
  trigger-recognition rule, parallel to the Skill-Recognition Rule (Section 6)
  but tuned for a different signal — something closer to "this sounds like a
  passing thought, not a task" than "this sounds like a repeatable process."
- Does capturing an idea require any confirmation from Todd ("want me to add
  this to the inbox?"), or should it happen silently in the background the way
  MEMORY.md updates do?
- Should ideas in the inbox ever expire or get pruned, the same way CLAUDE.md
  gets trimmed (Section 16) — or does the inbox just grow indefinitely until
  something is manually promoted out of it?
- This could plausibly extend to non-Claude-Chat sources later (a note app, a
  voice memo) — but per the decision above, the inbox should be scoped to
  something Claude can capture directly at first, not expanded to match
  14A's multi-source ambition before it's even built once.

### Related Behavioral Rule, Once This Is Built

Once an idea inbox exists, this planning space should recognize the same kind
of moment the Skill-Recognition Rule (Section 6) watches for, but applied to
ideas instead of processes: if Todd raises something that sounds like a
not-fully-formed idea rather than a request to act on now, Claude should ask
whether to capture it in the inbox — the same way it asks whether to capture a
skill. This rule cannot go live until the inbox itself exists; it is deferred
alongside the rest of this section.

---

## 15. Function vs. Judgment Work — What AI Should Own

**Source: `The_Field_Guide_to_Building_Products_with_AI.md` (Chapter IV)**

### The Distinction

**Function work** is repeatable. The same input reliably produces the same output.
Writing a first draft, pulling a CRM record, running a daily brief, formatting a report,
scoring an ICP, calculating a follow-up date. AI handles this.

**Judgment work** is contextual. It requires taste, relationship knowledge, strategic
thinking, or a decision that could go many ways depending on factors no system can fully
encode. Deciding whether to push a skeptical lead or let them come around. Reading a
prospect's energy on a call. Deciding what content to create this week. Todd handles this.

Judgment work itself splits into two moments: judgment at the *beginning*
(what's worth Todd's or the OS's attention — what to build, which lead to
prioritize) and judgment at the *end* (is this output actually good enough to
ship). AI increasingly owns everything in between. Both bookends stay with
Todd; the middle is where the OS's function work lives.

### The Audit Question

For every task in Todd's workflow: *Is this function work or judgment work?*

- If function work → build it into the OS with an approval gate
- If judgment work → Todd owns it; the OS may support it with context and drafting, but
  the decision stays with Todd

### What This Means in Practice

The goal is not to have AI make more decisions. It is to have AI handle everything that
is not genuinely a decision, so Todd's time and attention are available only for what
requires judgment.

**Function work that is currently manual in Todd's workflow (and should not be):**
- Drafting DM responses based on cadence stage
- Pulling prospect context before a call
- Flagging who is due for a follow-up
- Generating a weekly pipeline summary
- Routing post-call objection buckets to the right Kit.com sequence
- Formatting and filing client research and reporting outputs

**Judgment work that will always stay with Todd:**
- Deciding to push or hold a lead
- What the message actually says when stakes are high
- Sales call dynamics and close decisions
- Relationship strategy with ministry network contacts
- What to build next and in what order

---

## 16. OS Maintenance — Keeping It Clean Over Time

**Source: `How_Claude_Code_s_Creator_Starts_EVERY_Project_-_Austin_Marchese`, `Building_AI_Agents_that_actually_work_-_Greg_Isenberg`, `Become_AI_Native_in_less_than_60_mins_-_Greg_Isenberg`**

### The Core Loop

The OS improves through use. The feedback loop is:

1. The agent runs a task
2. Todd notices something off or approves a correction
3. The relevant file gets updated (CLAUDE.md, MEMORY.md, or a skill gotcha)
4. The next run is better
5. Over time, the corrections compound and the agent rarely makes the same mistake twice

This loop only works if corrections land in the file immediately. An unwritten correction
is a lesson that disappears when the session ends.

### How New Skills Enter the System

New skills enter the maintenance pipeline through two doors — both described in Section
5 and governed by the root CLAUDE.md's Skill-Recognition Rule (Section 6):

1. **Todd initiates:** at any point during a session, Todd says "create a skill for
   what we just did" or "build a skill for [task]"
2. **Claude recognizes and prompts:** Claude notices a task being re-explained or a
   result that will clearly recur, and asks whether to capture it as a skill

Either way, the session produces a first-draft skill file. That file enters the
install protocol in `skills-protocol.md` — it moves to Claude Code for testing
and refinement, gets installed in the right folder, gets referenced in the
relevant CLAUDE.md, and only then is considered live. A first-draft skill from
a session is not live until it has completed that protocol. Flag it explicitly
when handing it off.

### Session Audit Protocol

The checklist itself is a behavioral rule, not reasoning — it belongs in the
root CLAUDE.md. **It lives in `proposed-claude-md-additions.md`, ready to merge.**

**Why this runs at the end of every session, not just when something
noticeably breaks:** most improvement opportunities are quiet — a fact that
should've gone in MEMORY.md but didn't, a skill that would've helped but
nobody thought to build it in the moment. Section 16's core loop only works if
these get caught systematically, not just when a mistake is big enough to be
obvious.

In Cowork, this is handled by the session audit skill. In Claude Code sessions, end with
the prompt: *"Based on this session, update CLAUDE.md so this doesn't happen again."*

### Periodic Skills Audit

Beyond individual session maintenance, run a full skills audit periodically — any time
the skill library feels out of sync with actual usage, or at least every few months.
The source material (`How_Anthropic_Employees_ACTUALLY_Use_Claude_Skills_-_Austin_Marchese`)
provides a specific prompt for this: ask Claude to review all installed skills alongside
recent chat history and identify: (1) which skills should be adjusted to fit more cleanly
into one type, (2) which skills have gotchas that should be added based on observed
failures, and (3) which skills have usability components missing (config.json,
arguments field, ask user question tool — see `skills-protocol.md`'s Setup Prompts).

This audit is a Claude Code session, not a Cowork session — it involves reading and
potentially editing multiple skill files, which is build-environment work.

### When to Trim vs. When to Add

**Add to CLAUDE.md** when the same mistake happens twice.
**Add a gotcha to a skill** when Claude fails at a step in that specific skill.
**Add to MEMORY.md** when a fact is stated that the agent needs to know going forward.

**Trim from CLAUDE.md** when a rule is no longer needed because:
- The model now handles it without instruction
- The behavior it was correcting no longer applies
- It duplicates another rule
Periodically run: *"Remove anything from CLAUDE.md that is no longer needed, contradictory,
duplicate, or unnecessary."*

**After any merge into root CLAUDE.md** (such as merging
`proposed-claude-md-additions.md`), count the resulting file's total lines.
If it exceeds 200, this is the trigger to run the prune prompt above or
consider Boris's more aggressive "delete and start fresh" approach — don't
wait for a future bloat symptom to notice.

**Note: the source material treats CLAUDE.md and MEMORY.md differently on
bloat.** The 200-line rule and the "delete and start fresh" guidance are
specifically about CLAUDE.md. On MEMORY.md, the source material's own stance
is more relaxed — "I personally haven't hit that threshold yet... I wouldn't
worry too much" — with a lighter suggestion (tell it to only save substantial
corrections) rather than a hard limit. Don't apply CLAUDE.md's strict length
discipline to MEMORY.md by default; they're governed differently in the
source material itself.

**Never add gotchas pre-emptively.** Only add what you have actually seen go wrong.
Pre-emptive gotchas add noise without signal.

### Simplicity as a Design Value

The Launch Point OS actively avoids over-engineering. Preferences stated in sessions:
- Consolidate context into existing files rather than creating new redundant documents
- If a rule can go in an existing file, it does not get a new file
- Documentation should be the minimum needed to be clear, not comprehensive for its own sake
- When in doubt between two approaches, choose the simpler one

The same principle applies to file proliferation: more files is not more organized. More
files means more for the agent to load, more for Todd to maintain, and more surface for
context to get stale. One well-maintained file beats three half-maintained ones.

---

## 17. Handoff Protocol — The File Organizer Skill

**Status: designed, not yet built.**

A skill for moving files finalized in Claude Chat into the correct location
in the local OS folder structure and committing them to GitHub — matches
against a file's stated Identity (Section 6), stops and asks when ambiguous,
stages the git change, and always waits for Todd's explicit approval before
pushing. Full specification — the ten-step process, the routing table, and
multi-repo handling — lives in `file-organizer-skill-spec.md`. Consult it
when actually building this skill; the short version here is enough for
everything else.

---

## Quick Reference: Decision Table

| Question | Answer |
|---|---|
| Should this go in CLAUDE.md or MEMORY.md? | Does it prescribe behavior? CLAUDE.md. Does it describe a fact that could change? MEMORY.md. |
| Should this be a skill or a prompt? | Will it be used more than once? Make it a skill. |
| Should this run in Cowork or Claude Code? | Is it thinking and planning work? Claude Chat. Does it run on a schedule or start with Todd? Cowork. Does it run when something external happens, or is it a build? Claude Code. |
| What type of skill is this? | One purpose only → Utility. Checks output → Verification. Pulls data → Enrichment. Chains skills → Orchestration. |
| Should this be in Make.com or Claude Code? | Condition-based logic with deterministic output → Make.com. Judgment, language, or context-reading → Claude Code. |
| Should I build this now? | Does it move Todd closer to 5 calls/week? If not, defer. |
| Who approves before anything sends? | Todd. Always. No exceptions. |
| When is a task ready to schedule? | After it has been manually tested and runs correctly. Never before. |
| When do I add a gotcha to a skill? | After I have actually observed Claude fail at that step. Not before. |
| When do I trim CLAUDE.md? | When a rule is duplicated, outdated, or no longer needed. Periodically. |
| Is this function work or judgment work? | Can the same input reliably produce the same output? Function work → AI. Requires contextual decision? Judgment work → Todd. |
| Should a finalized document auto-commit to GitHub? | Filing/finding the path → automate it. Committing/pushing → Todd approves every time. |
| Is a new piece of work part of an existing workstation, or a separate agent/application? | Does it run on a schedule or start with Todd? It's workstation work. Does it run because something external happened? It's an agent. Is it too large/self-contained to be one agent (own skills, own pipeline)? It's an application. All three serve the function but are siblings, not nested inside each other. |
| Should this be called a "workstation," "agent," or "application"? | Cowork-resident, schedule- or session-triggered → workstation. Claude Code-resident, event-triggered, single-purpose → agent. Claude Code-resident, multi-skill, deliverable-grade → application. Never blend two of these labels for the same thing. |
| Does a new application need its own GitHub repo? | No — every Launch Point Application lives in `launch-point-os`, no exceptions, regardless of deployment needs. |
| Does a new project belong inside Launch Point OS at all? | Hard rule: anything to do with the career coaching business → `launch-point-os` repo. Everything else → its own separate repo, no exceptions, even in the same GitHub account. |

---

*Last updated: July 2026 — established the four-tier model (Function, Workstation,
Agent, Application) and reserved "agent" exclusively for Claude Code's autonomous
layer; restructured the OS folder model to function-first (each function owns its
Workstation, Agents, and Applications as siblings); added the Launch Link worked
example (Application + Agent pairing) under Client Delivery; added "Creating a new
Agent" and "Creating a new Application" processes parallel to the existing
workstation process; clarified the hard rule that anything related to the career
coaching business lives in the Launch Point OS repo, with all else in separate
repos; added the Source File Reference Table and converted every section's Source
line to exact filenames; added Section 17 (Handoff Protocol); updated status notes
for Skill-Recognition Rule and Tier-Prerequisite Rule (both now live in root CLAUDE.md);
extracted Sections 8, 14A, and 17 into companion files.*
*Replaces: agent-and-os-best-practices.md (previous version)*
*Full source files (see the Source File Reference Table above): `Become_AI_Native_in_less_than_60_mins_-_Greg_Isenberg`,
`Building_AI_Agents_that_actually_work_-_Greg_Isenberg`,
`How_Claude_Code_s_Creator_Starts_EVERY_Project_-_Austin_Marchese`,
`How_Anthropic_Employees_ACTUALLY_Use_Claude_Skills_-_Austin_Marchese`,
`I_got_a_private_lesson_on_Claude_Cowork___Claude_Code_-_Greg_Isenberg`,
`The_Field_Guide_to_Building_Products_with_AI.md`, and the Launch Point OS files
(`CLAUDE.md`, `MEMORY.md`, `launch-point-os-context.md`, `voice-principles.md`)*
