# Survey Codebook — Parent Insights

> The single source of truth for the AI when reading
> [`parent_insights_survey.csv`](./parent_insights_survey.csv).
>
> If you re-export new responses from the Google Form, **update this file in
> the same commit** so the agent doesn't drift from the underlying data.

---

## Provenance

- **Source:** Google Form titled *"Research Study Invitation — Tracking Your
  Child's Learning: Parent Insights (Responses)"*
- **Sheet URL:** https://docs.google.com/spreadsheets/d/1bWpD3j0CJzuJGrwi_aBi5W4YoCoq7rEYRHc4XcoMBis/
- **Pull date:** 2026-05-19
- **Response window:** 2024-09-05 → 2024-09-21
- **Raw rows in sheet:** 10
- **Valid rows in CSV:** 9 (one test row `9/4/2024 23:11:11` containing
  `n/a testing` was dropped)

## Form versions in the data

The form was edited at least once between the first and last response. Both
versions are preserved in the CSV via the `form_version` column.

| version | n  | distinguishing feature                                    |
| ------- | -- | --------------------------------------------------------- |
| `v1`    | 3  | No "Are you currently residing in the United States?" field. `us_resident` is empty for these rows. `child_age_band` uses school-level labels (`Elementary School`, `Secondary School`). |
| `v2`    | 6  | Adds `us_resident` (Yes/No). `child_age_band` uses age ranges (`6-12 years`, `13-18 years`). |

When aggregating, **do not** mix the two age-band encodings in a single bucket.
Either filter to one `form_version`, or map them with the table in the
[Value mappings](#value-mappings) section.

---

## Columns

| Column                  | Type            | Description                                                                                          |
| ----------------------- | --------------- | ---------------------------------------------------------------------------------------------------- |
| `response_id`           | string          | Stable ID we assigned (`R02`–`R10`). Matches the source-sheet row number minus 1.                    |
| `form_version`          | enum            | `v1` or `v2`. See table above.                                                                       |
| `timestamp`             | ISO 8601 (UTC?) | Submission time as recorded by Google Forms. Timezone is **not** specified in the source.            |
| `occupation`            | string          | Free text. No normalization.                                                                          |
| `us_resident`           | enum/null       | `Yes` / `No` / empty (for `v1` rows where the question didn't exist).                                |
| `child_age_band`        | string          | See [Value mappings](#value-mappings) — semantics differ by `form_version`.                          |
| `school_type`           | string          | Free text but in practice always `Public School` in this dataset.                                    |
| `info_useful`           | multi-select    | Semicolon-separated list. See [Value mappings](#value-mappings).                                     |
| `importance_label`      | enum            | Verbatim Likert label (`Extremely important`, `Very important`, etc.).                               |
| `importance_score`      | int 1–5         | Numeric encoding of `importance_label`. See mapping below.                                           |
| `challenges`            | multi-select + free text | Semicolon-separated. Some entries are free text appended by the respondent.                 |
| `comm_tools_used`       | multi-select    | Semicolon-separated.                                                                                  |
| `satisfaction_label`    | enum            | `Dissatisfied`, `Neutral`, `Satisfied`. (No `Very` variants observed in this dataset.)               |
| `satisfaction_score`    | int 1–5         | Numeric encoding. See mapping below.                                                                  |
| `decision_factors`      | multi-select    | What makes the respondent pick / drop a tool. Semicolon-separated.                                   |
| `desired_improvements`  | multi-select    | Semicolon-separated.                                                                                  |
| `other_comments`        | string / null   | Free text. Empty for 8/9 respondents.                                                                |

### How multi-select fields are encoded

Google Forms exports multi-select as a comma-separated string. Several option
labels in this form contain commas of their own (e.g.
`"Features offered (e.g., real-time notifications, progress tracking, messaging)"`),
which makes comma-splitting unsafe.

**In the CSV, multi-select fields use `; ` (semicolon-space) as the option
separator.** Always split on `; ` — never on `,`.

---

## Value mappings

### `importance_score` (Likert 1–5)

| label                  | score |
| ---------------------- | ----- |
| `Not at all important` | 1     |
| `Slightly important`   | 2     |
| `Moderately important` | 3     |
| `Very important`       | 4     |
| `Extremely important`  | 5     |

### `satisfaction_score` (Likert 1–5)

| label               | score |
| ------------------- | ----- |
| `Very dissatisfied` | 1     |
| `Dissatisfied`      | 2     |
| `Neutral`           | 3     |
| `Satisfied`         | 4     |
| `Very satisfied`    | 5     |

> ⚠ The form description says "Linear scale (1–5)" but the **export contains
> labels, not numbers**. Whoever set up the form replaced the default 1–5
> radio buttons with named options. The numeric encoding above is reconstructed
> by us and may differ from a strict 5-point Likert if the form is ever
> changed back to numbers.

### `child_age_band` cross-form mapping

The two form versions use different bands. Use this table when you need to
compare across versions:

| v1 label            | v2 label        | unified band   |
| ------------------- | --------------- | -------------- |
| `Elementary School` | `6-12 years`    | `elementary`   |
| `Secondary School`  | `13-18 years`   | `secondary`    |
| (not in v1)         | `0-5 years`     | `early_years`  |

R03 (`Elementary School`) and R10 (`6-12 years`) belong to the same unified
band `elementary`, etc.

### `info_useful` — canonical option list

These are the checkboxes the form offered (we observed all of them in
responses):

- Safety
- Grades and test scores
- Homework assignments
- Teacher feedback
- Attendance records
- Behavioral reports
- Socialization

### `comm_tools_used` — canonical option list

- Emails
- Phone calls
- In-person meetings
- School-provided website/apps
- Messaging platforms
- (free-text additions observed: `Class Dojo`, `Quick conversation at pick up/drop off`)

### `decision_factors` — canonical option list

- Ease of use and user-friendly interface
- Privacy and data security concerns
- Quality and frequency of updates provided by the app
- Recommendations from other parents or teachers
- Features offered (e.g., real-time notifications, progress tracking, messaging)
- Positive reviews and ratings from other users
- (free-text observed: `How user friendly it is`, `If it's not a way to communicate and see updates about my child`)

### `desired_improvements` — canonical option list

- More frequent updates on student progress
- Clearer and more concise communication
- Easier access to teachers through multiple communication channels
- Personalized feedback specific to each child's needs and performance
- Timely responses to parent inquiries or concerns

---

## Known data-quality issues

1. **Tiny N.** 9 valid responses. Treat all percentages as directional, not
   statistically reliable.
2. **Geography skew.** Only 1 of 6 `v2` respondents said `Yes` to "residing in
   the US"; the rest are international. The `v1` rows don't have this field at
   all. Do not generalize US-vs-international claims from this sample.
3. **Mixed school-type / age semantics across form versions.** See above.
4. **Free-text contamination in `challenges`.** R02 wrote a complaint
   (`"Not finding out what class performance is until it's too late"`) instead
   of selecting from the offered checkboxes. R03 wrote just `"Communication"`.
   Don't treat these as canonical option values.
5. **R03 `comm_tools_used = "Class Dojo"`** is a single free-text answer, not a
   multi-select. Don't split it.
6. **Timezone unknown.** Google Forms records the submitter's local time by
   default but doesn't always export the offset. Treat timestamps as
   approximate.
7. **No respondent linkage to interviews.** The 5 in-depth interviewees in
   `../transcripts/` are **not** the same people as these 9 survey
   respondents. Alignment is thematic, not per-person. See
   [`../alignment.md`](../alignment.md).
