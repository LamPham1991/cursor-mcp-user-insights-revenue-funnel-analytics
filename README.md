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
# Revenue Funnel Conversion Analytics

<img width="797" height="346" alt="Screenshot 2026-05-19 at 14 44 34" src="https://github.com/user-attachments/assets/83f0c8aa-6a3c-409c-bc0c-e61104a75b11" />

### How it works:
1. **Ask in Natural Language:** You ask Cursor a question about funnel drop-offs, or revenue correlation in the Chat sidebar (Ctrl/Cmd + L) or Composer.
2. **AI Translates & Executes:** Cursor translates it into an optimized SQL query reflecting your database schema via the MCP BigQuery tool.
3. **Local & Secure Run:** The query runs against your BigQuery project using your local `gcloud` credentials. **No data leaves your machine.**
4. **Insight Delivery:** Results are returned to Cursor, which interprets the trends, identifies friction points, and presents them conversationally.

# Qualitative & Quantitative User Insights
<img width="528" height="554" alt="Screenshot 2026-05-19 at 16 29 00" src="https://github.com/user-attachments/assets/a76d6d89-6dae-4c1a-89a4-70e20823e8a9" />

### How it works:
1. **Ask in Natural Language:** You ask Cursor a question about user product behavior (e.g., "What is the drop-off rate at the KYC step?") or user feedback (e.g., "What are the top 3 user complaints about our new feature?") in the Chat sidebar or Composer.

2. **AI Intent Routing & Execution:** Cursor analyzes your request and routes it dynamically via the mcp-server-user-insight tool:

For Quantitative Data: It calls the Analytics API (Amplitude/Mixpanel) to fetch real-time behavioral cohorts, funnels, and retention metrics.

For Qualitative Data: It performs a semantic search against your local Vector DB to retrieve relevant segments of user feedback and interview transcripts.

3. **Local & Secure Processing:** All operations run locally. Authentications to third-party analytics platforms use your local API Keys, and qualitative analysis is strictly bounded to your local workspace context. Your raw feedback data and transcripts never leave your machine.

Insight Synthesis: Cursor correlates the behavioral metrics (the What) with the semantic user feedback (the Why), giving you a holistic, conversational breakdown of user insights directly in your editor.
---

## Example Insights & Output Dashboards

