# Cloud Mastery Training Introduction

## Overview
Build and deploy SokoAI — a real, production-grade AI-native commerce platform — in a single day using Google Cloud. Three sessions, one complete system: from zero to a deployed app with a conversational agent and ML-powered recommendations.

## Detailed Session Breakdown

### Session 1 — Serverless & Secure Foundation
Zero to a live, deployed application: Next.js storefront + NestJS API on Cloud Run, backed by Cloud SQL.

**Cloud Run Deployment**

- Clone the SokoAI starter repo (pre-seeded GitHub repository provided)
- Containerise with Docker — walkthrough of the Dockerfile
- Deploy Next.js frontend and NestJS API to Cloud Run as separate services
- Configure environment variables, secrets, and Cloud Run service settings

**Keyless Authentication**

- Why service account keys are dangerous — the "keys under the mat" problem
- Workload Identity Federation — concept and hands-on configuration
- Connect NestJS API to Cloud SQL via Cloud SQL Auth Proxy — no stored credentials
- Test the connection: NestJS API returns SokoAI product data from Cloud SQL

---

### Session 2 — The Agent Brain
Build the SokoAI multi-agent system from scratch in Vertex AI Agent Builder and wire it into the frontend.

**Agent Architecture**

- Vertex AI Agent Builder overview — agents, tools, data stores, playbooks
- One orchestrator → three specialist sub-agents
- Persona, tone, and scope: Kenyan English, grounded only in SokoAI data

**Building the Three Sub-Agents**

- **Soko Shop Agent** — products + inventory
  Tools: `search_products`, `add_to_cart`, `check_delivery_eta`
- **Soko Parts Agent** — vehicle-to-battery compatibility matrix
  Tools: `match_part`, `check_stock`, `find_nearest_location`
- **Soko Wealth Agent** — financial products + rule engine
  Tools: `get_products`, `build_recommendation`, `calculate_projection`

**Frontend Integration**

- Call the Vertex AI Agent API from Next.js (server action or API route)
- Stream agent responses into the chat UI
- Live test: send a message and watch SokoAI respond from your own agent


---

### Session 3 — Data Intelligence
Every user action becomes a training signal. This session shows how an AI-native app gets smarter from its own usage.

**The Data Pipeline**

- Cloud Functions triggered by Cloud SQL events → BigQuery
- Event schema:
  - `session_id`, `user_id`, `query_text`, `module`
  - `product_viewed`, `cart_action`, `order_status`
- Walk through the pre-built pipeline — observe, don't configure from scratch

**BigQuery ML Models**

- K-Means clustering: segment users by behaviour (high-value, price-sensitive, parts-focused, wealth-seeking)
- Classification model: predict browse-to-buy conversion probability
- All in SQL — no Python, no ML framework

**Closing the Loop**

- BQML outputs feed back into agent recommendations (e.g. high-value segment → premium MMF products first)
- Looker Studio dashboard: agent activity, user segments, and conversion predictions on one screen

**Grand Finale**

The full SokoAI system live — deployed app, conversational agent, ML-powered recommendations. The room built this today.


---

## What to Bring
- Laptop with Docker installed (Cloud Shell fallback available)
- A Google Cloud account — project IDs and credentials provided on the day
- A browser with access to [Google Cloud Console](https://console.cloud.google.com)
- Curiosity and an appetite to build something real

<div class="page-nav">
  <div class="nav-item">
    <a href="../" class="btn-secondary">← Previous: Home</a>
  </div>
  <div class="nav-item">
    <span>Introduction</span>
  </div>
  <div class="nav-item">
    <a href="/devops/devops-lab" class="btn-primary">Next: Automate Securely →</a>
  </div>
</div>