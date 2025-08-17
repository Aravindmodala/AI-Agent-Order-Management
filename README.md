# AI Agent for Order Management

> Case study repo documenting architecture, workflow, and outcomes.  
> Note: The full source code lives in a proprietary environment; this repo shares design, pseudo-code, and results and can be walked through in interviews.

## Overview
This project demonstrates how Generative AI + automation streamline wholesale order management.  
The agent ingests **voice/text (EN + HI)**, extracts structured orders, validates intent, retrieves product knowledge, automates entry to Google Sheets/ERP, and returns confirmations.

## Highlights
- **LLMs & Agents:** OpenAI/Claude via **LangChain**, multi-agent orchestration with **CrewAI/LangGraph** (Planner → Executor → Verifier)
- **Retrieval:** **RAG** with FAISS/Pinecone; invoice & catalog context
- **Automation:** **n8n** for Google Sheets / notifications / webhooks
- **APIs:** **FastAPI** gateway; Redis/Kafka events
- **Obs & Eval:** **MLflow** for metrics; SHAP for explainability on classifier components
- **Media (optional):** Midjourney / DALL·E for catalog visuals

## Outcomes
- **95% intent accuracy** on bilingual (EN/HI) orders (measured on real flows)
- **70% reduction** in manual order entry effort
- **2.5× faster** response time vs. manual process

## Architecture
See `ai_agent_architecture_wireframe.png`.

```mermaid
flowchart LR
  A[User Input<br/>Voice/Text EN+HI] -->|REST| B[FastAPI Gateway]
  B -->|events| C[Event Bus<br/>Kafka/Redis]
  A --> D[NLU/Extraction<br/>OpenAI/Claude via LangChain]
  B --> E[Agent Orchestration<br/>CrewAI/LangGraph<br/>Planner→Executor→Verifier]
  C --> F[Vector Store + RAG<br/>FAISS/Pinecone]
  D --> E --> F
  E --> G[Automation<br/>n8n: Sheets/Notifications]
  E --> H[Warehouse<br/>Snowflake/BigQuery]
  E --> I[Observability<br/>MLflow + Logs]
  H --> J[Media Gen (Optional)<br/>Midjourney/DALL·E]
