# Story As Project Tracker — Claude Code Reference

This file is the working context for the Story As Airtable project tracker.
Read this before making any changes.

## 0. Non-negotiable rule: entity separation

Story As, Inc. and Jackrabbit Learning Experience LLC (JLX) are two distinct
legal entities with separate tracking systems, HubSpot accounts, and Airtable
bases. **They must never be commingled.**

- This tracker holds **Story As work only**. No JLX contracts belong in it —
  even for work historically performed by Story As while operating as a JLX
  service arm.
- Training courses are **always** JLX-entity contracts. Categorically excluded.
- **When in doubt, exclude.** Default to leaving something out rather than
  risking a JLX contract landing in the Story As tracker.
- Harvest is JLX-only. Do not build Harvest integration here.
- HubSpot for Story As is not yet populated — the connected HubSpot account
  is JLX's. Story As prospects/contacts live there once set up, not in Airtable.

## 1. What the tracker is

Internal-only, at-a-glance weekly view of every active Story As project:
status, owner, point of contact, next action. Not client-facing.

- Primary weekly user: **Simon** (CEO). Contributors: James, Jason, Marco, Sarah, Darina.
- Documents high-level project health, not day-to-day task work.
- Prospects live in HubSpot, not here. Active projects only.

## 2. Data model

Two linked tables, one base.

- **Projects table** — project-grain. One row = one project, never one client.
  A client can own several rows (e.g. Northwestern → Trienens, NU Water, Wise Scales).
- **SOW table** — contract-grain. One row per Statement of Work, linked to its
  parent project.
- Rollups on Projects (`# SOWs`, `# Needing Attention`) surface contract health
  without opening anything.

**Explicitly out of scope:**
- No granular task tracking (goals/deliverables/deadlines only)
- No financial or cost data
- No speculative features
- No dedicated Clients/Parent-Org table or MSA/Entity-Assignment tracking
  (Parent Org and Client are plain fields on Projects for now)

## 3. Airtable IDs

- Base: `apph33yxxKNjn23Bj` — https://airtable.com/apph33yxxKNjn23Bj
- Projects table: `tblERbkBOwDY4WsMV`
- SOW table: `tblRnLtee8fTaI57w`

### Projects table fields

| Field | Field ID | Type | Notes |
|---|---|---|---|
| Project Name | fldDRRUGOxDudM0zO | singleLineText | Primary field |
| Client | fldkBb2clHrhCUETA | singleLineText | |
| Status | fld7cX7gulOI2Z3Mg | singleSelect | Active / Prospect / Proposal / Closed |
| Work Description | fldkcnkZCVCDm2ur9 | multilineText | |
| Next Action | fldl36WJAPPzJWh29 | multilineText | Most useful weekly field |
| (attachments) | fldJNmEXIElwJKMf2 | multipleAttachments | Largely unused |
| Type | fld2ZfDcdzo0Y6rDy | singleSelect | Website / Report·Deck / Planning / Consulting / Event / Design·Visual Identity / Story·Content / Other |
| (date) | fldLLAezZtayEKu7S | date | Stub, unpopulated |
| POC | fld2KZ8MrMiPC9yT1 | multilineText | |
| (collaborator) | fldynVLnXJLkYah8v | singleCollaborator | Stub, unpopulated |
| Parent Org | fldqp8Y5GORwrQ1Jk | singleSelect | ⚠️ reused old "Priority" column — stray High/Medium/Low options mixed in with org names |
| (AI summary) | fldcOxO7pMOQajz46 | aiText | Legacy, may error |
| (AI summary) | fld8ckvy9mG8iEvqr | aiText | Legacy, over attachments |
| Assigned To | fld3aQFBjfYcGrhp9 | multipleSelects | Deliberately sparse — only fill where source is explicit |
| SOWs | fld5FahVTcPKhDv2x | multipleRecordLinks | → SOW table |
| # SOWs | fldZUCc4U9bQppt6M | count | |
| # Needing Attention | fldD9WWmOh5GH2Ymf | rollup | SUM of SOW `Needs Attention` |

### SOW table fields

| Field | Field ID | Type | Notes |
|---|---|---|---|
| SOW / Engagement | fldulsGfCZWjOzdb8 | singleLineText | Primary field |
| Project | fldkqlsRVk0wxDWqC | multipleRecordLinks | → parent Project |
| Topic (from document) | flduZFquZJm2UMfH8 | multilineText | From SOW Services/Deliverables |
| Sent Date | fldbsBE3Hk1jPzmVN | date (ISO) | |
| Signed by Us | fldCLYudse7i4n2Ju | checkbox | |
| Signed by Client | fldWFFreejkIB3OKz | checkbox | |
| Filed in Drive | fldPTr8HNrRYHygGb | checkbox | |
| Status | fld9l96Ul2qMSJAUo | singleSelect | See controlled list below |
| Next Action | fldPCaeCH6PEeUE6O | singleLineText | |
| Notes | fldR3qSn7ppA5vqx0 | multilineText | Also where normalization notes go |
| Needs Attention | fldvloqdFQwJ60QlT | formula | `IF({Status}="Executed & Filed", 0, 1)` |

**SOW Status controlled options (exact spelling matters):**
Executed & Filed · Executed, not filed · Awaiting client countersignature ·
Under review · Awaiting signature · Stalled · Not started · Not yet formalized

## 4. Other connected systems

| System | Scope | Key IDs |
|---|---|---|
| Google Drive | Story As shared drive | `0AE8LR9pTzQbRUk9PVA` |
| Google Sheets | Legacy project tracker | `10s4YeJl4kqERlu-4n6DKlhGuxncAk5bIJsUqr4A17Sw` |
| Google Docs | ACT meeting notes log | `1zXXGvzzWlYb3nUkC2WPhkuSvgoS3wy5-g0z2VfHEqD4` |
| Google Group | Ops intake inbox — Simon forwards SOW/contract tracking info here | `ops@story.as` (members: James, ops-monitoring session — confirm exact address/membership) |
| HubSpot | JLX only — Story As not yet populated | — |
| Harvest | Out of scope (JLX side) | — |
| Source file | Authoritative SOW master | `SOW_Tracker_Redesigned_DRAFT.xlsx` |

## 5. Working principles

1. **Explicit approval at each structural decision point.** James makes the
   structural call; build to it. No unilateral scope or schema decisions.
2. **Full process transparency.** Keep a running log of decisions, rules, and
   actions during any complex data operation.
3. **Drive documents beat the tracker sheet when they conflict.** The actual
   SOW documents in Drive are authoritative over the spreadsheet.
4. **Normalize, don't silently overwrite.** Status values that don't match a
   controlled option get normalized with a note in that record's Notes field.
5. **Stub-and-populate over wait-for-complete.** Build the structure, then
   fill incrementally.
6. **Scope discipline always.** No speculative features, no financial data,
   no granular task tracking.

## 6. Tool gotchas

**Airtable MCP:**
- Linked-record fields require the target record ID as an array
  (e.g. `["recXXXX"]`) with `typecast: true` on create/update calls.
- Rollup fields need both `recordLinkFieldId` and `fieldIdInLinkedTable` —
  omitting either causes a silent failure.
- `create_field` occasionally hits an approval gate; retry the identical
  call — it typically clears. `update_field` and `update_records_for_table`
  run clean.
- `search_records` is reliable for finding project rows by keyword.
- `get_table_schema` returns field IDs/types/options but not display names —
  read a few records to confirm names.

**Google Drive:**
- A global title search (e.g. `title contains 'SOW'`) pulls from all drives
  the account can see, including JLX archives — don't rely on it to scope
  results.
- `parentId = '0AE8LR9pTzQbRUk9PVA'` returns top-level contents only, not
  recursive. Walk subfolders explicitly via `parentId`, or use the uploaded
  master tracker as source of truth.
- File content is read via `json.loads(result[0]['text'])['fileContent']`.

**HubSpot:** connected account is JLX's — searching Story As university
names returns nothing, confirming Story As contacts aren't there yet.

## 7. Open items

- **Unlinked SOWs** — 14 SOW records have no matching Project row (NC State,
  Princeton, UIC, USC, West Arete, Michigan, Texas, several Northwestern
  sub-units). Decide whether to create Project rows for any of these.
- **WISE SOW2** — reconcile a filed-status discrepancy noted during population.
- **Duke StoryBank SOW1** — no matching Duke project row.
- **"HMMB"** — appears in older notes, unidentified; suspected transcription error.
- **Yale PRISE** — retainer doesn't start until September; open question in
  its Next Action field (keep active now vs. hold).
- **Weekly view** — a filtered/grouped view (by Parent Org, `# Needing
  Attention > 0` rising to top) was proposed but not yet built.

## 8. Current state snapshot (verified 2026-08-28)

- Projects table: 20 active rows (all Status = Active).
- SOW table: 55 records — 41 linked, 14 unlinked (see §7).
- Rollups live and reading correctly.
- Five off-list status values normalized to controlled options, each noted
  in that record's Notes field.
- Six original out-of-scope rows deleted during rebuild: RENU, Kaplan, ABA,
  Associated Colleges, NC State, and a blank orphan.

## 9. Ongoing update process (started 2026-09-03)

**Goal:** stop relying on one-off rebuilds. Simon forwards tracking material
(SOW updates, signed docs, new engagements) to a shared inbox; a recurring
Claude routine reads it and keeps Projects/SOW current. Because this base is
also meant to capture as much project history as possible for future use,
prefer over-logging to silently dropping something forwarded.

- **Intake inbox:** `ops@story.as`, a Google Group. James and Simon are
  both members; the Claude session that runs the routine also needs to be
  a member (or otherwise have read access) so forwarded mail is visible to it.
- **Cadence:** twice weekly (target Monday + Thursday) — a scheduled Routine
  checks the inbox and updates the tracker.
- **Triage rule, per the normalize-don't-overwrite and entity-separation
  principles in §0/§5:**
  - Clear, unambiguous updates (e.g. a status flip to "Executed & Filed",
    a confirmed sent/signed date) get written directly to the matching
    SOW/Project record.
  - Anything new, ambiguous, or requiring a judgment call — especially any
    call on whether an item is Story As vs. JLX — is **not** auto-written.
    It gets logged to the Decisions table (below) for Simon/James to
    resolve, with the source email and any forwarded doc linked.
- **Decisions table:** a new Airtable table in base `apph33yxxKNjn23Bj`,
  not yet created, for logging open items/decisions needing a call. This
  supersedes the ad hoc §7 list going forward — new open items should be
  logged there instead of edited into this file. §7 remains as the
  historical record of items open as of the 2026-08-28 rebuild.
- **Blocked on:** the Gmail and Airtable connectors being enabled for
  whichever Claude Code session runs this routine — as of 2026-09-03,
  neither is enabled in this chat (only Google Drive is). Once enabled,
  create the Decisions table and wire up the recurring Routine.
