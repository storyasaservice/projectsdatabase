# Story As Project Tracker — Claude Code Reference

This file is the working context for the Story As Airtable project tracker.
Read this before making any changes. Last rewritten 2026-10-12 against the
live schema; it replaces the 2026-09-03 version, which predated the 9/22 ACT
workflow and the 9/29 schema cleanup. Updated 2026-10-15: contract-coverage
fields, the new Card Line field, Whiteboard card rules, Simon's weekday
routine and the [Archive] naming for old logs.

The authoritative append-only history of what was decided and why is the
Drive log in **Project Tracking** (see §4). This file is a working summary.

## 0. Non-negotiable rule: entity separation

Story As A Service, Inc. and Jackrabbit Learning Experience LLC (JLX) are two
distinct legal entities with separate tracking systems, HubSpot accounts, and
Airtable bases. **They must never be commingled.**

- This tracker holds **Story As work only**.
- Scope rule as updated 2026-09-22: projects Story As actually delivers are
  tracked, including Story As work done on older contracts JLX signed (e.g.
  Excelsior SOW1–10, Kean, HMMB). JLX's own work is never tracked here.
- Training courses are **always** JLX work. Categorically excluded.
- JLX's own website, finances, staffing and vendors (bookkeeping, Azurium,
  etc.) are JLX matters. Leave them out even when they come up in a shared
  "Story As + JLX | ACT" meeting.
- **When in doubt, exclude** and ask James or Simon.
- Harvest is JLX-only. Do not build Harvest integration here.

## 1. What the tracker is

Internal-only, at-a-glance view of every live Story As project, built to
prepare the weekly **ACT** meeting (Simon, James, Jason). Not client-facing.

- Answers two questions: where does every active project stand, and what
  deliverables are coming up?
- Sales (proposals, verbal yeses, unsigned SOWs, signatures, renewals, new
  deals) is Simon's, in HubSpot. It never gets an ACT Flag or Next Action.
- Not a contract-admin or task tool: no granular tasks, **no financial data**
  (fees, invoices, costs), no speculative features.

## 2. Data model

Two linked tables in one base.

- **Projects** — project-grain. One row = one project, never one client. A
  client can own several rows (Northwestern → Trienens retainer, Trienens
  Visual Identity Phase 2, NU Water…). Retainers are their own rows. Status:
  Active / Prospect / Proposal / Closed. Proposal rows are sales.
- **SOWs** — contract-grain. One row per Statement of Work, linked to its
  project. Contract facts only (term, deliverables, signatures, document
  link). Next-step info lives on Projects (`Next Action` + `Due`).
- Splitting out a separate **Clients** table was proposed at the 9/30 ACT.
  It is not built and not approved. Don't build it without James's sign-off.

## 3. Airtable IDs and fields

- Base: `apph33yxxKNjn23Bj` — https://airtable.com/apph33yxxKNjn23Bj
- Workspace: **Story As Primary Workspace** (`wsptEf3s31waD0PG7`), owned by
  the Story As Airtable account. James moved the base here from the JLX
  workspace on 2026-10-13 with Airtable's Move base; the base ID, table IDs,
  field IDs and record IDs all stayed the same (verified after the move).
- Projects: `tblERbkBOwDY4WsMV`
- SOWs: `tblRnLtee8fTaI57w`
- ACT Headlines view (Projects, ACT Flag not empty, sorted on `Due`):
  `viwPepItyTxwcPj9K`
- Decisions: `tblCWLdCkpz3DETd4` (created 2026-09-03) is **gone** from the
  base.
- Questions: `tblezlMZ5zJbua29I` (added 2026-10-15). Things Simon (or
  James/Jason) needs to explain or decide: contract gaps, missing signed
  copies, odd dates. Not delivery issues (those use ACT Flag). Replaces the
  old Questions for Simon doc; its open items were moved in on 10/15 and the
  doc is now `[Archive] 2 · Questions for Simon`.

### Projects fields (live schema, 2026-10-12)

| Field | Field ID | Type | Notes |
|---|---|---|---|
| Project | fldDRRUGOxDudM0zO | singleLineText | Primary. "Client — specific project name" from the SOW |
| Client | fldkBb2clHrhCUETA | singleLineText | Specific client/sub-unit |
| Status | fld7cX7gulOI2Z3Mg | singleSelect | Active / Prospect / Proposal / Closed |
| Notes | fldkcnkZCVCDm2ur9 | multilineText | **Append only.** Dated history; old Next Actions land here |
| Next Action | fldl36WJAPPzJWh29 | multilineText | "Due M/D — what". Live but undated work: "Due TBD — what" with Due blank (Whiteboard tag "Date TBD"). Blank only when nothing specific is in progress |
| Due | fldzBD35sasrNRos6 | date | Date of the Next Action deliverable. Set and clear together with Next Action |
| ACT Flag | fldMEtAAai6aqfMOu | singleSelect | See ACT rules below |
| Card Line | flde7ZQbRqXa7ZQl0 | singleLineText | Added 10/15. The one line the Whiteboard card shows: WHY the row has its flag. Rewritten, not appended. See ACT rules |
| Category | fld2ZfDcdzo0Y6rDy | singleSelect | |
| POC | fld2KZ8MrMiPC9yT1 | multilineText | Client-side point of contact |
| Parent Org | fldqp8Y5GORwrQ1Jk | singleSelect | Umbrella institution. Reused old Priority column; has dead legacy options |
| Assigned To | fld3aQFBjfYcGrhp9 | multipleSelects | Simon / James / Jason / Marco / Sarah / Darina |
| SOWs | fld5FahVTcPKhDv2x | multipleRecordLinks | → SOWs |
| # SOWs | fldZUCc4U9bQppt6M | count | |
| OLD SOW Term Starts (any status) | fldAHeQKLe1MPE54s | rollup | **Deprecated 10/14.** Use Current Term Starts / First Engagement |
| OLD SOW Term Ends (any status) | fldj9PNzOGgKus5Bj | rollup | **Deprecated 10/14.** Counts unsigned/declined SOWs. Don't use. Safe to delete (no Timeline view exists) |
| Contract Coverage Ends | fldymNUumnGejgKIv | rollup | Latest end date across SIGNED SOWs = when signed coverage runs out. Renewals = sales, not ACT |
| Current Term Starts | fldvgCPxbeGnDcDFd | rollup | Start of the most recent signed SOW that has begun |
| First Engagement | fldRmPboS1382NWcb | rollup | Earliest start across signed SOWs |
| Signed SOWs | fld1CdZd4yqlG3XUH | rollup | Count of signed SOWs |
| SOWs Pending Signature | fldUfA5kChlBUWotm | rollup | Count out for signature |
| Signed SOWs Missing Dates | fldPA2d7fwMozZssp | rollup | >0 means Contract Coverage Ends may be wrong |
| Coverage | fldALr1TDJXfLgKUw | formula | Covered / Expiring soon (≤60 days) / Expired / Expired — SOW awaiting signature / Unsigned — awaiting signature / No signed SOW / Missing dates / No SOW. Drives the Whiteboard re-sign chips |
| Project Summary (AI) | fldcOxO7pMOQajz46 | aiText | Legacy |

### SOWs fields (live schema, 2026-10-12)

| Field | Field ID | Type | Notes |
|---|---|---|---|
| SOW / Engagement | fldulsGfCZWjOzdb8 | singleLineText | Primary, e.g. "Trienens Institute — SOW12 (visual phase 2)" |
| Project | fldkqlsRVk0wxDWqC | multipleRecordLinks | → Projects |
| SOW Term Start | fldgj7pNkkYH5tQeI | date | From the SOW document |
| SOW Term End | fldQ5Dv4ss1EWWTtC | date | From the SOW document |
| Deliverables | flduZFquZJm2UMfH8 | multilineText | Was "Topic". Static copy of what the SOW contracts for: "N deliverables:" then one bullet each |
| Status | fld9l96Ul2qMSJAUo | singleSelect | Controlled list below |
| Signed by Us | fldCLYudse7i4n2Ju | checkbox | |
| Signed by Client | fldWFFreejkIB3OKz | checkbox | |
| SOW Document | fldqDiwA0pxtzR1LX | url | Direct Drive link to the source document |
| Notes | fldR3qSn7ppA5vqx0 | multilineText | Append only. Normalization and source notes go here |
| Signed / Pending Signature | fldr1TcZxhZ6NF1Ly / fld3EV692w3Fq4hZ9 | formula | 1/0 flags from Status |
| Term State | fldCKi4aHcktAJgAr | formula | Declined / Unsigned / Missing dates / Upcoming / Current / Expired |
| Signed End / Signed Start / Begun Signed Start | fldqyuOEUf6lX1qxW / fldzBh9SPwTjN6w2f / fldHxQznYBr1WDPow | formula | Feed the Projects coverage rollups |

Removed on 9/29 (renamed "DELETE — …", then deleted): SOW Next Action, Sent
Date, Filed in Drive, Next Deliverable, Due Date, Needs Attention, and the
Projects rollups that depended on them. Unused Projects fields were also
dropped. Don't reference any of them.

### Questions fields (2026-10-15)

| Field | Field ID | Type | Notes |
|---|---|---|---|
| Question | fldfMFAH9usykSQV5 | singleLineText | Primary. Short enough to read aloud at ACT |
| Context | fldsmv2ri1pjwWR4T | multilineText | What the records/Drive show, and what changes once answered |
| Ask | fldqE2tSvOdogu2Fh | singleSelect | Simon / James / Jason |
| Status | fldMSIgwNEvmwrk2h | singleSelect | Open / Answered / Dropped |
| Topic | fldLtgo86KoBnLdcr | singleSelect | |
| Project / SOW | fldSfNqythe4foIC2 / fld7ySJLAiYEqHDKs | multipleRecordLinks | Back-links: Projects `fldgFUDyEaqMvlp7s`, SOWs `fldE9xyHwRw8cH8WX` |
| Answer | fldOqvbuqDXdMaDxk | multilineText | Simon's words where possible |
| Asked / Answered On | fld1CJevkmX8fjOuy / fldcm3ZTACq2M5z1y | date | |

Workflow: Simon answers at ACT. When notes are processed, write the Answer,
set Status Answered + Answered On, **and fix the underlying Project/SOW
record**. Simon's routine may add and answer rows but leaves record fixes to
a person.

**SOW Status options (exact spelling):** Executed & Filed · Executed, not
filed · Awaiting client countersignature · Under review · Awaiting signature
· Stalled · Not started · Not yet formalized · Declined

### ACT rules (James, 9/22–9/29)

- Every **Active** row gets exactly one ACT Flag. Priority when several apply:
  **Issue > Due This Week > Win > Coming Up**.
  - **Issue**: a delivery problem (late, at risk, stuck, rescoped, unhappy
    client), including a `Due` date that passed with no sign of delivery.
    Contract or signature matters are never Issues.
  - **Due This Week**: `Due` falls within 7 days of the ACT date.
  - **Win**: rare. A specific, exemplary client reaction to our work in the
    last ~2 weeks. Never for signing, renewing or simply delivering.
  - **Coming Up**: every other Active project in play, dated or not.
- Proposal and Closed rows: flag blank, Next Action blank.
- "Due Soon" and "Update" are retired. "Update" is still a dead option on the
  field; the Whiteboard shows such rows under Coming Up.
- Changing a Next Action: first append the old text to Notes as
  `\n\n[Status M/D/YY] <old text>`, then write the new one. Never delete
  history.
- **Card Line** (James, 10/15): every Active row has one, so the Whiteboard
  works without clicking into a card. Issue = the specific problem. Win = who
  said what about which work, and when ("10/12: Amber & Christian said the
  core story made them feel seen"). Due This Week / Coming Up = the
  deliverable and date, or what's happening now ending "nothing dated".
  Under ~110 characters, no financial figures. Blank on Proposal/Closed.
- Re-signs are still sales and never get a flag. The Whiteboard shows them
  as chips computed from `Coverage`.

## 4. Other connected systems

| System | Scope | Key IDs |
|---|---|---|
| Google Drive | Story As shared drive (source SOW/MSA documents) | `0AE8LR9pTzQbRUk9PVA` |
| Drive folder | **Clients** (shared with james@story.as): one subfolder per client, holding SOWs and work files. The repo session's Drive connection can read it | `1gJjAicV-FFM4Y1yVq2jAaDWFqKrIoWQz` |
| Drive folder | **Project Tracking** (visible to james@story.as since 10/13). Flat layout: `0 · START HERE` index, Tools & limitations, the latest session log, the latest ACT prep log, Questions for Simon, the routine instructions and setup docs. Everything older goes in **Archive** (`13R7bogMArKydWbWoRyGzrOlJzn8DfkK7`). New logs go at the top level. When a new log lands, rename the previous one of the same kind so its title starts with `[Archive] ` (James, 10/15); a person then moves `[Archive]` files to Archive by hand, since the connector can't move files here. Simon's routine does this for its own ACT prep logs. Naming (10/13): standing docs `0 · START HERE`, `1 · Tools & limitations`, `2 · Questions for Simon`, `3 · ACT prep routine instructions`, `4 · Routine setup (one time)` (archive once Simon's routine runs); session logs `Log · YYYY-MM-DD · what it covers`; ACT prep runs `ACT prep · YYYY-MM-DD`. Renaming works through the connector even though moving doesn't | `1FfXGfTU-FmjO3Mgy8YSG0i04lMhgYuLd` |
| Google Docs | ACT agenda/notes log (newest at top, very large) | `1zXXGvzzWlYb3nUkC2WPhkuSvgoS3wy5-g0z2VfHEqD4` |
| Google Group | `ops@story.as` ("Operations"). Simon and James forward tracking material here; subjects tagged `[Story As Ops]`. Delivery to james@story.as confirmed (threads arrive under CATEGORY_FORUMS) | — |
| Whiteboard | Read-only live kanban of Projects by ACT Flag, owned by the Story As claude.ai account. Source: `whiteboard/index.html`. Since 10/15 (v3) each card shows its Card Line (Issue/Win) or Next Action (else Card Line), a due chip, and a re-sign chip from `Coverage` (Re-sign · ends DATE when ≤60 days; Lapsed DATE; SOW awaiting signature; No signed SOW; SOW dates missing). Coming Up sorts dated work, then re-signs. Reads Projects fields incl. Card Line `flde7ZQbRqXa7ZQl0`. Also reads the Questions table: open questions show in an "Ask Simon" section below the columns and as a "? N for Simon" chip on the linked project's card | https://claude.ai/artifact/7S4KW12R9qgtm7BVR15dza |
| HubSpot | Story As sales/pipeline (Simon). The HubSpot connector seen so far was JLX's | — |
| Harvest | Out of scope (JLX) | — |

The old Whiteboard copy on James's JLX account
(https://claude.ai/artifact/3HWJxHvKbbG8LynnE1rn33) is to be deleted by James
once the new one is confirmed.

## 5. Working principles

1. **Explicit approval at each structural decision point.** James makes the
   structural call; build to it. No unilateral scope or schema changes.
2. **Full process transparency.** Log decisions and actions; long operations
   get a dated entry in the Project Tracking Drive log.
3. **The SOW document wins.** Drive documents are authoritative over the
   tracker spreadsheet, and the signed SOW is authoritative over email
   summaries. For naming, use the SOW's name even if it's imperfect (e.g. UT
   Austin "Biological Sciences", not "Molecular Biosciences").
4. **Normalize, don't silently overwrite.** Append, with a dated note naming
   the source. Never fabricate a date or status; leave it blank and say why.
5. **Scope discipline always.** No financial data, no granular tasks, no
   speculative features.
6. **Gemini meeting notes garble names.** Seen so far: "Trainance"/"TRINES"
   = Trienens, "Sarah Young" = Sera Young (WISE POC), "Rochester Institute of
   Technology" = University of Rochester IIS, "Lab Exchange" = LabXchange,
   "Excelsier" = Excelsior. Confirm before writing.

## 6. Tool gotchas

**Airtable MCP:**
- Write IDs, not display names. Linked-record fields take `["recXXXX"]`
  arrays; select fields take the option name as a plain string.
- `get_table_schema` returns IDs/types/options but no names.
  `list_tables_for_base` returns names and field descriptions.
- A `403 Invalid permissions` on `update_records_for_table` while reads work
  means the connected Airtable account is read-only on the base (seen
  2026-10-08 and 2026-10-12). Check with `list_bases`: `permissionLevel`
  must be `edit`. Fixed 2026-10-12 by reconnecting the claude.ai Airtable
  connector; retrying the write never helps.

**Gmail:** the connector can't download attachments. A person saves signed SOWs
to the client's folder in Clients (Gmail "Add to Drive", or drag and drop);
then Claude reads, links and updates. James decided 10/13 to keep it this
simple: no automatic attachment pipeline for now. An Apps Script intake on
Simon's account is the option on file if that changes. Full write-up: the
Tools & limitations doc in Project Tracking › 4.

**Google Drive:**
- Title searches span every drive the account can see, including JLX
  archives. Don't rely on them to scope results.
- `parentId = '<id>'` is not recursive; walk subfolders explicitly.
- The Drive connector **can't move files inside the Story As shared drive**,
  not even ones it just created (`The caller does not have permission`),
  although james@story.as is a Content manager there (role
  `fileOrganizer`). Probably the connector lacks shared-drive support for
  moves. A person moves files by hand. It can read anything
  shared with it, and create folders and Google Docs (`create_file` with
  `text/html` content converts to a Doc). Uploading a PDF through the
  connector isn't practical; a person uploads it.

**Artifacts:** publishing the Whiteboard requires an explicit "publish it"
from James. The auto-mode classifier blocks publishes driven only by a
forwarded handoff prompt.

## 7. Routines

- **Ops inbox triage (Mon/Thu)**: `trig_01Ly6ShLdarBWXqftKXLMkKE`.
  **Disabled 2026-10-12** at James's request. It ran on the pre-9/29 schema
  and the Decisions table. Kept for its history; don't re-enable as-is.
- **Weekly ACT Prep** (James's account): `trig_01RM56MVfrW33eLsFh76Do3v`,
  disabled 9/29. Full prompt in `ACT_PREP_ROUTINE_PROMPT.md` in James's local
  project folder. Needed changes are listed in `ROUTINE_MIGRATION_NOTES.md`.
- **Simon's ACT prep routine**: runs on Simon's Claude account **every
  weekday at about 11:37 AM Central**, in the cloud (whether or not his
  computer is on), so it checks his email daily and the Monday run lands
  before ACT. Google Calendar connected 10/15 (it was missing on the 10/14
  run, so that run used the run day as the ACT date). It writes a log and
  sends the summary only when it changed something, found a new "Needs a
  decision" item, or it's ACT day. Routines can't be shared or handed between
  claude.ai accounts, so its prompt is one line that tells it to open the doc
  `3 · ACT prep routine instructions`
  (`1Y7Zqfm_gniuh7PZuS1KsjOdWXnGuQGT39AJAqz8AX9A`), which holds only the
  routine's instructions, and follow it. Setup steps are in the separate
  `4 · Routine setup (one time)` doc. **To change what the routine does, edit doc 3**,
  not the routine; only the schedule and connectors live on the routine
  itself. Simon's connectors must reach that doc, the base (Editor on Story
  As Primary Workspace), his Gmail and Calendar, and the shared drive. Manual
  Airtable edits made between runs (e.g. Win flags) may be overwritten on
  its next run; that's expected.
- Routine rules that carry over: the only email sent is the run summary,
  from Simon's Gmail to simon@story.as and james@story.as only (James,
  10/15); anything else is a draft; append to Notes, never delete; no sales
  flags; anything Story As vs. JLX goes to a human.

## 8. Current state snapshot (2026-10-12)

- Projects: 40 rows: 20 Active, 2 Proposal (NC State PRTM, Michigan
  Neuroscience), 18 Closed.
- SOWs: about 65 rows. A 9/29 Drive audit (`sow_audit_2026-09-29.json`)
  filled links, term dates and deliverables on most of them.
- All SOWs are linked to a Project (the 14 orphans from 8/28 were resolved
  on 8/28).

## 9. Open items (2026-10-15)

- **Simon's first weekday run with auto-send**: confirm the summary email
  actually arrives (a cloud routine may need send permission approved once)
  and that the ACT date comes from Calendar.
- **Michigan Neuroscience**: work is underway but the row is Proposal. A
  completed Adobe Sign agreement came in 10/13. Status change and SOW row
  are for a person. Empty folder Clients › University of Michigan
  (`1CB9tT7Y4nR4ap882RkoifCL28RXbIv7I`) created 10/12 for it.
- **Trienens SOW13 PDF** sits loose in Clients › Trienen's Institute; a
  person has to move it into the `SOW` subfolder. SOW12 and SOW13 are both
  linked and Executed & Filed as of 10/13.
- **Duke Climate Commitment Notes** hold financial figures from an earlier
  run (10/12 entry). James may delete them by hand.
- **Clients table split** (from 9/30 ACT): proposed, not approved.
- **OLD rollups**: Projects has no Timeline view (views: Grid view, Active -
  RYG, ACT Headlines), so the two OLD rollups can be deleted in the Airtable
  app. The connector can't delete fields.
- **Misfiled Drive file**: `1OOlUuX7mErSXsPYohnZ-NEtmSx2Syh6u` holds NU Water
  SOW9 but sat in the Brown folder as "Brown MSA + SOW1.pdf". Renamed 10/15
  to say so; a person moves it.
