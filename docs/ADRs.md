# Architecture Decision Records (ADRs)

## ADR-001: Project Storage
- **Decision:** Projects are Git repositories, all files managed in Git  
- **Status:** Approved  
- **Context:** Keeps projects versioned, auditable, and local-first  
- **Consequences:** No DB required for project data; central execution plane stores ephemeral state only  

## ADR-002: Execution Control Plane
- **Decision:** Central ephemeral control plane tracks runtime state  
- **Status:** Approved  
- **Context:** Centralized scheduling, priority evaluation, and pre-emption coordination  
- **Consequences:** Failure of control plane does not corrupt project; workers hold heavy state  

## ADR-003: Task Isolation
- **Decision:** Each task runs in a micro-VM, model inference in Docker  
- **Status:** Approved  
- **Context:** Protects filesystem, GPU memory, and allows snapshot rollback  
- **Consequences:** VM snapshots required for checkpointing; cross-machine migration not supported  

## ADR-004: Checkpointing
- **Decision:** Checkpoints occur at logical step boundaries (Option B)  
- **Status:** Approved  
- **Context:** Mid-inference interruption is infeasible; step-level checkpoints are manageable  
- **Consequences:** Tasks must be segmented; cooperative pre-emption only  

## ADR-005: Priority & Pre-emption
- **Decision:** Processes declare priority; scheduler combines system rules, project policy, and process suggestions (Option E)  
- **Status:** Approved  
- **Context:** Ensures fairness, system safety, and project-level governance  
- **Consequences:** Pre-emption occurs at checkpoints; long tasks must be segmented  

## ADR-006: Failure Handling
- **Decision:** Worker machine dies → task restarts from beginning (Option A)  
- **Status:** Approved  
- **Context:** Preserving KV cache and VM snapshots across machines is too heavy for MVP  
- **Consequences:** Tasks must be idempotent or restartable; system simpler and more predictable  

## ADR-007: Async / Streaming UX
- **Decision:** Two event types via OpenClaw: Checkpoints & Input/Clarifications  
- **Status:** Approved  
- **Context:** Users approve artifacts and provide input when required  
- **Consequences:** Task execution is paused for input; cooperative pre-emption respected  

## ADR-008: Model Fabric
- **Decision:** Centralized model scheduler handles inference allocation, priority, and resource arbitration  
- **Status:** Approved  
- **Context:** Prevents GPU contention, allows multi-machine orchestration  
- **Consequences:** All model calls go through Fabric; cooperative checkpointing ensures pre-emption


