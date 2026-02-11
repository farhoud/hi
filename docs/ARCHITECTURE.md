# Architecture Overview

## 1. System Components

### 1.1 Central Execution Control Plane

- Ephemeral runtime authority  
- Tracks:
  - Process definitions
  - Step index
  - Priority and model requirements
  - Pause/resume state
  - Machine assignment
- Responsible for scheduling and pre-emption decisions

### 1.2 Worker Nodes

- Each machine holds:
  - Micro-VM snapshots
  - Local artifacts
  - Model runtime (Docker container)
  - KV cache (optional, preserved per step)
- Executes steps as isolated processes
- Reports checkpoint events and input requests to control plane / OpenClaw

### 1.3 Projects

- Git repositories (single source of truth)
- Contain:
  - Source code
  - PRDs
  - Beads metadata
  - Ralph artifacts
  - Build outputs (optional)
  - Execution manifests

---

## 2. Execution Model

- Processes are **step-based**:

```

Step → Inference / Task → Checkpoint → Optional Input → Next Step

```

- Checkpointing occurs at logical step boundaries  
- Input/clarification requests are mandatory steps mid-process  
- Pre-emption is **cooperative** and occurs **at checkpoint boundaries only**  

---

## 3. Priority & Scheduling

- Processes declare:
  - Priority
  - Required model
  - Checkpoint capability
- Scheduler evaluates:
  - System rules (non-overridable)
  - Project-level policies
  - Process suggestions
- Scheduler enforces **Option E policy** (combination authority)
- Long tasks are segmented to allow cooperative pre-emption

---

## 4. Isolation & Safety

- Tasks run in **micro-VMs**  
- Models run in **Docker containers**  
- VM snapshots and local artifacts prevent corruption and allow rollback  
- If a worker dies → task restarts from the beginning  

---

## 5. Async / Streaming UX

1. **Checkpoint Events:**  
   - Step completes → artifacts/PRDs pushed to OpenClaw  
   - User approves step before continuing  

2. **Clarification / Input Events:**  
   - Step requires user decision → event pushed to OpenClaw  
   - Task paused until response is received  

- Both types of events allow the user to guide execution  
- OpenClaw channels provide **single interface for async interactions**

---

## 6. Model Fabric

- Centralized model scheduler:
  - Allocates models to processes
  - Evaluates priority & resource usage
  - Supports cooperative pre-emption
  - Logical checkpoints only (Option B)  
- Prevents GPU contention and allows multi-machine orchestration  

---

## 7. Multi-Machine Orchestration

- Automatic task distribution across machines  
- Constraints:
  - GPU availability
  - Memory availability
- Multiple tasks per machine supported  
- Worker-local VM snapshots enable independent execution

---

## 8. Failure Handling

- Infra / system / resource / network errors → retry up to 5 times  
- Other errors → stop execution and escalate to user  
- Machine crash → task restarts from step 0

---

## 9. MVP Goals

- Git-based projects with all PRDs, beads, and artifacts versioned  
- Stable parallel execution on local hardware  
- Async UX for checkpoints and clarifications  
- Deployable artifacts generated  
- Transparent failure handling

---

