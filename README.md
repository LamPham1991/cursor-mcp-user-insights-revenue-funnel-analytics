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

<img width="1594" height="692" alt="image" src="https://github.com/user-attachments/assets/43cf9b99-edbf-4900-9dca-0dcf1432ccfd" />

### How it works:
1. **Ask in Natural Language:** You ask Cursor a question about funnel drop-offs, or revenue correlation in the Chat sidebar (Ctrl/Cmd + L) or Composer.
2. **AI Translates & Executes:** Cursor translates it into an optimized SQL query reflecting your database schema via the MCP BigQuery tool.
3. **Local & Secure Run:** The query runs against your BigQuery project using your local `gcloud` credentials. **No data leaves your machine.**
4. **Insight Delivery:** Results are returned to Cursor, which interprets the trends, identifies friction points, and presents them conversationally.

# Qualitative & Quantitative User Insights

<img width="466" height="498" alt="Screenshot 2026-05-19 at 16 37 50" src="https://github.com/user-attachments/assets/9bce6a6e-48a1-4299-ac48-443e709eced6" />

### How it works:
1. **Ask in Natural Language:** You ask Cursor a question about user product behavior (e.g., "What is the drop-off rate at the KYC step?") or user feedback (e.g., "What are the top 3 user complaints about our new feature?") in the Chat sidebar or Composer.

2. **AI Intent Routing & Execution:** Cursor analyzes your request and routes it dynamically via the mcp-server-user-insight tool:

For Quantitative Data: It calls the Analytics API (Amplitude/Mixpanel) to fetch real-time behavioral cohorts, funnels, and retention metrics.

For Qualitative Data: It performs a semantic search against your local Vector DB to retrieve relevant segments of user feedback and interview transcripts.

3. **Local & Secure Processing:** All operations run locally. Authentications to third-party analytics platforms use your local API Keys, and qualitative analysis is strictly bounded to your local workspace context. Your raw feedback data and transcripts never leave your machine.

Insight Synthesis: Cursor correlates the behavioral metrics (the What) with the semantic user feedback (the Why), giving you a holistic, conversational breakdown of user insights directly in your editor.

---

## Example Insights & Output Dashboards

**Core Feature Engagement (Quantitative)**

*"What is the engagement depth of our new Gamification feature by monthly cohort?"*

<img width="982" height="518" alt="Screenshot 2026-05-19 at 17 09 50" src="https://github.com/user-attachments/assets/854f5be2-b036-4ef2-9915-f902f7b287ca" />

**Qualitative Feedback & Interview Synthesis**

*"What are the most frequent user complaints about the Gamification UI, and what examples can you synthesize from recent interviews?"*

<img width="1024" height="931" alt="image" src="https://github.com/user-attachments/assets/a6d15404-d1d8-4216-aba0-d065ba515968" />


**Conversion Dashboard**

*"Show me the new user onboarding-to-paid conversion rate, broken down by monthly cohorts for the last 6 months."*

<img width="706" height="337" alt="Screenshot 2026-05-19 at 16 45 02" src="https://github.com/user-attachments/assets/5e3381d7-7f9e-4f06-80dd-f5d18bfc299c" />

**Lead & Revenue Interaction**

*"How do lead volume and revenue correlate across our different segments?"*

<img width="1024" height="629" alt="image" src="https://github.com/user-attachments/assets/b986cecd-053d-439a-abd1-4121cb15bbcc" />

---

## How to Set Up

**Prerequisites**

- Cursor IDE installed.
- A Google Cloud project with BigQuery enabled (for Revenue & Quant data).
- Python 3.10+ and uv.
- API Key for your Analytics platform (e.g., Amplitude/Mixpanel).

**Step 1 — Configure Cursor MCP**
Open the Cursor settings, navigate to Features > MCP, and click Add New MCP Server.
Paste the following configuration:
JSON
{
  "mcpServers": {
    "bigquery-revenue": {
      "command": "uvx",
      "args": ["mcp-server-bigquery"],
      "env": {
        "BIGQUERY_PROJECT": "your-gcp-project-id",
        "BIGQUERY_LOCATION": "us-central1"
      }
    },
    "user-insights": {
      "command": "node",
      "args": ["/path/to/your/mcp-server-user-insight/index.js"],
      "env": {
        "AMPLITUDE_API_KEY": "your_key",
        "AMPLITUDE_SECRET": "your_secret",
        "LOCAL_VECTOR_DB_PATH": "./data/vector_store"
      }
    }
  }
}

**Step 2 — Authenticate Locally**
For BigQuery: Run gcloud auth application-default login in your terminal. This allows the server to query data using your local credentials without needing service account JSON files.
For Insights: Ensure your .env file (if using node server) contains valid API keys for your behavioral analytics provider.

**Step 3 — Initialize Local Context**
Place your qualitative data (interview transcripts in .md, .txt, or .pdf) into the ./data/transcripts folder. The user-insights server will automatically index these for semantic search.

**Step 4 — Verify Connection**
Restart Cursor. In the Chat sidebar or Composer (Ctrl/Cmd + L), look for the MCP icon (a plug or hammer symbol). You should see bigquery-revenue and user-insights active.
Start Asking Questions
*Revenue Funnel:*

"Show me the new user onboarding-to-paid conversion rate, broken down by monthly cohorts for the last 6 months."

"What's the drop-off rate at each step of our onboarding funnel?"

*Quantitative Behavioral Insights:*

"Which core features are most utilized by users who have a high PQL score?"

"Compare the 30-day retention rate between users from organic channels vs. paid ads."

*Qualitative Feedback:*

"Synthesize the most common friction points regarding our 'Dashboard' UI from recent user interviews."

"Based on feedback, why are users finding it difficult to complete the setup process?"
