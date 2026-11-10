# AI-Native-Growth-Operations

An agentic growth operations system for Meta Ads optimization and experimentation.

Built around **DESKFLOW**, a fictional DTC brand serving the US market, this project explores how AI agents can support the daily work of a cross-border e-commerce advertiser: analyzing performance, planning account changes, and running experiments.

## What can be done with this system?

Advertising operators routinely move between fragmented reports, optimization decisions, account changes, and experiment records. This makes it difficult to connect evidence to actions and preserve what was learned.

This project aims to connect those activities in one traceable workflow, with human review of proposed changes and clear records of decisions and outcomes.

## Project Focus

The project is designed as a portfolio for **Growth Marketing / Growth Operations** roles, with an expected development period of approximately 10 weeks.

| Focus | Share of development effort | Scope |
| --- | --- | --- |
| Ads operations core | 70% | Advertising analysis, optimization decisions, simulated operations, and experiments |
| Creative & CRO collaboration | 20% | Creative fatigue analysis, creative test briefs, and conversion investigation |
| Growth operations presentation | 10% | Workflow documentation, evaluation, and portfolio case studies |

These percentages describe planned effort allocation, not completed functionality.

## MVP Scope

| Module | Planned capability |
| --- | --- |
| Ads Analytics | Calculate metrics, compare periods, identify anomalies, and produce evidence-based diagnoses |
| Ads Operations | Recommend budget changes and pause/resume actions, then simulate approved changes |
| Ads Experimentation | Generate hypotheses, design and register experiments, and evaluate results |
| Governance | Validate operations against budget rules, require human approval, and record errors and changes |
| Knowledge | Preserve diagnoses, decisions, experiment results, and reusable findings |

### Outside the First Release

- Real advertising delivery or modifications to live Meta Ads accounts
- Automated creation of complex campaign structures
- Cross-platform advertising optimization
- Influencer marketing management

Creative and CRO collaboration will follow the advertising core. Site-conversion investigations require appropriate site data and will not be inferred from advertising metrics alone.

## Planned System Workflow

Simulated data input
        ↓
Performance diagnosis
        ↓
Optimization proposal
        ↓
Risk and budget validation
        ↓
Human approval
        ↓
Simulated execution and change log
        ↓
Experiment design and evaluation
        ↓
Knowledge and decision records

An operation with a `Pending` or `Rejected` approval status must not change simulated account state. Approval is required for execution, and approved operations must also pass the applicable validation rules.

## Planned Agent Responsibilities

| Agent | Responsibility |
| --- | --- |
| Analysis Agent | Diagnose performance changes and cite supporting data |
| Decision Agent | Propose structured optimization actions and explain risks and uncertainty |
| Experiment Agent | Design experiments and assist with result evaluation |
| Orchestrator Agent | Coordinate tasks, workflow states, and human approvals |

Data loading, metric calculations, budget validation, simulated execution, and logging will use deterministic code. Agents will focus on reasoning, planning, and tool coordination.

## Data and Metrics

The MVP will use synthetic DESKFLOW advertising data at the **Campaign**, **Ad Set**, and **Ad** levels.

Core metrics include:

- Spend, Impressions, Clicks, Purchases, and Purchase Revenue
- CTR, CPC, CPM, CVR, CPA, and ROAS

Metric definitions and attribution assumptions will be documented in the data dictionary. Platform-attributed purchases and revenue must not be presented as proven incremental business impact. Shopify orders, onsite funnel data, and new-customer metrics are potential later extensions.

## Planned Repository Structure

The following is the target structure. Files and folders will be added as the corresponding work is completed.

```text
ai-native-growth-ops/
├── README.md
├── .gitignore
├── docs/
│   ├── project_brief.md
│   ├── requirements.md
│   └── data_dictionary.md
├── data/
│   └── simulated/
│       └── DESKFLOW_simulated_marketing_data_v1.xlsx
├── agents/
│   ├── analysis/
│   ├── decision/
│   └── experiment/
├── workflows/
├── tests/
└── reports/
```

## Roadmap

| Phase | Target period | Deliverable |
| --- | --- | --- |
| Project foundation | Week 1 | Repository, project brief, requirements, initial data archive, and README |
| Data foundation | Week 2 | Dataset audit, Meta Ads data model, and metric definitions |
| Advertising analysis | Week 3 | Analysis Agent and diagnostic reports |
| Advertising operations | Weeks 4–5 | Structured decisions, validation, human approval, and simulated execution |
| Advertising experiments | Weeks 6–7 | Experiment design, registry, and evaluation |
| Integration | Weeks 8–9 | Connected workflow, error handling, and end-to-end validation |
| Portfolio delivery | Week 10 | Demo, case studies, evaluation report, presentation, and video |

The schedule is a planning estimate. Progress will be measured by verified deliverables.

## MVP Acceptance Criteria

- Detect predefined advertising anomalies in synthetic scenarios.
- Include data evidence and uncertainty in each optimization recommendation.
- Require rule validation and human approval before simulated changes.
- Prevent unapproved operations from modifying simulated account state.
- Record the before and after state of simulated operations.
- Define experiment hypotheses, primary metrics, evaluation conditions, and results.
- Support repeatable workflow runs with auditable logs.

Success is defined by correct analysis and controlled, reproducible workflows—not by claims of real advertising ROAS improvement.
