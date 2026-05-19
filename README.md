# AI-Powered Qualitative & Quantitative User Insights & Revenue Funnel Analytics

> **Skip the SQL and analytics queue.** Go from complex behavioral questions to deep product and revenue insights in seconds — powered by Cursor and BigQuery via MCP (Model Context Protocol).

---

## The Business Problem

Every product and growth team hits the same bottleneck: **bridging the gap between what users do (Quantitative/Revenue) and why they do it (Qualitative/Behavioral).**

You have urgent questions like:
* *"What does our trial-to-paid funnel look like for users who completed onboarding within 24 hours vs. those who didn't?"*
* *"Which product features are high-revenue users actually interacting with before they subscribe?"*

The answer sits in BigQuery. But between writing massive SQL queries, validating complex event-join logics, formatting outputs, and iterating on follow-ups, what should takes hours. Your product analytics queue becomes the blocker to active decision-making.

The core issue isn't access to data. It's the **latency between behavioral questions and actionable business insights.**

| Traditional Workflow | With Cursor AI Agent + MCP |
| :--- | :--- |
| Write complex behavioral SQL & joins manually | Describe your user insight needs in plain English |
| Debug session-event syntax and schema errors | Cursor handles complex event-log schemas natively |
| Re-run queries for every user segment follow-up | Ask follow-ups in the same continuous chat window |
| Manually stitch funnel numbers with user signals | Get interpreted, unified Quant & Qual answers instantly |
| **Hours to days per deep analysis** | **~10 seconds per deep insight** |

---

## The Architecture

This project connects your AI Code Editor (**Cursor**) directly to **Google BigQuery** using the **Model Context Protocol (MCP)** — an open standard that lets AI assistants call external databases and tools natively.

┌──────────────────────────┐       MCP Protocol        ┌─────────────────────┐
│                          │ ◄──────────────────────►  │                     │
│          Cursor          │    Tool calls / Data     │ mcp-server-bigquery │
│     (AI Assistant)       │                           │ (local MCP server)  │
│                          │                           │                     │
└──────────────────────────┘                           └──────────┬──────────┘
                                                                  │
                                                                  │ Authenticated via
                                                                  │ gcloud CLI
                                                                  ▼
                                                       ┌─────────────────────┐
                                                       │                     │
                                                       │   Google BigQuery   │
                                                       │ (User & Revenue Whse)
                                                       │                     │
                                                       └─────────────────────┘

### How it works:
1. **Ask in Natural Language:** You ask Cursor a question about user behavior, funnel drop-offs, or revenue correlation in the Chat sidebar (Ctrl/Cmd + L) or Composer.
2. **AI Translates & Executes:** Cursor translates it into an optimized SQL query reflecting your database schema via the MCP BigQuery tool.
3. **Local & Secure Run:** The query runs against your BigQuery project using your local `gcloud` credentials. **No data leaves your machine.**
4. **Insight Delivery:** Results are returned to Cursor, which interprets the trends, identifies friction points, and presents them conversationally.

---

## Example Insights & Output Dashboards

### 1. Unified Funnel & Behavioral Analysis
> *"What's the drop-off rate at each step of our onboarding funnel, and what qualitative signals correlate with users who drop off early?"*

```text
[ FUNNEL STAGE ]     [ CONV. RATE ]    [ QUALITATIVE METRIC / DROP-OFF SIGNAL ]
1. Landed Homepage   ■■■■■■■■ 100%     Avg. Session: 45s
2. Registered        ■■■■■■   72%      Friction: Captcha verification errors (8%)
3. Onboarding Compl. ■■■■     48%      High Drop-off: Users skipped the interactive tour
4. Started Trial     ■■■      31%      PQL Signal: 65% of active trials used Feature X
5. Subscribed        ■        8.2%     Conversion latency: Avg. 12 days to upgrade
