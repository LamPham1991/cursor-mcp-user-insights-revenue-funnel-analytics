# Dataset Overview

> The AI should read this file **before** answering any question that touches
> the qualitative or quantitative data. It is the map of what exists, where,
> and how to use it.

This project has **two datasets** that live side by side and have to be
reasoned about jointly. They are **not** linked at the record level — a
survey respondent and an interview participant are different people. Any
alignment is **thematic**, not row-by-row. See
[`../alignment.md`](../alignment.md) for the cross-reference matrix.

---

## 1. Qualitative — interview transcripts

- **Location:** [`../transcripts/`](../transcripts/)
- **Format:** One `.txt` file per interviewee with a metadata header,
  body, and a `Tags:` footer.
- **Count:** 5 interviews (3 verbatim / structured Q&A + 1 summary +
  1 verbatim hybrid).
- **Domain:** Parent ↔ teacher communication, school-monitoring apps.
- **Languages of original recording:** 2 in Vietnamese (translated to English
  before transcription), 3 in English.
- **Recruitment:** Convenience sample from a product bootcamp; not
  representative.

| file                                | id     | participant (anonymized)            | format            |
| ----------------------------------- | ------ | ----------------------------------- | ----------------- |
| `interview_01_vn_primary.txt`       | USR-01 | VN parent, 1st-grade kid            | meeting summary   |
| `interview_02_uk_parent.txt`        | USR-02 | UK father, primary-school daughter  | verbatim Q&A      |
| `interview_03_vn_daycare.txt`       | USR-03 | VN parent, toddler in daycare       | verbatim Q&A      |
| `interview_04_us_classdojo.txt`     | USR-04 | US parent, ClassDojo power user     | structured Q&A    |
| `interview_05_us_satisfied.txt`     | USR-05 | US mother of 8-year-old             | structured Q&A    |

### How to query this data

- Use the filesystem MCP server pointed at `data/` to grep / open files.
- Each file's `Tags:` footer is the agent's fastest path to thematic slicing
  (e.g. `grep -l 'classdojo-user' data/transcripts/`).
- When citing a quote, **always include the `Interview ID`** from the file
  header (`USR-01` … `USR-05`).

---

## 2. Quantitative — parent insights survey

- **Location:** [`../survey/`](../survey/)
- **Files:**
  - `parent_insights_survey.csv` — 9 valid responses, 18 columns.
  - `CODEBOOK.md` — column descriptions, Likert mappings, known issues. **The
    agent must read this before doing any aggregation.**
- **Source:** Google Form "Tracking Your Child's Learning: Parent Insights".
- **Form versions present in the data:** `v1` (3 rows) and `v2` (6 rows). They
  differ in whether the "residing in the US" question was asked, and in how
  the age band is labelled. See codebook §"Form versions in the data".

### How to query this data

The dataset is **tiny** (n=9). Don't write SQL or spin up BigQuery — just read
the CSV directly via the filesystem MCP server and reason in-memory.

If a question requires aggregation, the agent should:

1. Read `CODEBOOK.md` first.
2. Decide whether to filter to a single `form_version` or apply the unified
   age-band mapping.
3. State `n` for every percentage. With n=9, percentages are directional only.
4. Split multi-select fields on `'; '` (semicolon-space), **never** on `,`.

---

## 3. Datasets we do NOT have

To avoid the AI hallucinating sources, here is what is **out of scope**:

- ❌ No BigQuery warehouse. Earlier versions of this repo described a SaaS
  revenue schema (`events`, `users`, `subscriptions`, MRR, churn). None of
  that exists for this project. Disregard any prompt asking for MRR, churn,
  or funnel SQL.
- ❌ No identity bridge between survey respondents (`R02`–`R10`) and
  interview participants (`USR-01`–`USR-05`). Do not invent one.

---

## Hard rules for the agent

1. **Read the codebook before aggregating the CSV.** No exceptions.
2. **Cite sources for every claim.** A claim from the survey cites
   `response_id`(s); a claim from an interview cites the `Interview ID`.
3. **Never collapse across the two datasets as if they were one.** If a
   theme appears in both, present them as two pieces of evidence
   triangulating, not a single sample of `n=14`.
4. **Don't infer demographics that aren't in the data.** E.g. `v1` rows
   have no `us_resident` field — leave it unknown, don't guess from
   occupation.
5. **State sample sizes whenever you give a number.** "5/9 respondents
   said X" not "most respondents said X".
