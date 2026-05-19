# Qual ↔ Quant Alignment

> A thematic bridge between the **5 interviews** in
> [`transcripts/`](./transcripts/) and the **9 survey responses** in
> [`survey/parent_insights_survey.csv`](./survey/parent_insights_survey.csv).
>
> **The two datasets are not joined at the person level.** No survey respondent
> is known to be an interview participant. Every alignment below is **thematic
> triangulation**, not record linkage. Read [`schemas/dataset_overview.md`](./schemas/dataset_overview.md)
> first if you haven't.

---

## Snapshot

| dimension                                | qualitative                                          | quantitative                                         |
| ---------------------------------------- | ---------------------------------------------------- | ---------------------------------------------------- |
| **N**                                    | 5 interviews                                         | 9 survey responses (1 test row dropped)              |
| **Recruitment**                          | Convenience sample, product-bootcamp network         | Convenience sample, online form                      |
| **Geography**                            | 2 Vietnam, 2 US (`USR-04`, `USR-05`), 1 UK     | 1 confirmed US, 5 non-US, 3 unknown (v1 rows)        |
| **Child age range**                      | 20 months → 8 yrs (1 daycare, 3 primary, 1 mid-school) | Elementary, 6-12, 13-18 (mixed v1/v2 encoding)     |
| **Format**                               | Verbatim + summary, free-form questions              | Closed multi-select + Likert + free text             |
| **Strongest for…**                       | Why & how (causal stories, app-specific friction)    | What & how-often (proportions, ranking of choices)   |

---

## Theme alignment matrix

Each row is a theme. The strength of triangulation is rated:
**🟢 strong** (both sides converge) ·
**🟡 partial** (one side covers more) ·
**🔴 conflict** (datasets disagree) ·
**⚪ gap** (only one side has signal).

| # | Theme | Qual evidence | Quant evidence | Triangulation |
|---|---|---|---|---|
| 1 | **More frequent / timely updates** | `USR-04` (US ClassDojo user) rates timing 5/5 importance; was *"supposed to be receiving updates every 30 minutes, which come as a daily report at the end of the day"* · `USR-01` (VN primary-school parent) wants a comprehensive school app with weekly info | `desired_improvements = "More frequent updates on student progress"` selected by **6/9** (`R02, R05, R06, R07, R08, R10`) · `R05` free text: *"Timely is key."* · `R02` (v1): *"Not finding out what class performance is until it's too late"* | 🟢 |
| 2 | **Personalized per-child feedback** | `USR-04` (US ClassDojo user) wants *"breakdown of performance and behavior, along with auxiliary notes from the teachers"* every day · `USR-02` UK parent wants *"more testing, so there's more clarity over how well people are doing"* | `desired_improvements = "Personalized feedback specific to each child's needs and performance"` selected by **4/6** v2 rows (`R04, R06, R07, R08`) · also appears in `challenges` for 5/9 | 🟢 |
| 3 | **Channel fragmentation / multiple tools** | `USR-02` UK parent juggles Google Classroom + email + WhatsApp + gymnastics app, ranks email #1 because the others can't do back-and-forth · `USR-05` mother uses Remind + Infinite Campus + weekly newsletter + direct teacher | `comm_tools_used` average is **2.2 tools** per respondent (max 5 for `R05`) · `desired_improvements = "Easier access to teachers through multiple communication channels"` selected by **4/6** v2 rows | 🟢 |
| 4 | **Teacher / staff adoption of the app** | `USR-04` (US ClassDojo user): *"I attempted to contact the principal via ClassDojo only to find out they don't actually even use the app"* · The doc's own "Key Takeaways" recommend *"building an app for teachers rather than solely for parents"* | `R03` (v1) free text: *"Just teachers being more active and watching out on the app"* · `decision_factors = "Recommendations from other parents or teachers"` selected by `R06` only | 🟡 — qual is sharper. Survey didn't ask the right question. |
| 5 | **Real-time notifications** | `USR-03` (VN daycare parent): *"Usually, you send a message, and the teacher responds when they check the app. It doesn't have any real-time notifications"* — capped his app rating at 4/5 | `decision_factors = "Features offered (e.g., real-time notifications, …)"` selected by **6/9** (highest-selected decision factor in the dataset) | 🟢 |
| 6 | **Safety (esp. younger children)** | `USR-03` (VN daycare parent) (toddler in daycare) ranks cleanliness > food > sleep > teacher ratio as top concerns · in-app camera is his primary use of "Sound & Kits" | `info_useful` includes `"Safety"` for **3/9** (`R03, R08, R10`) — concentrated in elementary / younger bands | 🟡 — qual much richer for daycare age; survey under-samples this band. |
| 7 | **Communication delays / can't save messages** | `USR-04` (US ClassDojo user) (2/5 satisfaction): *"I can't save any of the messages that come through, and message sending is delayed"* | `R02` (also 2/5 satisfaction): *"Not finding out what class performance is until it's too late"* | 🟢 — both 2/5 satisfaction rows independently point at delay. |
| 8 | **Paywall / cost of school-comms tools** | `USR-04` (US ClassDojo user) explicitly: *"The paywall is quite upsetting since I believe it should be the teachers' responsibility to pay for it rather than the parents"* | None. The survey never asked about pricing. | ⚪ — qual-only. Add a price-sensitivity item to next survey wave. |
| 9 | **In-app live camera for younger children** | `USR-03` (VN daycare parent): *"the school has an app that integrates a camera and communicates with parents"* — primary use case | None. Survey didn't list this as an option in `info_useful` or `decision_factors`. | ⚪ — qual-only, but possibly age-specific (daycare). |
| 10 | **SMS / push as preferred channel over apps** | `USR-04` (US ClassDojo user) "Key Takeaway": *"prefers receiving updates via SMS rather than navigating through an app"* | Not explicitly. `comm_tools_used` doesn't list SMS as a discrete option; respondents may have folded it into `Messaging platforms` (only `R05` selected this). | ⚪ — qual-only. Add SMS as an explicit option in next survey. |
| 11 | **Inconsistent communication methods** | `USR-02` UK parent: *"they're not ready for back-and-forward communication"* (different channels for different purposes) | `challenges = "Inconsistent communication methods"` selected by `R05` and `R09` | 🟡 |
| 12 | **Privacy / data concerns** | Not raised in any of the 5 interviews. | `decision_factors = "Privacy and data security concerns"` selected by `R07` and `R08` | ⚪ — quant-only. Probe in next interview round. |
| 13 | **Low-friction / "I have no problem"** | `USR-05` (US mother of 8-yo): *"I don't think I face any challenges really… it's very efficient now"* (4/5 satisfaction) | `R03` and `R04`: both **4/5 satisfaction**, minimal complaints in free text | 🟢 — both confirm a "happy user" persona exists, anchored around weekly-newsletter + multi-app stacks. |
| 14 | **Specific apps named** | `ClassDojo` (USR-01 plan, USR-04 daily); `Google Classroom` (USR-02); `Remind` + `Infinite Campus` (USR-05); `Sound & Kits` (USR-03); `Zalo` (USR-01) | `Class Dojo` named once (`R03`). Otherwise survey only captures category (`School-provided website/apps`, `Messaging platforms`). | 🟡 — qual lets you name the competitor; survey is too coarse. |

---

## What the survey confirms that the interviews suggested

> Useful when you want to defend a qual-driven hypothesis with quant signal,
> however weak.

- **More frequent updates is the #1 ask** (6/9 selected it, every interview
  raised it). Both qual and quant agree this is the primary unmet need.
- **Real-time notifications drive tool choice** (top-selected
  `decision_factors` value at 6/9; explicit pain in USR-03).
- **Personalized per-child feedback is a top-3 ask** in both datasets.
- **A "satisfied parent" persona exists** at sat=4/5, characterised by a
  multi-app self-assembled stack + weekly newsletter (USR-05, R03, R04).
  They are not the right ICP for a new product.

## What the qual found that the survey missed

> These are research gaps — add to next form/interview wave.

- **Pricing / paywall objection** (`USR-04`). Add a "Are you willing to pay
  for school-communication tools?" question.
- **SMS preference over apps** (`USR-04`). Add SMS as a discrete option in
  `comm_tools_used`.
- **In-app camera for daycare-age children** (`USR-03`). Either age-segment the
  next survey or add the option.
- **App stability / crashes** (`USR-03` capped his rating at 4/5 specifically
  because the camera crashes). Add a stability question.
- **Competitor naming.** `comm_tools_used` only captures categories. Add a
  free-text "Which apps?" follow-up.

## What the survey found that the qual missed

- **Privacy & data security concerns** as a tool-choice factor (R07, R08).
  Not mentioned in any of the 5 interviews. Probe in the next round.
- **"Limited or unclear communication from teachers"** as a top challenge
  (`R05, R07`). Interviews surfaced specific delays but not vagueness as a
  category.

## Where the datasets disagree

> Currently **no hard disagreements** — but two soft tensions worth flagging:

1. The 5 interviewees split roughly 2 highly-frustrated (`USR-04`, `USR-03` with caveats)
   vs 3 generally-fine (`USR-02`, `USR-05`, `USR-01`). The survey is more
   uniformly dissatisfied: **5/9 sit at sat ≤ 3**, only 2/9 at sat = 4, none at 5.
   → Possible recruitment bias on the survey side toward unhappy parents.
2. The interviews talk about apps the survey never names; the survey talks
   about features the interviews never name. Both instruments are under-tuned
   for "what tool, doing what, how well".

---

## Recommended uses of this matrix

- **Product prioritization.** Items rated 🟢 are the safest bets — both
  sources point at the same gap. Start with theme #1 (frequent/timely
  updates) and #5 (real-time notifications).
- **Research next steps.** Items rated ⚪ are the highest-information moves
  for the next research cycle.
- **Avoid:** any decision based on a single 🟡 or ⚪ row without explicitly
  saying "this is qual-only" / "this is quant-only" in the write-up.
