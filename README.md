# Informatica Cloud Automation & AI Agent POC Suite

This repository is a collection of **Unix shell script automations** , **Informatica Cloud (IDMC) POCs**, combined with **Google Vertex AI ADK agents**.

The focus areas are:

- **Unix shell automation tightly coupled with Informatica Cloud**
- **Cloud Data Integration (CDI) & Cloud Application Integration (CAI) orchestration**
- **Google Vertex AI ADK agents** for:
  - Change plan validation (ServiceNow)
  - Change implementation plan generation
  - Flowchart generation from PDFs

---

## Tech Stack

- **Informatica Intelligent Cloud Services (IDMC)**
  - Cloud Data Integration (CDI)
  - Cloud Application Integration (CAI)
- **Unix / Linux**
  - Bash shell scripts for orchestration, file handling, and migration
- **Messaging**
  - JMS queues (publish from CAI)
- **Security & Auth**
  - JWT-based authentication
  - Informatica session ID generation
- **AI & Apps**
  - Google Vertex AI ADK (agents)
  - Streamlit (web UI)
  - Claire GPT (for mapping generation)
- **Service Management**
  - ServiceNow (change plan validation)

---

## Key Proof-of-Concepts

### 1. Dynamic Taskflow Trigger (File-Driven CDI + CAI)

**Goal:** Automatically trigger an Informatica **taskflow** when a file arrives in a specific location.

- Uses **Cloud Data Integration (CDI)** to detect file availability.
- Uses **CAI** to orchestrate the trigger logic.
- Can be combined with a shell wrapper to:
  - Monitor file systems or SFTP folders.
  - Call CAI processes / IICS APIs based on file presence.
- Supports:
  - Parameterised taskflows.
  - Environment-aware execution (`dev`, `test`, `prod`).

---

### 2. JMS Queue Integration with CAI

**Goal:** Demonstrate integration between Informatica **Cloud Application Integration** and **JMS queues**.

- CAI process publishes messages to JMS queues.
- Can be used for:
  - Event-driven architectures.
  - Decoupling Informatica jobs from downstream consumers.

---

### 3. JWT & Session ID Helper for Informatica APIs

**Goal:** Simplify secure API access to Informatica Cloud using Informatica Guide.

- **Script / guide** to:
  - Generate a **JWT token**.
  - Exchange it for an **Informatica session ID**.
- Designed as:
  - A re-usable reference for API-based automations.
  - A building block for shell scripts and CAI processes.

---

### 4. Multi-Structure File Consumption in a Single CAI Process

**Goal:** Handle **multi-structure files** (e.g. different record layouts/segments) with a single CAI process.

- One CAI process can:
  - Parse and route multiple structures.
  - Apply different transformation logic per structure.
- Useful for:
  - Complex file ingestion.
  - Mixed record types (header/detail/trailer) within one file.

---

### 5. End-to-End Mapping Validation Automation (JSON-Based)

**Goal:** Validate Informatica mappings **end-to-end** using JSON definitions.

- Automation uses JSON to:
  - Describe expected source–target mappings.
  - Verify if actual mappings and flows align to the design.
- Can be driven by:
  - Shell scripts.
  - IDMC APIs.
---

### 6. File System Sync Script for Data Migration

**Goal:** Provide a general-purpose script to **synchronise two different file systems**, suitable for **data migration**.

- Unix shell script to:
  - Compare source and target file systems.
  - Sync new/changed files using rsycn.
  - Log activity and differences.
- Can be integrated with:
  - Pre-migration checks for CDI.
  - Post-migration validation tasks.

---

### 7. Taskflow Restartability from Failed Point

**Goal:** Enable **restartability** of taskflows from the **failed step** instead of re-running everything.

- Concept / implementation pattern:
  - Track taskflow step status.
  - Store checkpoints / last completed step.
  - Restart only from the **failed point**.
- Helps reduce:
  - Reprocessing time.
  - Risk of duplicate runs.

---

### 8. Mapping Generation with Claire GPT

**Goal:** Use **Claire GPT** to assist in **mapping design**.

- Generate draft Informatica mapping logic based on:
  - High-level requirements.
  - Source/target metadata.
- Combined with:
  - AI agents for **validation**.
  - JSON-based mapping specs for downstream automation.

---

### 9. ServiceNow Change Plan Validator Agent (Vertex AI ADK + Streamlit)

**Goal:** Validate change plans in **ServiceNow** using a **Google Vertex AI ADK agent**, wrapped in a **Streamlit** UI.

- Agent capabilities:
  - Read/ingest change plan details (e.g. from JSON/PDF/text).
  - Validate if required components exist:
    - Pre-checks
    - Implementation steps
    - Validation / rollback steps
  - Highlight gaps or risks.

---

### 10. Change Implementation Plan Generator Agent

**Goal:** Automatically **generate change implementation plans** using Google ADK agent logic.

- Inputs:
  - System name.
  - Change window.
  - Risk level / impact.
- Outputs:
  - Structured implementation plan:
    - Steps
    - Checkpoints
    - Validation tasks
    - Rollback strategy
---

### 11. Flowchart Diagram Generator from PDF

**Goal:** Generate **flowchart diagrams** from uploaded **PDFs**.

- Agent pipeline can:
  - Extract process steps from PDF documents.
  - Convert them into a logical flow.
  - Generate a **flowchart representation**
- Useful for:
  - Visualising existing runbooks.
  - Reverse-engineering old documentation.

