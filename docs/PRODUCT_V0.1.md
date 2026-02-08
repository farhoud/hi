# Product Specification: Execution Governor (MVP)

## 1. Vision
A local-first, PRD-driven autonomous execution platform for solo developers. It orchestrates high-VRAM GPU resources to turn specifications into code through hardware-isolated Micro-VMs.

## 2. Core Governance: "No PRD → No Action"
- **Strict Approval:** The agent cannot initiate a VM task without an explicitly approved `PRD.md`.
- **Beads Integration:** Every PRD is compiled into a Beads graph (Epics/Tasks). Ralph-TUI executes only against these validated Beads.

## 3. Product Features
- **Strategy A (Full Autonomy):** Ralph runs the entire epic until completion or error.
- **Human-in-the-Loop:** - Gate 1: PRD Approval.
    - Gate 2: Final Review (Git Diff + Bead Status).
- **Kill Switch:** Immediate `SIGINT` + Snapshot rollback via OpenClaw.

## 4. Success Criteria (MVP v0.1)
- The system must build its own landing page.
- All code changes must be reversible via Git.
