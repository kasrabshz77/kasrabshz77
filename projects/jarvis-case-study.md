# JARVIS — Explainable Task Reasoning System

**A full-stack project exploring inspectable symbolic reasoning, dependable APIs, and human-readable explanations**

**[Kasra's GitHub profile](https://github.com/kasrabshz77)** · **Repository: private** · **Project status: Phase 2 reasoning functionality implemented and integrated (November 2026)**

> **What works today:** Persistent task management, dependency editing, a visual dependency graph, a bounded SWI-Prolog reasoning engine, and a **Why?** interface that explains readiness and blockers.  
> **Not yet implemented:** Conversational LLM chat, speech recognition and voice output, automated scheduling, hosted deployment, and calendar integration. This is a public portfolio case study, **not** a public release of the private source code.

## The idea

A normal task manager can tell you whether a task is checked off. **JARVIS aims to tell you why a task cannot start yet—and what would have to change to unblock it.**

The current application models task prerequisites as a graph, evaluates their relationships through a constrained symbolic reasoner, and presents explanations grounded in the saved tasks and dependency edges. Rather than outputting an unsupported "Ready" or "Blocked" label, the application can identify direct and underlying blockers and display the evidence path.

## A working example

Imagine three tasks:

- **A:** Deliver a project
- **B:** Finish integration
- **C:** Validate the underlying component

A requires B, and B requires C.

~~~mermaid
flowchart LR
    A["A: Deliver project"] -->|"requires"| B["B: Finish integration"]
    B -->|"requires"| C["C: Validate component"]
~~~

When none are completed, JARVIS can explain that **A is blocked by B**, with **C as an underlying/root blocker**, and show the dependency path `A → B → C`.

As the user completes **C**, **B** becomes ready while **A** remains blocked. Once **B** is completed, **A** becomes ready. If the user tries to add a dependency that introduces a cycle, the application rejects the edit rather than saving a contradictory graph.

The same flow has been exercised through the actual browser, API, native reasoner, and database using disposable test data.

## What I've implemented

| Capability | What it demonstrates |
| --- | --- |
| **Persistent task management** | Task creation and updates backed by SQLite, with owner-scoped application boundaries |
| **Dependency editing** | Safely add and change prerequisites; reject cycles and invalid graph updates |
| **Symbolic reasoning** | SWI-Prolog evaluates validated dependency snapshots through a supervised Python/Janus boundary |
| **Explainable results** | Direct, intermediate, transitive, and root blockers presented with specific evidence paths |
| **Visual graph and Why interface** | Browser UI for understanding dependencies, highlighting paths, and reading explanations |
| **Freshness and failure handling** | Snapshot/version checks and explicit failed or truncated states instead of false readiness |
| **Automated verification** | Independent graph oracles, negative tests, real-browser integration, and CI checks |

## High-level architecture

~~~mermaid
flowchart TB
    UI["React + TypeScript interface"] --> API["FastAPI application and validated task API"]
    API <--> DB[("SQLite: tasks, dependency edges, versions")]
    API --> ADAPTER["Bounded reasoning adapter (Python / Janus)"]
    ADAPTER --> LOGIC["SWI-Prolog: blocker rules and evidence paths"]
    LOGIC --> ADAPTER
    ADAPTER --> API
    API --> UI
~~~

**Separation of responsibility matters:**

- **SQLite** stores the authoritative facts. It is not replaced by a mutable Prolog knowledge base.
- **Python/FastAPI** manages validation, ownership, persistence, API responses, and execution boundaries.
- **SWI-Prolog** performs a narrow, read-only reasoning task over a validated snapshot, deriving blockers and proof-shaped paths.
- **React/TypeScript** displays the actual returned status, evidence, and limitations.

This choice is intentional: Python could also evaluate the graph, but Prolog provides a compact way to express recursive relationships and derive evidence paths. Independent Python implementations are used as test oracles to challenge the Prolog results.

## Reliability is part of the feature

JARVIS is designed to avoid an especially harmful outcome: **incorrectly declaring a blocked task ready.** The current engineering checks cover:

1. **Cycles:** invalid dependency changes are rejected and leave the graph unchanged.
2. **Bounded execution:** limits on graph size, recursion, output, queue capacity, and runtime; incomplete work is labelled accordingly.
3. **Native failures:** a crashed reasoning worker surfaces a controlled error and can recover on a subsequent request.
4. **Data freshness:** edits invalidate stale reasoning so an old explanation is not displayed as current.
5. **Independent verification:** expected results come from reviewed truth tables and a separate graph oracle, not solely from the implementation being tested.

### Recorded integration evidence

The Phase 2 integrated browser audit exercises a real `A → B → C` journey: create tasks, inspect exact blocker paths, reject a cycle, handle a native crash and truncation, complete prerequisites, reload, and verify persisted SQLite state.

The recorded Phase 2 audit reports **224 Python tests plus 3 pytest subtests and 179 frontend tests** passing in the reviewed local verification. The merged integration changes also passed the repository's locked GitHub Actions workflow on **November 2, 2026**.

Those checks establish the tested behaviors within their defined environments; they are **not** a claim of mathematically proved correctness, hosted production readiness, or completed voice/LLM functionality. The detailed evidence remains in the private project repository.

## Tech stack

| Layer | Technology |
| --- | --- |
| Frontend | React, TypeScript, Vite |
| Backend | Python, FastAPI |
| Persistence | SQLite |
| Reasoning | SWI-Prolog, Janus |
| Quality | pytest, frontend tests, real Chromium browser audits, GitHub Actions |
| Architecture | Typed API contracts, bounded native workers, versioned dependency snapshots |

## Development approach

The project is built in small, reviewed increments. Each increment is expected to show not just a working happy path, but also negative cases, failure boundaries, and a meaningful cross-layer test where appropriate.

I value features that can **explain their own outputs**, fail safely when evidence is insufficient, and keep the stored system of record separate from the logic used to interpret it.

## Demo walkthrough (recording planned)

A short, synthetic-data walkthrough would show:

1. Creating tasks A, B, and C.
2. Drawing the dependency chain and opening the visual graph.
3. Selecting A and opening **Why?** to see blockers and evidence.
4. Attempting a cyclic dependency and seeing it rejected.
5. Completing C and B in order, then watching A become ready.

**A publicly accessible demo video or hosted instance has not yet been published.** No real personal tasks, credentials, or private project materials are needed for the demonstration.

## Current scope and what's next

**Implemented:** Task CRUD, dependency management, symbolic graph reasoning, explainability, bounded execution, and integrated UI/backend/database tests.

**Future roadmap:** Conversational assistant capabilities, natural-language operations with approvals, scheduling, managed memory, voice interaction, and integrations. These are **planned**, not presented as already working.

## About the developer

I'm **Kasra Bashizadeh**, a Computer Science student at Toronto Metropolitan University. I built JARVIS to explore how formal reasoning, readable evidence, and pragmatic full-stack engineering can make intelligent software more dependable.

**[Visit my GitHub profile](https://github.com/kasrabshz77)** · **[See my MoneyMind case study](moneymind-case-study.md)**

---

*Public portfolio write-up · November 2026. The working repository remains private. This page contains no private source code, secrets, internal test data, or unpublished implementation files.*
