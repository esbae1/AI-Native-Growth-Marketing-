# Project Brief v1.0

## AI-Native Growth Operations OS

**An AI agent system for Meta Ads operations and growth experimentation**

| Item | Definition |
| --- | --- |
| Version | v1.0 |
| Plan confirmed | November 10, 2026 |
| Document prepared | November 12, 2026 |
| Brand | DESKFLOW, a fictional DTC brand |
| Target market | United States |
| Initial platform | Meta Ads |
| Target roles | Growth Marketing / Growth Operations |
| Project type | AI agent MVP and portfolio using simulated business scenarios |
| Estimated duration | Approximately 10 weeks, subject to deliverable acceptance |
| Current phase | Week 1: project foundation and documentation |

This brief defines why the project exists, what the first release covers, and what constitutes completion. All capabilities below are planned, not yet implemented or validated. The README tracks progress; the requirements document will specify detailed rules and acceptance cases.

## 1. Background and Business Problem

Cross-border e-commerce advertisers primarily work on advertising data analysis, account operations, and experiments. These activities are often spread across reports, advertising dashboards, and personal notes:

- Campaign, Ad Set, and Ad data require repeated preparation, calculation, and comparison.
- Moving from anomaly detection to optimization decisions involves repetitive analysis, with evidence and reasoning scattered across tools.
- Budget and advertising status changes lack consistent validation, approval, and recordkeeping.
- Experiment hypotheses, designs, execution records, and results do not consistently become reusable knowledge.
- Platform metrics can be disconnected from business goals, and attributed results may be mistaken for incremental growth.

The project connects analysis, decisions, controlled operations, and experiment evaluation around an advertiser's daily workflow.

## 2. Product Objective

Build a Meta Ads operations system for DESKFLOW with the following workflow:

```text
Data input → Performance diagnosis → Optimization decision → Risk validation
    → Human approval → Simulated execution → Experiment design and evaluation
    → Knowledge capture
```

AI handles analysis, planning, and coordination that can be standardized. The advertiser retains responsibility for business goals, critical judgment, and final approval. The project demonstrates how AI gathers evidence, uses tools, proposes actions, and preserves decision records, beyond generating reports.

## 3. Positioning and Effort Allocation

| Focus | Effort | Main activities |
| --- | --- | --- |
| Ads operations core | 70% | Analysis, optimization decisions, simulated operations, and experiments |
| Creative and CRO collaboration | 20% | Creative performance analysis, fatigue hypotheses, creative testing briefs, and conversion investigations |
| Growth operations and portfolio presentation | 10% | Workflow and decision records, evaluation reports, business cases, and demo materials |

These percentages guide effort, not feature completion. The advertising workflow takes priority, followed by creative and conversion rate optimization (CRO) collaboration. Onsite conversion investigations require appropriate onsite data.

## 4. MVP Scope

| Module | Required capabilities | Main outputs |
| --- | --- | --- |
| Ads Analytics | Load and validate data, calculate metrics, compare trends, identify anomalies, explain evidence and uncertainty | Standardized data and diagnostic reports |
| Ads Operations | Recommend budget adjustments and pause/resume actions; simulate execution after validation and approval | Structured action plans, approval records, before/after states |
| Ads Experimentation | Generate hypotheses, design and register experiments, evaluate against predefined conditions | Experiment plans, records, and evaluations |
| Governance | Human approval, budget constraints, workflow states, operation logs, and error handling | Validation results, approval states, auditable logs |
| Knowledge | Preserve diagnoses, decisions, experiments, and results for retrieval and reuse | Traceable knowledge and decision records |

Initial operations focus on budget adjustments and advertising pause/resume actions. Requirements will define eligible objects, editable fields, and budget constraints.

### 4.1 Outside the First Release

- Automated modifications to live Meta Ads accounts or real advertising delivery.
- Automated creation of complex campaigns or large advertising structures.
- Cross-platform advertising and budget optimization.
- Influencer marketing management.

Live platform integration is a future extension, not a prerequisite for first-release acceptance.

## 5. Agent and Program Responsibilities

The initial design uses four principal agents rather than a separate agent for every processing step.

| Agent | Responsibility |
| --- | --- |
| Analysis Agent | Diagnose performance, provide evidence, distinguish observations from causal hypotheses |
| Decision Agent | Produce structured optimization proposals with rationale, risks, and uncertainty |
| Experiment Agent | Design experiments, define evaluation conditions, assist with result interpretation |
| Orchestrator Agent | Coordinate tasks, workflow states, and human approval |

Data loading, metric calculation, budget validation, simulated execution, and logging use deterministic code. Agents focus on judgment, reasoning, planning, and tool coordination.

Knowledge capture means storing and retrieving external records, not automatic model retraining.

## 6. Data and Metrics

### 6.1 Sources and Levels

The first release uses synthetic DESKFLOW Meta Ads data at the Campaign, Ad Set, and Ad levels.

The existing dataset will be archived as the v1 baseline without modification during Week 1. Week 2 covers data quality review and model upgrades. File completeness and whether fields support the intended capabilities must be confirmed during archival and review.

The evolving model must support advertising performance, account configuration, operation logs, and an experiment registry. Shopify orders, onsite funnels, and new-customer share are later extensions.

### 6.2 Core Metrics

- Base measures: Spend, Impressions, Clicks, Purchases, Purchase Revenue.
- Derived metrics: CTR, CPC, CPM, CVR, CPA, ROAS.

The data dictionary must specify formulas, click and conversion definitions, attribution windows, reporting periods, currency, missing-value handling, and zero-denominator rules. Platform-attributed purchases and revenue must not be interpreted directly as incremental sales.

## 7. Approval and Simulated Execution Principles

- Each proposal identifies the target object, proposed change, supporting evidence, and risks.
- Operations must pass applicable validation rules and receive human approval before simulated execution.
- Operations with a `Pending` or `Rejected` approval status must not modify simulated account state.
- Operations that fail validation must not execute, even if approved.
- Execution records include before/after states, action rationale, approval outcome, and execution result.
- Insufficient inputs and execution failures must be explicitly reported and recorded.

Budget limits and detailed state transitions will be specified in the requirements document. This brief does not assign unconfirmed thresholds.

## 8. MVP Success Criteria

The MVP succeeds when simulated scenarios demonstrate that it can:

1. Correctly identify predefined anomalies and provide supporting evidence.
2. Include evidence and uncertainty in every recommendation without presenting causal hypotheses as facts.
3. Enforce validation and approval, preventing unapproved or invalid operations from executing.
4. Record before/after states and trace changes to proposals and approvals.
5. Document experiment hypotheses, primary metrics, evaluation conditions, and results; report when evidence is insufficient for a conclusion.
6. Run the complete workflow repeatedly with auditable logs.

Scenarios, pass conditions, and evaluation methods will be defined in `docs/requirements.md`. Real advertising ROAS improvements are not a success claim for this simulated MVP.

## 9. Implementation Milestones

| Phase | Estimated period | Deliverables |
| --- | --- | --- |
| Project foundation | Week 1 | Repository, README, brief, requirements, initial data archive, task list |
| Data foundation | Week 2 | Dataset review, Meta Ads data model, data dictionary, metric rules |
| Advertising analysis | Week 3 | Analysis Agent and diagnostic reports |
| Advertising operations | Weeks 4–5 | Structured proposals, validation, approval, simulated execution, logs |
| Advertising experiments | Weeks 6–7 | Experiment design, registration, evaluation |
| System integration | Weeks 8–9 | Connected workflow, error handling, end-to-end validation, creative/CRO collaboration scenarios |
| Portfolio delivery | Week 10 | Demo, cases, architecture diagram, evaluation report, presentation, video |

The schedule is an estimate; progress depends on verified deliverables. Creative and CRO timing will be refined as the core system progresses.

## 10. Portfolio Deliverables

Final deliverables include a GitHub repository and documentation, a runnable MVP demo, an architecture diagram, an evaluation report, a project presentation, and a demonstration video.

Three business cases will connect analysis, decisions, and validation:

1. Declining advertising acquisition efficiency.
2. Creative fatigue and testing.
3. Stable advertising clicks with declining onsite conversion, after appropriate onsite data is added in a later phase.

The portfolio demonstrates business understanding, AI application, experimentation, and operational process design. All synthetic data and simulated results must be labeled, without claims of real business growth or production readiness.

