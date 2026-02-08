# System Architecture

## 1. Component Topology
The system uses a **Split-Plane** approach to solve the GPU-in-VM isolation problem.

### A. Control Plane (Host)
- **Orchestrator:** Python/FastAPI service managing `machines.yaml`.
- **Interface:** OpenClaw (Slack/Telegram) bridge.
- **Repository:** The Git Host containing the code and Beads database (`.beads/`).

### B. Inference Plane (Docker)
- **Container:** vLLM or Ollama with GPU pass-through (`--gpus all`).
- **Endpoint:** Exposed at `http://172.17.0.1:11434` for internal bridge access.

### C. Execution Plane (Firecracker Micro-VM)
- **Isolation:** Stateless Micro-VM with no GPU access.
- **Runtime:** `ralph-tui` + `bd` (Beads CLI).
- **Proxy:** A gRPC agent (Go/Rust) listening for orchestrator commands.

## 2. Data Handoff (Git-Native)
1. **Host:** Commits PRD/Beads to a task branch `task/feature-x`.
2. **Orchestrator:** Sends Git URL/Branch to VM via gRPC.
3. **VM:** Clones branch, runs `ralph-tui run`, and pushes updates back to Host.