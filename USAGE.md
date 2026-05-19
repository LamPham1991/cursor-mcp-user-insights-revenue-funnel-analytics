# Parent-Teacher Communication Research — Cursor + MCP Workbench

A lightweight workspace that turns **Cursor** into a research-analysis co-pilot
by wiring two halves of the truth into one chat:

1. **Qualitative** — 5 in-depth interview transcripts in `data/transcripts/`.
2. **Quantitative** — 9 normalized survey responses in `data/survey/`.
3. **Bridge** — a thematic cross-reference between the two in
   [`data/alignment.md`](./data/alignment.md).

The AI can then answer questions that neither dataset alone can answer, e.g.:

> "Where does the survey actually back up what USR-04 said about update
> frequency, and where is the qual ahead of the quant?"

This repo started life as a generic *user-insights / revenue-funnel analytics*
template (hence the name). It has been narrowed to a **single research
project** — parent ↔ teacher communication for an MVP in EdTech. The
SaaS-revenue / BigQuery scaffolding has been removed; everything now runs
**locally against text files and a small CSV**, with no warehouse.

---

## Repository layout

```
.
├── data/
│   ├── transcripts/                                  # One .txt per interviewee (n=5, anonymized)
│   │   ├── interview_01_vn_primary.txt
│   │   ├── interview_02_uk_parent.txt
│   │   ├── interview_03_vn_daycare.txt
│   │   ├── interview_04_us_classdojo.txt
│   │   └── interview_05_us_satisfied.txt
│   ├── survey/                                       # Survey responses (n=9)
│   │   ├── parent_insights_survey.csv                # 9 rows × 18 cols, two form versions
│   │   └── CODEBOOK.md                               # Columns, Likert mapping, data-quality issues
│   ├── schemas/
│   │   └── dataset_overview.md                       # Hard rules + index the agent reads first
│   └── alignment.md                                  # Qual ↔ Quant theme cross-reference
├── mcp-config-template.json                          # Copy → fill in → save as ~/.cursor/mcp.json
└── README.md
```

There is **no application code**. The "app" is Cursor + a single MCP server.

---

## How it works

```
   ┌───────────────────────────────┐
   │  data/                        │
   │  ├── transcripts/  (5 .txt)   │◀────┐
   │  ├── survey/       (1 .csv)   │     │
   │  ├── schemas/      (rules)    │     │   ┌────────────────┐
   │  └── alignment.md  (bridge)   │     ├───│   Cursor AI    │──► you, in chat
   └───────────────────────────────┘     │   └────────────────┘
                                         │
                       ┌─────────────────┴─────────────────┐
                       │  filesystem MCP server            │
                       │  (read-only, scoped to ./data/)   │
                       └───────────────────────────────────┘
```

- A **single filesystem MCP server** gives Cursor read-only access to `data/`.
  It can grep, summarize, quote interviews verbatim, and read the CSV.
- `data/schemas/dataset_overview.md` is the contract the agent reads first.
  It tells the agent **what exists, what doesn't, and what rules to follow**.
- For a dataset this small (n=9 survey + 5 interviews), there is no benefit
  to BigQuery / SQL / a warehouse. Reasoning happens in-context.

---

## Setup

### 1. Prerequisites

- Cursor ≥ 0.45 (MCP support)
- Node.js ≥ 20 (for `npx` to fetch the MCP server)

That's it. No GCP, no service account, no API keys.

### 2. Configure MCP

```bash
cp mcp-config-template.json ~/.cursor/mcp.json
```

Then open `~/.cursor/mcp.json` and replace the one placeholder:

- `<ABSOLUTE_PATH_TO_THIS_REPO>` — full path to where you cloned this repo,
  so the filesystem server can serve `./data/`.

Restart Cursor. Open **Settings → MCP** — the `filesystem-research` server
should show a green dot.

> The template also ships with **commented-out** entries for BigQuery,
> Stripe, and PostHog. Ignore them unless this project gains a warehouse.

### 3. Adding more data later

- **New interview:** drop a `.txt` into `data/transcripts/` with the same
  header convention as the existing files (`Interview ID`, `Participant`,
  `Source`, etc.) and a `Tags:` footer.
- **New survey responses:** append rows to `parent_insights_survey.csv`.
  If you change the columns or the form, update
  [`data/survey/CODEBOOK.md`](./data/survey/CODEBOOK.md) in the same commit.
- **New theme:** add a row to the matrix in
  [`data/alignment.md`](./data/alignment.md).

---

## Prompt recipes

All recipes work today against the data already in the repo.

### Qualitative

**Q1 — Synthesize themes across all 5 interviews**
> "Read every `.txt` in `data/transcripts/`. Produce a 1-page synthesis with
> (a) the top 5 recurring pain points ranked by interviewee count,
> (b) the apps/channels each interviewee used, and (c) one verbatim quote per
> theme with the `Interview ID`."

**Q2 — Build a JTBD table**
> "For each interviewee, extract the underlying job the parent is hiring
> their current tool to do. Output a Markdown table: interview_id, persona,
> job_to_be_done, current_tool, satisfaction_1to5, top_unmet_need."

### Quantitative

**N1 — Frequency table for a multi-select column**
> "Read `data/survey/CODEBOOK.md`, then for each value of
> `desired_improvements`, count how many of the 9 respondents selected it.
> Output sorted descending. State n for every row."

**N2 — Compare v1 and v2 form respondents**
> "Group respondents by `form_version`. For each group, give me the mean
> `importance_score` and `satisfaction_score`, and note which features of
> the dataset differ between versions (per the codebook)."

### Mixed — the actual value of this workbench

**M1 — Triangulate a theme**
> "Take the theme 'more frequent updates'. Find every interview quote that
> supports it (with `Interview ID` — USR-01 to USR-05) and every survey row
> that supports it (with `response_id` and the column where it appears).
> Rate the strength of triangulation 🟢 / 🟡 / 🔴 / ⚪."

**M2 — Find a research gap**
> "Read `data/alignment.md`. List every theme rated ⚪ (qual-only or
> quant-only). For each, propose one new survey question and one new
> interview probe that would convert it to 🟢."

**M3 — Score an MVP feature with evidence from both sides**
> "I'm considering shipping 'real-time push notifications when a teacher
> posts an update.' Pull every relevant piece of evidence from both
> transcripts and survey, score the strength of the case 1–5, and tell
> me what evidence is missing before I'd commit."

---

## Hard rules for the agent

These are encoded in [`data/schemas/dataset_overview.md`](./data/schemas/dataset_overview.md):

1. Read `dataset_overview.md` and the relevant codebook **before** answering.
2. Cite sources: interview claims cite `Interview ID`, survey claims cite
   `response_id`.
3. Never collapse the two datasets into one `n=14` sample. Triangulation only.
4. With `n=9` survey responses, state n for every percentage. Treat all
   numbers as directional.
5. Split multi-select fields on `; ` (semicolon-space), never on `,`.

---

## FAQ

**Q: Will my interview transcripts be sent to a third party?**
A: Interview text is sent to whichever model you've selected in Cursor as
part of the chat context, like any other file you open. Strip PII before
committing if that's a concern.

**Q: Why no BigQuery / SQL setup?**
A: n=9. SQL would be theatre. The earlier version of this repo had a
SaaS-revenue BigQuery template that didn't match the actual research, so
it was removed. If the dataset grows past a few hundred rows or you start
tracking app telemetry, add a BigQuery MCP entry to `mcp-config-template.json`
(the commented block is still there) and a real schema doc to `data/schemas/`.

---

## Privacy & ethics

All interview transcripts in this repo have been anonymized:

- Participant and interviewer names replaced with `[Participant]` /
  `[Interviewer]` placeholders.
- Filenames refer to demographic descriptors (region + child age band +
  primary tool), not to individual people.
- The survey CSV (`data/survey/parent_insights_survey.csv`) contains no
  direct identifiers; respondents are referenced by synthetic IDs (`R02`–
  `R10`).

If you spot residual identifying language and you are a participant,
please open an issue and the affected file will be redacted further.

## License

Code (MCP config, scripts, schema docs, README) is released for
educational reuse. Research transcripts and survey data are published as
study artifacts — please credit this repository if you cite them, and do
not attempt to re-identify participants.
