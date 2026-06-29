# Launch Point — Agent & OS Best Practices

*Distilled from six expert sources. Consult this file when designing
any agent, skill, workflow, or OS architecture decision. For deeper
detail, read the full source referenced in each section.*

*Sources attached to this project:*
- *Become_AI_Native_in_less_than_60_mins_-_Greg_Isenberg (Theo Taba)*
- *Building_AI_Agents_that_actually_work_-_Greg_Isenberg (Remy Gasill)*
- *How_Claude_Code_s_Creator_Starts_EVERY_Project_-_Austin_Marchese (Boris Churnney)*
- *How_Anthropic_Employees_ACTUALLY_Use_Claude_Skills_-_Austin_Marchese*
- *I_got_a_private_lesson_on_Claude_Cowork___Claude_Code_-_Greg_Isenberg (Boris Churnney)*
- *The_Field_Guide_to_Building_Products_with_AI.md (Ed Landon & Theo Taba)*

---

## 1. How to Start Any Build

**Source: Boris Churnney (Claude Code creator)**

- Start 80% of sessions in **plan mode** before writing a single line of
  code or instruction. Ask the agent to interview you about the problem
  first. Slow down to speed up.
- The prompt that works: *"Before building anything, ask me what I'm
  trying to solve, who it's for, what success looks like, and what it
  should NOT do. Summarize it back before proceeding."*
- Once the plan is solid, execution is almost automatic. Bad plans
  produce rework. Good plans produce clean first drafts.
- **Babysit early runs.** Watch the first few executions of any new
  build closely. Correct fast, then let it run.
- Use **subagents** for complex tasks — break large jobs into smaller
  parallel agents rather than one agent trying to do everything.

---

## 2. What Makes an Agent Work

**Source: Remy Gasill**

An agent has four components. All four must be present for it to work:

1. **Identity** — who the agent is, what it owns, what it doesn't own.
   Written in CLAUDE.md. Keep it focused. One agent, one lane.
2. **Memory** — what the agent knows and remembers across sessions.
   Written in MEMORY.md. Facts that could change go here, not in
   CLAUDE.md.
3. **Skills** — step-by-step playbooks for repeatable tasks. Once
   written, the agent can run any skill without re-explanation.
4. **Tools (MCPs)** — the external systems the agent can read from and
   write to. Only connect what the agent actually needs.

**The distinction between CLAUDE.md and MEMORY.md:**
- CLAUDE.md = behavioral rules. "Always do X before Y." "Never post
  publicly." Prescriptive, stable, doesn't change often.
- MEMORY.md = facts and context. Contacts, pipeline stage, decisions
  made, preferences. Descriptive, can change, updated frequently.

**Agents work in departments, not as generalists.** One agent per
function. Revenue Ops agent owns pipeline. Client Delivery agent owns
client experience. They don't cross lanes.

---

## 3. The Four Skill Types

**Source: Anthropic internal team (via Austin Marchese) + Field Guide**

Every skill falls into one of four types. The best skills fit cleanly
into exactly one. Skills that straddle multiple types confuse the agent.

1. **Utility skills** — small, reusable, single-purpose. "Draft a
   response in Todd's voice." "Summarize this transcript." Used as
   building blocks inside larger skills.

2. **Verification skills** — check the final output before it goes
   anywhere. Does this match the voice? Is anything hallucinated? Does
   it follow the right format? *Anthropic's own team found verification
   skills have the most measurable impact on output quality of any skill
   type.* Every output-producing workflow should have one.

3. **Data enrichment skills** — pull external data into the system.
   "Pull this prospect's CRM record, Kit history, and Kondo thread."
   Feeds richer context into other skills.

4. **Orchestration skills** — chain other skills together into a
   complete workflow. "Run data enrichment, then draft, then verify,
   then deliver to Slack." These are what make agents truly autonomous.

**The three levels of skill maturity (Field Guide):**
- L1: Personal shortcut — lives in your CLAUDE.md, helps you only
- L2: Team standard — shared, consistent, everyone uses the same skill
- L3: Organizational asset — maintained, versioned, improved over time
  with a feedback loop. Almost nobody reaches L3. The ones who do
  compound indefinitely.

**When to build a skill:** Any time you find yourself explaining the
same process twice. If you've done it once and will do it again, it's
a skill.

---

## 4. Compound Systems — How the OS Gets Smarter

**Source: Field Guide (Chapter VI) + Remy Gasill**

There are three levels of AI systems:

- **Static** — you ask, it answers. Every session starts from zero.
  Useful but a treadmill — you run every day but never move forward.
- **Configured** — persistent context via CLAUDE.md, MEMORY.md, and
  resource files. Each session starts from a foundation. Dramatically
  better, but the foundation only changes when you change it.
- **Compound** — the system updates itself. Corrections get captured.
  Decisions get logged. Patterns get noticed and encoded. You don't
  maintain it. It maintains itself. Every interaction makes the next
  one better.

**The three building blocks of compound systems:**
1. **Persistent memory** — MEMORY.md updated after every correction
2. **Feedback capture** — every edit to an agent's output gets saved
   as a lesson. The session audit skill handles this in Cowork.
3. **Automated pipelines** — work that runs while Todd isn't watching.
   The daily brief, the CRM snapshot — these run overnight and surface
   output for review. The system does work while Todd sleeps.

**The compounding math:** A compound system is 20% better than static
in month one. By month six it's 200% better. The gap is merciless.
Start one loop. Let it compound. Add another when it's stable.

---

## 5. Function Work vs. Judgment Work

**Source: Field Guide (Chapters II and IV)**

Every role is a mix of function work and judgment work.

**Function work:** follows a pattern. Could be written down. Someone
with the right instructions could reproduce the output. Complex,
skilled — but repeatable. This is what AI handles.

**Judgment work:** context-dependent. Requires taste, experience, and
reading of a situation. Can't be reduced to steps. This is what Todd
does. Calling which leads are worth a personal message. Knowing when
a client is disengaging before they say so. Deciding what content will
resonate. Leading the call.

**The design principle:** AI handles the function work between the
judgment bookends. Todd makes the call at the beginning (what to
prioritize) and at the end (is this output good enough to send).
Everything in the middle — drafting, researching, logging, formatting,
scheduling — is function work that the OS should own.

**For Todd specifically:** The goal is to reduce his morning reactive
block from 2.5 hours to 1 hour by having the OS handle all the
function work in that block. Todd's time is for judgment only.

---

## 6. The Knowledge Layer — Context Is the Moat

**Source: Field Guide (Chapter VII)**

The AI model is a commodity. What makes it useful is the context it
has access to. Two businesses using the same AI model with different
context get wildly different results.

**Three principles for a strong knowledge layer:**
1. **Text-first, not tool-first.** Markdown files in a folder
   structure. Plain text that any AI can read and any tool can search.
   This is exactly what Todd's OS already does — CLAUDE.md, MEMORY.md,
   and resource files are all markdown. This is correct.
2. **Capture is automated, not manual.** If a human has to remember to
   capture something, it won't happen consistently. Build capture into
   the workflow — session audit skill, MEMORY.md update prompts,
   scheduled tasks that log outputs.
3. **Retrieval is semantic.** You should be able to ask "what did we
   decide about alumni outreach?" and get the answer — not navigate
   a folder structure to find it.

**The practical implication for Launch Point:** The career transcript
archive in Google Drive, the three years of community Q&A in Circle,
and Todd's call notes are all context that should eventually feed the
OS brain. This is the foundation for Mission Control's AI coach and
for the OS agents getting dramatically better over time.

---

## 7. The Three-Layer Sequence — Order Matters

**Source: Field Guide (Chapter II)**

Most organizations get this wrong. The sequence is:

1. **Individual leverage** — one person, one setup, dramatically better
   results. This is where Todd is now. Revenue Ops daily brief,
   pre-call prep, cadence management. The OS doing Todd's function work.
2. **Organizational coordination** — shared context, shared skills,
   consistent output across the team. When Tara uses the same DM
   framework that the OS verifies against voice-principles.md. When
   Michelle follows the same onboarding skill. The team gets smarter
   together.
3. **Strategic differentiation** — the OS becomes a competitive moat.
   Mission Control for clients. The AI coach trained on three years of
   Todd's knowledge. The career assessment tool running autonomously.
   This is what makes Launch Point genuinely hard to replicate.

**You cannot skip a layer.** Building Mission Control before the
individual OS is solid wastes the investment. Getting organizational
coordination before individual leverage is solid creates expensive
tools nobody uses correctly.

---

## 8. How to Become AI Native

**Source: Theo Taba (via Greg Isenberg)**

AI native is not about using AI tools. It's about restructuring how
work gets done so AI is the default operator and humans are the
managers and decision-makers.

**The operating principle:** Identify every repeatable task in the
business. Ask: can AI do this with a human approval gate? If yes,
build it that way. Only keep humans in the loop at genuine decision
points.

**Context is everything.** The more context an agent has — about the
business, the person, the history, the goal — the better its output.
Investing in context files (CLAUDE.md, MEMORY.md, resource files) is
the highest-leverage activity in building an AI-native operation.

**Skill chains are the product.** Individual skills are useful. Chains
of skills that run automatically, produce output, and surface it for
approval are transformative. The goal is always to build toward longer,
more autonomous chains with fewer interruptions.

---

## 9. How Cowork and Claude Code Work Together

**Source: Boris Churnney (via Greg Isenberg)**

- **Cowork** is the right home for scheduled, session-based work —
  daily briefs, weekly reviews, anything that runs on a timer and
  works through a structured sequence.
- **Claude Code** is the right home for triggered, event-driven
  intelligent work — when something happens and the agent needs to
  read context, make a judgment, and produce output autonomously.
- They are not competing. Use both. Cowork for sessions, Claude Code
  for triggers.
- **The mature pattern:** External event (Make.com detects Calendly
  booking) → triggers Claude Code → agent pulls context, drafts
  output → delivers to Slack for approval → human approves → action
  taken.
- **Less is more in instruction files.** CLAUDE.md files should be
  short and behavioral. Concise rules outperform long rules.
- **Memory compounds.** Every correction, preference, and decision
  that gets saved to MEMORY.md makes future sessions better.

---

## 10. Approval Gates — Non-Negotiable Design Pattern

**Consistent across all sources**

Every workflow that produces output for a lead, client, or the public
must have an approval gate before anything sends or publishes.

**The pattern:**
```
TRIGGER → AGENT EXECUTES → VERIFY (skill) → DRAFT TO SLACK → TODD APPROVES → SEND
```

Note the verification step before Slack delivery. The agent checks
its own output against the voice principles and output standard before
surfacing it for Todd's review. This is what separates a configured
system from a compound one.

Approval gates are not bureaucracy — they are the training mechanism.
Every time Todd edits a draft before approving, that edit is a lesson.
Capturing that lesson in MEMORY.md or the relevant skill is how the
system gets smarter over time.

**When to remove an approval gate:** Only when Todd has approved the
same type of output 20+ times without editing it. Not before.

---

## 11. Common Mistakes to Avoid

**Compiled across all sources**

- **Building before planning.** The most expensive mistake. Always
  plan first.
- **One giant agent.** Agents that try to own everything produce
  mediocre output across the board. One agent, one lane.
- **No verification skill.** Sending output directly to the approval
  gate without a quality check first. The agent should verify its
  own output before Todd ever sees it.
- **Skipping memory capture.** If a correction doesn't get saved,
  the agent makes the same mistake again. Always capture corrections.
- **Automating before testing manually.** Never schedule a task that
  hasn't been run manually first. Automate proven processes, not
  untested ones.
- **Building in the wrong tool.** Make.com for event triggers.
  Claude Code for judgment. Cowork for sessions.
- **Staying at individual leverage forever.** Fast individuals rowing
  in different directions. A lot of motion, no organizational
  compounding. Build toward shared skills and shared context.
- **Context leaking out of the system.** Every decision made in a
  meeting, every client insight, every correction — if it doesn't
  get captured in a markdown file, it's gone. The knowledge layer
  is the moat. Protect it.