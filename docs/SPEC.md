# Product Specification

## 1. Product Overview

This product is a **local‑first, PRD‑driven autonomous AI execution agent** designed for **solo developers and small development teams** who own powerful local hardware.

The agent orchestrates **multiple projects across multiple machines**, runs **local AI models**, executes tasks **in parallel**, and interacts with users through **OpenClaw-supported channels** (Slack, Telegram, WhatsApp, CLI).

The system is explicitly **not a general AI assistant**. It is an **execution platform** with strict governance:

> **No PRD → No Action**

The user is always the final authority.

---

## 2. Target Users

### Primary Users
- Senior solo developers
- Small developer teams (2–10 people)
- Power users with local GPU hardware

### User Characteristics
- Comfortable with PRDs and structured requirements
- Pragmatic expectations of AI
- Strong preference for control, transparency, and reversibility

---

## 3. Core Value Proposition

- Turn **PRDs into executable work** using local AI models
- Maximize utilization of **local GPU + memory resources**
- Safely run autonomous tasks with **VM sandboxing and rollback**
- Coordinate work across **multiple machines and projects**
- Interact naturally via **existing communication tools**

---

## 4. Non‑Goals (Explicit Boundaries)

The product will **not**:
- Act outside an approved PRD
- Invent new projects or cross project boundaries
- Auto‑commit, auto‑deploy, or auto‑publish without user approval
- Provide strategic product decisions
- Replace developer judgment

---

## 5. PRD‑Driven Workflow

### 5.1 PRD Lifecycle
1. User initiates a task within a project
2. Agent asks **only necessary clarification questions**
3. Agent produces a structured PRD
4. User reviews and **explicitly approves** the PRD
5. Agent executes tasks derived from the PRD
6. Results are returned to the user

### 5.2 PRD as Contract
- PRD defines the **execution boundary**
- All actions must trace back to PRD items
- PRD approval is mandatory before execution

---

## 6. Execution Capabilities (MVP Scope)

### Included
- Anything **Ralph TUI** can produce
- Code generation and reasoning tasks
- Project scaffolding
- Documentation and static sites
- Deployable artifacts

### Outputs
- Docker images
- Build folders / ZIPs
- Git repositories with Beads / PRD artifacts

---

## 7. AI Model Execution

### Model Types
- Code generation models
- Reasoning / general LLMs

### Execution Mode
- Fully local inference
- Target hardware: ~60 GB VRAM (GPU + shared memory)
- Parallel task execution

---

## 8. Multi‑Machine Orchestration

### Scheduling
- Automatic task distribution
- Primary constraints:
  - GPU availability
  - Memory availability

### Parallelism
- Multiple tasks per machine
- Multiple machines per project

---

## 9. Safety & Isolation

### Sandboxing
- Each task runs inside a **virtual machine**

### Recovery
- VM snapshot before execution
- Snapshot restore on failure

### Failure Handling
- Infra/system/resource/network errors:
  - Automatic retry
  - Retry limit: **5 attempts**
  - Reschedule on another machine if needed
- Other errors:
  - Stop execution
  - Escalate to user

---

## 10. Reporting & Feedback

### Task Signals (MVP)
- Completion
- Error

### User Authority
- User decides whether output is acceptable
- No automatic continuation without user consent

---

## 11. Suggestions System

### Scope
- Limited strictly to **current project/task context**

### Nature
- Small, optional follow‑ups only
- Examples:
  - Add tests
  - Improve docs
  - Refactor module

### Constraints
- Never auto‑queued
- Never cross‑project

---

## 12. Interfaces

### Primary Interface (MVP)
- OpenClaw
  - Slack
  - Telegram
  - WhatsApp
  - CLI

---

## 13. MVP Definition

### MVP v0.1 Success Criteria
- Agent builds its **own landing page and documentation app**
- Output is a **deployable artifact**
- Execution is stable under parallel load
- Failures are recoverable and transparent

---

## 14. Future Extensions (Out of MVP Scope)

- Advanced task prioritization
- Cross‑project suggestions
- Cloud model fallback
- Fine‑grained metrics
- Enterprise policy layers

---

## 15. Product Philosophy

- PRD over prompts
- Autonomy with hard boundaries
- Local‑first
- Safety before intelligence
- Human always in control
