# The File Organizer Skill — Full Specification

*Extracted from `agent-and-os-best-practices.md`, Section 17. This file is a
companion to the main best practices document — cross-references to numbered
sections (Section 2, Section 5, Section 6, Section 8, Section 9, Section 11,
Section 14, Section 15) refer back to that document. Open this file only when
actually building this skill.*

---

**Source: Session decision — no external transcript covers this. Applies the model from
Section 2 (`I_got_a_private_lesson_on_Claude_Cowork___Claude_Code_-_Greg_Isenberg`) and
the skill protocol from Section 8 (`How_Anthropic_Employees_ACTUALLY_Use_Claude_Skills_-_Austin_Marchese`).**

## The Problem This Solves

Documents get finalized in Claude Chat (this Planning Space) but the files need to
end up in the right place in the local OS folder structure and committed to GitHub.
Today that handoff is manual: download the file, find the right folder, move it,
commit it. This skill removes the "find the right folder" and "commit it" steps
without removing Todd from the loop on anything that touches the repo.

This skill lives in Cowork, not Claude Code, because it is triggered by Todd opening
a session and asking for it (Section 2, Mode 2 — On-demand session). It is built and
tested in Claude Code first, per the standard skill protocol (Section 8), then
installed as a global skill in `00_Resources/Skills/`.

Unlike other global skills (voice check, session audit), this one isn't a tool any
workstation calls on as part of its function — no workstation's job is to file
documents into the repo. It's OS-level infrastructure that happens to live in the
same global skills folder. That distinction doesn't change where it's installed,
just what it's for.

## What the Skill Does

1. Todd downloads a file produced in Claude Chat to the local Downloads folder
2. Todd opens Cowork and triggers the skill (e.g., "organize my downloads")
3. The skill reads the file's **Identity section** — every CLAUDE.md in this OS
   states its identity in the first few lines per Section 6's required structure
   ("who this agent is and what it owns"). That identity statement, not a guessed
   header pattern, is what the skill matches against the routing table — this
   mirrors exactly how Claude itself decides which skill or workstation to use:
   by reading a stated description of what something is and when it applies, not
   by pattern-matching file structure (see Section 8, Rule 3)
4. It checks the file's stated identity against the routing table below
5. **Match found** → moves the file to the correct path on the local machine
6. **No match, or the identity statement doesn't clearly name a function** → stops
   and asks Todd where the file belongs. Does not guess. This becomes a new row in
   the routing table once Todd answers, so the same file type is recognized
   automatically next time.
7. Stages the change in git (`git add`)
8. Presents the diff to Todd and asks for explicit approval before committing
9. On approval: commits with a clear message (e.g., `Update agent-and-os-best-practices.md`)
   and pushes
10. Does not push without that explicit approval — staging is not the same as approval

## The Routing Table

This is the seed table for the skill. Add a row every time a new file type is
introduced or an ambiguous file gets manually resolved — this is how the skill
gets smarter over time, the same compounding pattern described in Section 14.

| File pattern (stated Identity, per Section 6's required CLAUDE.md structure) | Destination Path |
|---|---|
| `agent-and-os-best-practices.md` | `00_Resources/` |
| `skills-protocol.md` | `00_Resources/` |
| `launch-point-brain-deferred-spec.md` | `00_Resources/` |
| `file-organizer-skill-spec.md` | `00_Resources/` |
| `CLAUDE.md`, Identity states root-level OS rules (e.g. "Launch Point Cowork OS") | `/CLAUDE.md` (root) |
| `CLAUDE.md`, Identity names a function and describes session/schedule-triggered work | `[Function Name]/Workstation/CLAUDE.md` |
| `CLAUDE.md`, Identity names a function and describes autonomous/event-triggered work | `[Function Name]/Agents/[Agent Name]/CLAUDE.md` |
| `CLAUDE.md`, Identity names a function and describes a larger multi-skill, deliverable-grade build | `[Function Name]/Applications/[Application Name]/CLAUDE.md` |
| `MEMORY.md` (root-level) | `/MEMORY.md` (root) |
| `MEMORY.md` (workstation-specific) | `[Function Name]/Workstation/MEMORY.md` |
| `MEMORY.md` (agent-specific) | `[Function Name]/Agents/[Agent Name]/MEMORY.md` |
| `MEMORY.md` (application-specific) | `[Function Name]/Applications/[Application Name]/MEMORY.md` |
| `voice-principles.md` | `00_Resources/` |
| `launch-point-context.md` | `00_Resources/` |
| New skill file, global (description states it applies across functions) | `00_Resources/Skills/` |
| New skill file, workstation-specific | `[Function Name]/Workstation/Resources/` |
| New skill file, agent-specific | `[Function Name]/Agents/[Agent Name]/Resources/` |
| New skill file, application-specific | `[Function Name]/Applications/[Application Name]/Skills/` |
| Any SOP, gate file, or documentation for an Application | `[Function Name]/Applications/[Application Name]/` (all Application content, including docs, lives in `launch-point-os` — no separate repo, see Repo Note below) |

*This table works the same way Claude already decides which skill or workstation
applies in every other part of this OS (see Section 8, Rule 3 on trigger
descriptions, and CLAUDE.md's existing "Routing by task" table) — match against
what the file's Identity section actually says it is and does, not a guessed
structural pattern. When a file's Identity doesn't clearly name a function or
tier, that's the ambiguous case the skill stops and asks about — see step 6
above.*

## Why Manual Approval Stays on the Commit Step

This skill automates *finding the right folder* — pure function work, per Section 15's
distinction. It does not automate *deciding the change is correct and ready to be
permanent in the repo* — that is a judgment call, however small, and per Section 11's
approval gate rule, nothing changes in a way that's hard to undo without Todd's
explicit sign-off. A bad commit message or an accidental overwrite is exactly the
kind of mistake this gate exists to prevent.

## Repo Note

Launch Point has exactly one repo for anything related to the career
coaching business: `launch-point-os`. Every Workstation, Agent, and
Application lives there — no exceptions, regardless of deployment needs or
complexity (see Section 2, "One Repo for Everything Launch Point").

**If the skill ever encounters a file that seems to belong to a different
repo, that's a strong signal the file isn't a Launch Point file at all** —
it may belong to a separate, non-Launch-Point project (a side project, a
different revenue stream — see Section 2's repo rule for the full
distinction). In that case, the skill should stop and ask rather than
guess, the same as any other ambiguous case (see step 6 above). Do not
create or assume a new repo path for anything that reads as Launch Point
work — if it's Launch Point, it goes in `launch-point-os`, full stop.

## Build Status

This skill is **designed, not yet built.** Per Section 5's build protocol, the next
step is to take this specification into Claude Code, enter plan mode, and have Claude
Code interview back the exact mechanics (file read method, git commands, how the
routing table is stored and updated) before writing the skill file. Per Section 9,
it must be manually tested on a handful of real files before Todd relies on it.
