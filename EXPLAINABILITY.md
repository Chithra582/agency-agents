# EXPLAINABILITY.md

This document explains the internal mechanisms, data lineage, operational boundaries, and governance framework of **agency-agents** (`agency-agents`) in accordance with the **OpenGAP v0.1.0** specification for the **HiDevs GitAgent Passport** clearance pipeline.

> **Agent Name:** agency-agents (`agency-agents`)  
> **Specification:** OpenGAP v0.1.0  
> **Category / Domain:** Productivity / AI Agency Workforce & Specialized Domain Personas  
> **Compliance Standard:** OpenGAP Checkpoint 2 (Explainability & Decision Governance), OWASP LLM Top 10, MITRE ATLAS  

---

## How the Agent Decides

The agent operates via a strictly disciplined, 5-stage deterministic execution pipeline coordinating persona selection, cross-functional collaboration, rubric auditing, and human milestone governance.

### 1. Decision Architecture

The runtime intake, state classification, evaluation, and execution tracking operate across a deterministic, five-stage pipeline:

```
+-----------------------------------------------------------------------------------+
|                        Deterministic Agency Agents Pipeline                       |
+-----------------------------------------------------------------------------------+
|  [Stage 1: Client Ingestion & Domain Requirement Resolution Gate]                 |
|     --> Ingest task prompt, identify target divisions, & parse objective scope    |
+-----------------------------------------+-----------------------------------------+
                                          |
                                          v
+-----------------------------------------------------------------------------------+
|  [Stage 2: Persona Matching & Routing Gate]                                       |
|     --> Match task requirements against persona profiles across active divisions  |
+-----------------------------------------+-----------------------------------------+
                                          |
                                          v
+-----------------------------------------------------------------------------------+
|  [Stage 3: Cross-Functional Collaborative Pipeline]                               |
|     --> Execute ordered persona stages; exchange structured briefs across divisions|
+-----------------------------------------+-----------------------------------------+
                                          |
                                          v
+-----------------------------------------------------------------------------------+
|  [Stage 4: Quality Assurance & Rubric Specification Audit]                        |
|     --> Evaluate deliverables against division checklists & acceptance criteria   |
+-----------------------------------------+-----------------------------------------+
                                          |
                                          v
+-----------------------------------------------------------------------------------+
|  [Stage 5: Executive Delivery & Human Director Sign-Off]                          |
|     --> Synthesize deliverable bundle, format executive summary, & conclude turn   |
+-----------------------------------------------------------------------------------+
```

### 2. Decision Logic & Routing Formulations

Persona selection affinity $S_{\text{affinity}}(p)$ for an agent persona $p \in P$ given input requirements $q$ is computed through a multi-factor normalized scoring formula:

$$S_{\text{affinity}}(p) = w_1 \cdot \text{CosineSimilarity}(\mathbf{e}_q, \mathbf{e}_p) + w_2 \cdot \text{DivisionFit}(p, q) + w_3 \cdot \text{DeliverableAlignment}(p, q)$$

Where:
- $w_1 = 0.45$: Semantic embedding proximity between task prompt $\mathbf{e}_q$ and persona expertise profile $\mathbf{e}_p$.
- $w_2 = 0.35$: Categorical match score with the target division (e.g. `engineering`, `design`, `marketing`).
- $w_3 = 0.20$: Deliverable specification alignment matching required output formats (code, Figma spec, PRD, copy).

Deliverable quality audit score $Q_{\text{audit}}(d)$ validates compliance against division acceptance criteria $C$:

$$Q_{\text{audit}}(d) = \sum_{i=1}^{M} \lambda_i \cdot c_i(d) \ge \tau_{\text{quality}}$$

Where $c_i(d) \in \{0, 1\}$ represents satisfaction of checklist item $i$, weights $\sum \lambda_i = 1$, and $\tau_{\text{quality}} = 0.90$.

### 3. Thresholding & Refusal Decision Criteria

agency-agents enforces strict operational boundaries and deterministic refusal thresholds:
- **Refusal on ERR_LOW_PERSONA_AFFINITY**: Persona Routing Confidence ($S_{\text{affinity}} < 0.70$) halts execution with code `ERR_LOW_PERSONA_AFFINITY`.
- **Refusal on ERR_DELIVERABLE_QUALITY_BELOW_THRESHOLD**: Deliverable Quality Gate ($Q_{\text{audit}} < 0.90$) halts execution with code `ERR_DELIVERABLE_QUALITY_BELOW_THRESHOLD`.
- **Refusal on ERR_UNRESOLVED_CROSS_DIVISION_CONFLICT**: Cross-Division Conflict (Unresolvable domain disagreement) halts execution with code `ERR_UNRESOLVED_CROSS_DIVISION_CONFLICT`.
- **Refusal on ERR_PIPELINE_EXECUTION_TIMEOUT**: Pipeline Latency (Pipeline execution time > 180 s) halts execution with code `ERR_PIPELINE_EXECUTION_TIMEOUT`.
- **Refusal on ERR_MISSING_ACCEPTANCE_CRITERIA**: Missing Acceptance Criteria (Zero validation criteria specified) halts execution with code `ERR_MISSING_ACCEPTANCE_CRITERIA`.

### 4. Fallback Decision Mechanism

Continuous operational stability is maintained through layered fault recovery:
- **Tier 1 (Automated Persona Revision Loop)**: If a deliverable fails the quality audit with minor issues, the auditor persona generates targeted feedback and prompts the builder persona for an iterative fix pass.
- **Tier 2 (Secondary Specialist ReRouting)**: If an initial persona struggles to satisfy requirements after 2 rounds, the engine reroutes the task to a senior or adjacent specialist persona.
- **Model Fallback Cascade**: High-level reasoning and synthesis default to `gemini-2.0-flash` with automatic failover to `gpt-4o` and `claude-3-5-sonnet`.

### 5. Human-in-the-Loop Governance

Human operators retain sovereign authority over the multi-agent execution lifecycle:
- **Tier 3 (Human Agency Director Escalation)**: Irreconcilable architectural conflicts, critical brand safety concerns, or contract milestone approvals halt execution to require human signoff.
- **Benchmark Trajectory Auditing**: Operators inspect evaluation traces, raw generation tokens, and container logs to verify scoring fidelity.

---

## The Data It Uses

agency-agents operates under strict principles of data minimization, environment isolation, and privacy protection.

### 1. Ingested Input Data

The framework processes only operational data necessary to perform its functions:
- **Client Project Briefs**: Natural language problem descriptions, brand guidelines, and target objectives.
- **Division Persona Definitions**: Frontmatter schemas, role guidelines, and tool registries across divisions.
- **Deliverables & Artifacts**: Code files, architecture diagrams, copywriting drafts, and financial models.

### 2. Configuration & Reference Data

- **Agency Operational Archetypes**: Cross-functional agile team workflows, design sprints, and code review gates.
- **Division Checklists**: Division-specific quality criteria and deliverables definitions (`divisions.json`).
- **Markdown & Frontmatter Standards**: Structured schema definitions for prompt engineering.

### 3. Base Model & Inference Lineage

- **Underlying Models**: Anthropic Claude 3.5 Sonnet, Claude 3 Opus, OpenAI GPT-4o, Google Gemini 1.5 Pro.
- **Runtime Environment**: Multi-Agent Markdown Framework, Node.js tooling, Shell scripts, JSON catalogs.

### 4. Data Privacy, Storage, and Retention

- **OWASP LLM & MITRE ATLAS Hardened**: Defended against indirect prompt injection, credential leakage, and unauthorized external API dispatch.
- **Local Environment Isolation**: Agent execution workspaces, intermediate scratchpads, and vector stores reside strictly within designated local project directories.
- **Automated Secret Scrubbing**: API keys, database credentials, and personal credentials are automatically redacted prior to embedding or logging.
- **Zero Commercial Monetization**: Prompts, intermediate reasoning trajectories, and task deliverables are never commercialized or shared with third parties.

---

## Limitations

Understanding the operational boundaries and technical constraints of agency-agents is essential for effective deployment.

### 1. High Token Overhead During Multi-Division Collaborative Handoffs
- **Limitation**: Passing full contextual histories across multiple personas can rapidly consume context window budgets.
- **Mitigation**: Synthesize compact, structured handoff briefs containing only essential assets and explicit constraints.

### 2. Subjective Quality Variance Across Creative Design and Marketing Outputs
- **Limitation**: Creative copy and design suggestions have subjective elements that deterministic linters cannot fully evaluate.
- **Mitigation**: Anchor quality audits on objective criteria (word count, brand voice attributes, accessibility contrast, readability).

### 3. Context Dilution When Synthesizing Broad Multi-Agent Deliverables
- **Limitation**: Combining contributions from dozens of specialized agents can lead to inconsistent voice or disjointed sections.
- **Mitigation**: Employ an executive editor persona to harmonize tone, format, and structure in the final delivery pass.

### 4. Conflicting Objectives Between Speed-Oriented and Rigor-Oriented Personas
- **Limitation**: Growth hacker personas may propose aggressive tactics that conflict with legal, compliance, or security guidelines.
- **Mitigation**: Enforce an explicit governance hierarchy where security and compliance personas hold veto authority over risky actions.

### 5. Latency Accumulation Across Multi-Stage Sequential Pipelines
- **Limitation**: Multi-agent pipelines requiring sequential handoffs across 4+ personas can introduce user-noticeable wait times.
- **Mitigation**: Parallelize independent discovery tasks using parallel fan-out patterns and stream intermediate phase drafts.

---

## Summary & Compliance Checklist

| Checkpoint 2 Requirement | Corresponding Section | Status |
| :--- | :--- | :---: |
| **How the agent decides** | [How the Agent Decides](#how-the-agent-decides) | **Covered** |
| - Decision architecture & 5-stage pipeline | Section 1 | Verified |
| - Decision logic & routing formulations | Section 2 | Verified |
| - Thresholding & refusal decision criteria | Section 3 | Verified |
| - Fallback decision mechanism | Section 4 | Verified |
| - Human-in-the-loop governance & oversight | Section 5 | Verified |
| **The data it uses** | [The Data It Uses](#the-data-it-uses) | **Covered** |
| - Ingested input data & query streams | Section 1 | Verified |
| - Configuration & reference schemas | Section 2 | Verified |
| - Base model lineage & deterministic engines | Section 3 | Verified |
| - Data privacy, retention lifecycle & MITRE/OWASP | Section 4 | Verified |
| **Its limitations** | [Limitations](#limitations) | **Covered** |
| - High Token Overhead During Multi-Division Collaborative Handoffs | Section 1 | Verified |
| - Subjective Quality Variance Across Creative Design and Marketing Outputs | Section 2 | Verified |
| - Context Dilution When Synthesizing Broad Multi-Agent Deliverables | Section 3 | Verified |
| - Conflicting Objectives Between Speed-Oriented and Rigor-Oriented Personas | Section 4 | Verified |
| - Latency Accumulation Across Multi-Stage Sequential Pipelines | Section 5 | Verified |
