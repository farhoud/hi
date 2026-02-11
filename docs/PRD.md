# Product Requirements Document (PRD)

## 1. Product Overview

This product is a **local-first, PRD-driven autonomous AI execution agent** designed for **solo developers and small development teams** with powerful local hardware.  

It orchestrates multiple projects, executes tasks asynchronously, interacts via **OpenClaw-supported channels**, and is structured around **cooperative checkpointing and priority-based scheduling**.  

### Core Philosophy

- Projects are **Git-based**; all artifacts, PRDs, and metadata live in Git.  
- User interaction occurs **through OpenClaw**; users approve outputs and provide clarifications.  
- Execution is **local, isolated, and safe** — VM snapshots for tasks, Docker containers for models.  
- Cooperative pre-emption: tasks checkpoint at logical boundaries; priority governs scheduling.

---

## 2. Target Users

- Senior solo developers  
- Small developer teams (2–10 people)  
- Power users with local GPU hardware  

**User traits:**

- Comfortable with PRDs and structured requirements  
- Expect high control, transparency, and reversibility  
- Understand async task execution  

---

## 3. Core Value Proposition

- Transform PRDs into executable work locally  
- Maximize GPU + memory resource utilization  
- Safely run autonomous tasks with VM isolation and rollback  
- Coordinate multiple tasks across machines  
- Natural interaction through Slack, Telegram, WhatsApp, CLI  

---

## 4. MVP Scope

**Included:**

- PRD creation via Ralph TUI  
- Task execution producing deployable artifacts (code, docs, build folders, Docker images, Git repos with beads/PRD artifacts)  
- Cooperative checkpointing  
- Async / streaming user feedback via OpenClaw  
- Priority-aware scheduling with pre-emption at logical boundaries  

**Out of scope:**

- Cross-machine checkpoint migration  
- Cloud model fallback  
- Enterprise policies  
- Automated strategic product decisions  

---

## 5. Execution Workflow

### Step Lifecycle

1. Project initialized (Git repository, beads, Ralph artifacts)  
2. Task starts execution in micro-VM  
3. Step begins; may request **clarification/input** or produce **checkpoint artifacts**  
4. **Input/clarification events** pushed to OpenClaw; process pauses until user response  
5. Step completes; checkpoint event pushed for user validation  
6. Scheduler evaluates next step based on priority and resources  
7. Task continues until all steps complete  

---

## 6. Priority & Pre-emption

- Processes declare: priority, required model, checkpoint capability  
- Scheduler combines: system rules + project policy + process suggestions  
- Pre-emption occurs only at **checkpoint boundaries**  

---

## 7. Failure Handling

- If a machine dies, task restarts from **step 0**  
- Model state and KV cache are local; no cross-machine migration  
- Scheduler marks failed tasks; user can restart  

---

## 8. Interfaces

- OpenClaw-supported channels: Slack, Telegram, WhatsApp, CLI  
- Users receive:  
  - Checkpoint artifacts for approval  
  - Clarification/input requests  
