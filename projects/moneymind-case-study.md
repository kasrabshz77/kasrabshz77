# MoneyMind — A Context-Aware AI Finance Copilot

**A collaborative AI and fintech engineering project**

**[Open the Vercel preview](https://money-mind-prod.vercel.app)** · **[Kasra's GitHub](https://github.com/kasrabshz77)**

> **Project status:** Actively developed. The application is not presented here as a publicly launched banking product. The Vercel address comes from the project's deployment documentation; public access and availability should be checked before using it in an application or live presentation.

## Overview

Most finance tools can summarize what a person spent. MoneyMind explores a different question: **Can an AI assistant remember the context behind financial decisions and use it to provide more relevant, accountable guidance?**

MoneyMind brings together transaction data, personal goals, budgets, and user-provided context. Rather than treating every expense as an isolated event, the assistant can recall relevant information and respond with that context in mind.

The product is **built collaboratively**, with the actively developed production code kept in a private repository. This page is a portfolio case study—not a copy, fork, or source-code release.

## What the project demonstrates

| Capability | Engineering focus |
| --- | --- |
| **Context-aware AI** | Agentic workflows that connect user questions to retrieved context and financial tools |
| **Persistent memory** | MongoDB Atlas plus semantic retrieval using vector embeddings |
| **Financial data processing** | PDF/CSV statement ingestion, categorization, and transaction reconciliation |
| **Accounts and budgets** | Structured financial records and authenticated APIs |
| **Reliability** | Validation, automated tests, repeatable evaluations, and clear limitations |
| **Data boundaries** | User-scoped application access and controlled interactions with financial records |

### Example user journey (illustrative, using synthetic data)

1. A user imports sample transactions and asks about recent spending.
2. The assistant identifies relevant spending categories and trends.
3. The user explains that an unusual purchase was for a birthday gift.
4. On a later question, the assistant can retrieve that context and provide advice that accounts for the one-time expense.

The goal is **financial guidance informed by context**, not simply another spending chart.

## High-level architecture

~~~mermaid
flowchart TB
    WEB["React + TypeScript web client"] --> API["FastAPI application backend"]
    API --> DB["MongoDB Atlas: financial data and application state"]
    API --> AGENT["LangGraph + Gemini agent"]
    AGENT --> MEMORY["Context retrieval: Voyage AI embeddings + Atlas Vector Search"]
    MEMORY --> DB
    IMPORT["Statement and sandbox-bank ingestion"] --> API
~~~

This is a simplified **conceptual** diagram for reviewers. It intentionally omits private infrastructure, deployment configuration, internal interfaces, and security-sensitive implementation details.

### Technical stack

- **Languages:** Python, TypeScript, JavaScript
- **Application:** FastAPI, React, Vite
- **AI:** LangGraph, Gemini, Voyage AI embeddings
- **Data and memory:** MongoDB Atlas, Atlas Vector Search
- **Authentication and integrations:** Clerk; Plaid Sandbox in the development workflow
- **Engineering:** Automated tests, GitHub Actions, validation and evaluation workflows

## My engineering contributions — Kasra Bashizadeh

I collaborate with Mahyar on MoneyMind and have contributed across the backend, financial-data layer, and application reliability. Areas of work include:

**1. Backend and API development**  
Building and validating backend functionality that connects authenticated application features with structured financial data.

**2. Data ingestion and consistency**  
Working on financial transaction processing, source reconciliation, and duplicate-handling behavior when imported records overlap with bank-linked transactions.

**3. MongoDB-backed financial features**  
Contributing to account and transaction data flows, including currency-safe calculations and defensible handling of incomplete financial information.

**4. Agent quality and verification**  
Contributing to evaluation, regression testing, and evidence-grounded behavior so that AI outputs can be reviewed against expected tool usage and supplied data.

These are contributions to a **team project**; this case study does not claim sole authorship of the full application.

## Engineering decisions that matter

**Memory should help, not invent.** Contextual recall is intended to inform responses while keeping the distinction between a stored fact, an inference, and unavailable information.

**Financial numbers require care.** Transaction imports, duplicate records, currency boundaries, and missing balance information can change the meaning of a financial answer. Correctness and clear uncertainty are more valuable than a confident but incorrect summary.

**An agent needs boundaries.** The system is designed around authenticated application services, user-scoped financial data, and explicit testing of agent behavior.

**Evaluation is part of development.** Repeatable tests and structured agent evaluations help identify regressions; individual test results should not be mistaken for guaranteed real-world accuracy.

## Why this is relevant to MongoDB and AI-agent hackathons

MoneyMind is an applied example of using MongoDB for both **operational application data** and **retrievable contextual memory**. It brings together structured finance records, semantic search, backend APIs, and agent reasoning in one product.

The interesting problem is not just giving an AI model access to a database. It is **retrieving the right context, preserving data boundaries, and making the resulting answer dependable**.

## Demo

**Vercel preview:** https://money-mind-prod.vercel.app

For a reviewer walkthrough, the team can demonstrate an approved account with **synthetic financial data**:

- Import or review sample transactions.
- Ask the assistant to summarize spending.
- Add relevant context about a one-time purchase or personal goal.
- Ask a follow-up question and inspect how the context affects the answer.

The preview may require authentication and may change while the product is in development. **Do not use real bank statements or personal financial records for a public demonstration.**

## Project context and acknowledgments

MoneyMind is a **collaborative project with Mahyar**. The active production repository is private and is not included in this public portfolio page. Earlier public prototype material, where available, is separate from the current production work.

This case study describes the project at a high level and deliberately includes **no private source code, secrets, private datasets, internal evaluation artifacts, or proprietary deployment configuration**.

---

**Portfolio owner:** [Kasra Bashizadeh](https://github.com/kasrabshz77)  
**Project:** MoneyMind · AI agents · MongoDB Atlas · backend engineering  
**Last reviewed:** November 2026
