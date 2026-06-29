# MEMORY.md

*Last updated: June 2026*

## Active Projects

- **Cowork OS Build** — Foundation complete and live. Root CLAUDE.md,
  MEMORY.md, voice-principles.md, and launch-point-context.md all live.
  Four workstations defined: Revenue Ops, Client Delivery,
  Content & Marketing, Admin & Ops. Revenue Ops is fully built and live.
  Remaining workstation CLAUDE.md files not yet built.

- **Revenue Ops Workstation** — Fully live. Trigger: `revops`. Owns
  prospect-to-alumni pipeline: lead nurture, pipeline management,
  daily/weekly briefings, ICP scoring, client wins, testimonials,
  alumni cadence, referral asks. All resource files confirmed built:
  CLAUDE.md, MEMORY.md, notion-crm-schema.md, icp-rules.md,
  cadence-table.md, tool-configs.md, sales-ops-daily-brief.md,
  crm-snapshot-task.md, sales-crm-cache.xlsx.

- **Pre-Call Prep System** — Active build priority. Four steps in order:
  1. ScoreApp routing form rebuild — two results pages (qualified →
     Calendly booking, not qualified → free resources). Todd configures.
  2. Calendly intake questions — move sales call questions into booking
     form. Todd configures. Data accessible via Calendly API.
  3. Make.com: booking confirmed → homework email (immediate) + resume
     request (day before). Standard emails for now.
  4. Cowork scheduled task: morning call prep brief on days with sales
     calls. Design session needed in planning space before building.

- **Launch Link** — Standalone app built in Lovable. Links client target
  organizations with Todd's LinkedIn connections. MCP connector in
  development. When MCP is live, build dedicated workstation and skills
  around outreach workflow.

---

## Business Context

- **Company:** Launch Point
- **Owners:** Todd Linder and Taylor Linder
- **Email:** todd@launchpoint.co
- **Mission:** Help ministry leaders transition into non-vocational careers
- **Core program:** Interview Accelerator — 90 days, $1,500 (Pro),
  optional $300 resume writer add-on
- **North star metric:** 5 sales calls booked per week
- **Conversion rate on calls:** 60–70% — bottleneck is getting people
  onto calls, not closing them
- **Current lead sources:** LinkedIn (100%), YouTube (planned)
- **Lead breakdown:** ~70% referrals (LinkedIn ministry network contacts,
  not clients). ~30% email list, quiz funnel, LinkedIn organic.
- **Sales process:** LinkedIn → DM nurture → clarity call → close.
  Most clients select Interview Accelerator Pro.
- **Community platform:** Circle.so (Ministry to Marketplace community)
- **Email platform:** Kit.com (~1,500 subscribers)
- **CRM:** Notion — three separate databases (Sales, Client, Alumni)
- **SMS platform:** Quo
- **LinkedIn management:** Kondo (MCP connected — post-connection only,
  not top-of-funnel)
- **Content management:** ClickUp (EA handles handoffs)
- **Automation:** Make.com (event-driven scenarios)
- **Routing form:** Moving from Calendly to ScoreApp — Calendly routing
  form data is not accessible via API

---

## Weekly Rhythm

- **Monday:** Morning block 8–10:30 AM. EA/team meeting 1:15–2:30 PM.
  Client group call 3–4:30 PM.
- **Tuesday:** Morning block 8–10:30 AM. Sales calls at 10:30 AM, 2 PM,
  3 PM. New client onboarding at 1 PM.
- **Wednesday:** Morning block 8–10:30 AM. Sales calls at 10:30 AM,
  12:45 PM, 1:45 PM, 3 PM. Every other week 1:45 PM slot replaced by
  Public Workshop.
- **Thursday:** Morning block 8–10:30 AM. EA check-in 10:30 AM. Client
  group call 12:45–2:15 PM. Sales follow-up block 2:15–4:15 PM. Open
  relationship calls 4 PM and 4:30 PM.
- **Friday:** No calls. Admin, content, async work. Possible morning
  coffee meetups.
- **Daily:** Lunch 11:30 AM–12:45 PM. Circle community check-ins morning
  and afternoon (15–30 min each). LinkedIn review and writing end of day.

---

## Key Decisions Made

- Todd writes and posts all LinkedIn content himself — Claude may draft
  but never publish
- Todd owns: LinkedIn post writing, LinkedIn commenting
- Claude handles with Todd's voice: DM follow-ups, SMS, email, Calendly
  follow-ups, sales call prep
- Before sending any message of any kind, always present the draft with
  the last 2-3 messages of conversation context. Wait for explicit
  approval before sending. No exceptions.
- Circle MCP connection confirmed live
- Launch Link MCP connector not yet live — no action until connector is
  built and workstation is created
- Workstation folders will be created by Cowork inside the Projects
  folder, not manually
- Files stay local at /Users/toddlinder/Documents/Claude/ for now.
  Google Drive sync and team collaboration across Cowork revisit in
  ~December 2026 when Cowork matures.
- Sales Ops Agent absorbed into Revenue Ops workstation — no longer a
  separate project
- Final workstation structure: Revenue Ops, Client Delivery,
  Content & Marketing, Admin & Ops, Launch Link (pending),
  Strategic Relationships (pending — builds after Client Delivery
  and Launch Link are complete)
- Evaluated custom MCP server architecture (TypeScript + Supabase +
  vector memory). Decision: stay with Cowork OS. Revisit vector-based
  persistent memory in ~December 2026 alongside Google Drive sync.
- Local Excel caching is the preferred architecture for Sales CRM in
  daily briefs — 958 records exceed reliable live Notion API retrieval
- Slash commands avoided — conflict with Cowork skill system; using
  trigger keywords instead (e.g., `revops`)
- pipeline-stages.md not built — pipeline stage definitions folded into
  notion-crm-schema.md
- Lead ICP cadence intentionally excluded from daily brief for now —
  spec documented in Revenue Ops MEMORY.md under "Deferred"
- Make.com and Claude Code are complementary, not competing. Make.com
  handles event-driven triggers and routing. Claude Code handles
  judgment, context, and drafting. The pattern: Make.com detects event
  → triggers Claude Code → Claude drafts output → delivers to Slack
  for Todd's approval.
- Cowork stays as the home for the Revenue Ops daily brief. Claude Code
  handles autonomous triggered drafting as the OS matures.
- Routing form moving from Calendly to ScoreApp — Calendly routing form
  data is not accessible via API
- Notability eliminated from call prep workflow — replaced by a Cowork
  morning scheduled task that fires on days with sales calls
- EA DM framework is working as-is. Kondo is post-connection only and
  cannot support top-of-funnel outreach. Human scrape of followers,
  profile views, and engagements stays as-is per LinkedIn TOS.
- No-show reschedule trigger deprioritized — not a real problem in
  practice
- Kit.com post-call sequences (Not Yet / Not Enough Money / Spouse Not
  On Board) — hard no on building until copy is written. Copy being
  developed in a separate thread.

---

## Macro Build Sequence

Full detail in launch-point-os-context.md. High-level order:

- **Phase 1 (active):** Pre-call prep system
- **Phase 2:** Revenue engine gaps — post-call sequences, Kit link click
  notify, outbound via Sales Navigator, Stripe → Notion stage update,
  ScoreApp → Notion CRM routing, cold lead reactivation
- **Phase 3:** Referral and retention engine — client offboarding +
  testimonial sequence, Day 45 referral ask, alumni re-engagement
  sequence, alumni monthly win highlight
- **Phase 4:** Content and marketing system — batch drafting, repurposing,
  sequence monitoring, workshop templatization
- **Phase 5:** Operations cleanup — failed payments, revenue reporting,
  Drive SOP, ClickUp automation, Slack digest, Notion field cleanup
- **Phase 6:** AI-native business rebuild — before next hire, bake AI
  into every function across the business
- **Longer horizon:** Mission Control, Career Compass autonomy,
  LaunchLink, Resume Match, Experience Translator, Second Brain

---

## Contacts

- **Taylor Linder** — Co-owner, wife. Runs finances and serves as
  strategic thought partner. Communicates via Slack.
- **Tara** — Executive Assistant. Manages inbox, calendar, ClickUp
  content tasks, and LinkedIn DM outreach. Team meeting Mondays 1:15 PM,
  check-in Thursdays 10:30 AM. Communicates via Slack.
- **Michelle** — Virtual Assistant. Monthly meeting with Todd.
  Communicates via Slack.
- **Adrienne** — Resume Writer. Team member handling resume writing
  add-on. Monthly meeting with Todd. Communicates via Slack.
- **Ricardo** — Business Mentor. Monthly meeting with Todd.
  Communicates via email.

*All recurring team meetings appear on Google Calendar.*

---

## Things to Remember

- Todd is building toward full automation of his morning reactive block
  (currently 2.5 hours)
- Target: maximum 1 hour of human-touch messaging per day, Claude
  handles the rest. Todd's time for this should decrease as the system
  matures.
- Before sending any message of any kind, always present the draft with
  the last 2-3 messages of conversation context. Wait for explicit
  approval before sending. No exceptions.
- Never schedule a task that hasn't been manually tested first
- Testimonial calls prefer Thursdays when possible
- Alumni follow-up cadence: every 2–3 months via Quo SMS, pull recent
  LinkedIn activity before sending
- Launch Link outreach will be a significant part of Todd's workflow
  once MCP connector is live — plan workstation build at that time
- The pre-call prep Cowork scheduled task (step 4 of Phase 1) needs a
  dedicated design session in the planning space before being built
- Kit.com post-call sequence copy must be written before the Make.com
  routing scenario can be connected — do not build the sequences until
  copy is ready
- Sales Navigator is paid and unused — outbound prospecting via EA is
  planned for Phase 2
- All client-facing Lovable apps (Job Tracker, LaunchLink, Resume Match,
  Experience Translator, Mission Control) are being built in Lovable
  first; Claude Code integration comes later when OS brain is mature
  enough to improve output quality