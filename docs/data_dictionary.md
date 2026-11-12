# Data Dictionary — Draft v0.1

**Project:** AI-Native Growth Operations OS · DESKFLOW · Meta Ads

**Prepared:** November 12, 2026

**Status:** Proposed project conventions; source-file reconciliation pending.

## 1. Scope and Evidence

The planned baseline is `data/simulated/DESKFLOW_simulated_marketing_data_v1.xlsx`. Project discussions identify three baseline sheets: **Campaign Data**, **Creative Data**, and **Experiment Log**. The workbook is not available in the local project, so sheet names, headers, types, row counts, values, and relationships have not been independently verified.

This document defines a proposed normalized interpretation for those sheets. It is not an inventory of confirmed source columns. All field names below are proposed canonical names, and all source-header mappings remain pending. Missing fields must be reported as gaps, not invented or silently filled. Preserve the baseline workbook unchanged; implement approved mappings and upgrades in Week 2.

The dictionary does not claim API compatibility or official Meta field definitions. Click, conversion, and attribution conventions below are project proposals that require source confirmation.

## 2. Shared Conventions

| Topic | Proposed convention |
| --- | --- |
| Names | Canonical fields use `snake_case`; preserve original source headers in the mapping record |
| IDs | Nonempty strings; preserve leading zeros; names are not identifiers |
| Dates | Calendar dates in `YYYY-MM-DD` format |
| Timestamps | ISO 8601 with explicit UTC offset; use UTC for system logs |
| Reporting timezone | Required dataset metadata; not inferred from the US market or the operator's location |
| Currency | Proposed USD for DESKFLOW; verify the workbook and record an ISO currency code |
| Money | Nonnegative decimal amounts in major currency units, not cents; preserve source precision |
| Counts | Nonnegative integers for the proposed synthetic model; flag fractional source values for review rather than rounding |
| Ratios | Store as decimal fractions, e.g. `0.025`; display as `2.5%` where appropriate |
| Missing values | Normalize blank/NA cells to null; zero means an observed zero, not unknown |
| Synthetic provenance | Record the dataset as synthetic in its metadata; never describe simulated results as real business outcomes |

### Dataset Metadata

These properties must be recorded alongside the dataset before analysis. They are proposed metadata, not confirmed workbook columns.

| Property | Type | Required | Definition |
| --- | --- | --- | --- |
| `dataset_version` | string | Yes | Version identifying the immutable input baseline |
| `is_synthetic` | boolean | Yes | Must be `true` for this dataset |
| `platform` | string | Yes | `Meta Ads` for the planned MVP |
| `currency` | string | Yes | Currency shared by monetary measures |
| `reporting_timezone` | string | Yes | Named reporting timezone used to interpret dates |
| `click_definition` | string | Yes | Exact click measure; proposed: link clicks |
| `purchase_definition` | string | Yes | Exact attributed purchase event being counted |
| `attribution_window` | string | Yes | Explicit click-through/view-through windows; unresolved until verified |
| `reporting_time_basis` | string | Yes | Whether conversion metrics are assigned to interaction date or conversion date |
| `coverage_start` / `coverage_end` | date | Yes | Inclusive coverage interval |
| `data_as_of` | timestamp | Yes | Snapshot time for the synthetic reporting data |

Do not combine records with incompatible currency, timezone, click definitions, attribution windows, reporting time basis, or dataset versions. Unresolved metadata blocks interpretation of affected metrics and period comparisons.

## 3. Campaign Data — Proposed Canonical Fields

**Target grain:** one campaign per reporting date, with one consistent set of reporting definitions. **Proposed unique key:** (`date`, `campaign_id`) within a dataset version. This assumes one account and no device, placement, audience, or other breakdown. If source rows use a different grain, revise the mapping before aggregation; do not discard apparent duplicates.

`Required` means required by the proposed analysis model, not confirmed present in the workbook.

| Field | Type | Required | Meaning and validation |
| --- | --- | --- | --- |
| `date` | date | Yes | Reporting date within dataset coverage |
| `campaign_id` | string | Yes | Stable campaign identifier |
| `campaign_name` | string | No | Human-readable label; not a join key |
| `spend` | decimal | Yes | Advertising cost in the dataset currency; >= 0 |
| `impressions` | integer | Yes | Delivered impression count; >= 0 |
| `clicks` | integer | Yes | Count under `click_definition`; >= 0 |
| `purchases` | integer | Yes | Attributed purchase events under the declared definition; >= 0 |
| `purchase_revenue` | decimal | Yes | Attributed purchase value, not profit or incremental revenue; >= 0 |
| `ctr` | decimal | Derived | `clicks / impressions` |
| `cpc` | decimal | Derived | `spend / clicks`, currency per click |
| `cpm` | decimal | Derived | `1000 * spend / impressions`, currency per 1,000 impressions |
| `cvr` | decimal | Derived | `purchases / clicks`; project click-to-purchase ratio, not onsite session CVR |
| `cpa` | decimal | Derived | `spend / purchases`, currency per attributed purchase |
| `roas` | decimal | Derived | `purchase_revenue / spend`, expressed as a multiple |

Do not label CPA as CAC: the baseline does not establish new-customer counts. View-through purchases and repeat purchase events may make purchases exceed clicks; CVR is a diagnostic ratio, not necessarily a probability bounded by 1.

## 4. Creative Data — Proposed Canonical Fields

**Proposed target grain:** one campaign–creative pair per reporting date. **Proposed unique key:** (`date`, `campaign_id`, `creative_id`) within a dataset version. Source grain is unverified; if the sheet is a static creative inventory or uses period totals, document that and revise this proposal before use.

| Field | Type | Required | Meaning and validation |
| --- | --- | --- | --- |
| `date` | date | Yes | Reporting date; must align with the campaign report |
| `campaign_id` | string | Yes | Campaign identifier for the corresponding campaign/date record |
| `creative_id` | string | Yes | Stable creative identifier; does not establish an Ad ID |
| `creative_name` | string | No | Human-readable label |
| `creative_format` | string | No | Creative format label; preserve source categories until reviewed |
| `message_angle` | string | No | Creative message or value proposition tag |
| `spend` | decimal | Yes for performance analysis | Cost attributed to this creative slice |
| `impressions` | integer | Yes for performance analysis | Impression count for this creative slice |
| `clicks` | integer | Yes for performance analysis | Same click definition as campaign data |
| `purchases` | integer | Yes for purchase analysis | Same purchase/attribution definitions as campaign data |
| `purchase_revenue` | decimal | Yes for revenue analysis | Attributed purchase value for this creative slice |

Numeric base fields follow Campaign Data types and nonnegative rules. Derived metrics use the formulas in Section 3. Missing purchase measures allow limited click analysis but block CPA/ROAS evaluation.

Creative and campaign performance are alternate reporting levels. Never add them together. Reconciliation is valid only when creative rows provide complete, mutually exclusive coverage of the same campaign, dates, and reporting definitions. A creative may appear in multiple campaigns. Creative attributes should remain consistent per ID within a snapshot; conflicting attributes require review.

The baseline does not establish creative-to-ad mappings, frequency, reach, or fatigue thresholds. Declining CTR may motivate a fatigue hypothesis but does not prove fatigue.

## 5. Experiment Log — Proposed Canonical Fields

**Target grain:** one experiment per row. **Proposed unique key:** `experiment_id`. This is a summary registry, not a substitute for experiment-arm observations.

| Field | Type | Required | Meaning and validation |
| --- | --- | --- | --- |
| `experiment_id` | string | Yes | Unique experiment identifier |
| `experiment_name` | string | No | Human-readable label |
| `hypothesis` | string | Yes | Falsifiable business hypothesis |
| `test_variable` | string | Yes | Deliberately changed variable |
| `control_definition` | string | Yes | Control population, exposure, and configuration |
| `treatment_definition` | string | Yes | Treatment population, exposure, and configuration |
| `assignment_method` | string | Yes before launch | How units enter arms; distinguish randomized tests from observational comparisons |
| `primary_metric` | string | Yes | Prespecified metric with a formula defined here or in the experiment plan |
| `start_date` | date | Before launch | Inclusive planned start |
| `end_date` | date | Before launch | Inclusive planned end; must be >= start |
| `evaluation_conditions` | string | Before launch | Sample requirements, duration, stopping criteria, and analysis method |
| `status` | enum | Yes | Proposed: `Draft`, `Planned`, `Running`, `Completed`, `Cancelled` |
| `result_summary` | string | For completed records | Observed result, uncertainty, and evidence limitations |
| `conclusion` | enum | For completed records | Proposed: `Supported`, `NotSupported`, `Inconclusive` |
| `evidence_reference` | string | For completed records | Traceable reference to arm-level data and evaluation output |

Status does not imply approval. A completed experiment can be inconclusive. Historical before/after comparisons alone do not establish causal lift. A `Supported` conclusion requires the prespecified evaluation conditions to be met; missing evidence must not be converted into a positive conclusion.

## 6. Calculation and Aggregation Rules

1. Recalculate derived metrics from validated base measures. Preserve source-reported metrics separately for comparison; do not overwrite the baseline.
2. A zero denominator produces null with reason `zero_denominator`, not zero or infinity. A missing input produces null with reason `missing_input`. Thus zero purchases makes CPA null, while zero revenue with positive spend makes ROAS zero.
3. Calculate group metrics from summed base measures, not the arithmetic mean of row-level ratios. For example, aggregate ROAS = total revenue / total spend.
4. Aggregate only nonoverlapping rows at one grain and one reporting definition. Do not combine campaign totals with creative totals or overlapping reporting windows.
5. Missing rows and missing required values mean incomplete coverage, not zero activity. Block a complete-period claim until coverage is resolved; partial analysis must be explicitly labeled.
6. Round only display values. Preserve calculation precision. Differences from source ratios require documented display precision and tolerance before being judged errors.
7. Relative change = `(current - previous) / previous`. If the previous value is zero or missing, relative change is null. For rate metrics, percentage-point change = `100 * (current - previous)`.
8. Compare complete, equally sized periods with matching metadata. Synthetic snapshot maturity and attribution assumptions must be noted; avoid causal language based on period changes alone.

### Illustrative Formula Check — Not Source Data

For spend = 100 USD, impressions = 10,000, clicks = 200, purchases = 10, and purchase revenue = 500 USD:

| Metric | Expected value |
| --- | --- |
| CTR | 0.02 = 2% |
| CPC | 0.50 USD |
| CPM | 10.00 USD |
| CVR | 0.05 = 5% |
| CPA | 10.00 USD |
| ROAS | 5.0x |

For two rows with spend/revenue of 100/500 and 900/900, aggregate ROAS is 1.4x, not the unweighted average of 5x and 1x (3x).

## 7. Validation Rules

| Check | Handling |
| --- | --- |
| Missing required headers, IDs, or reporting metadata | Block the affected analysis; record the missing requirement |
| Invalid numeric types, negative base values, invalid dates | Reject affected rows from calculation and record errors; never silently coerce |
| Duplicate proposed keys | Block aggregation until the actual grain or duplication is resolved |
| Creative campaign/date without matching campaign record | Flag an unresolved relationship; block cross-level reconciliation |
| Clicks greater than impressions | Flag for review against the declared click definition; do not automatically cap values |
| Purchases greater than clicks | Review attribution/event definitions; not an automatic hard failure |
| Revenue with zero purchases | Flag purchase/value definition inconsistency for review |
| Source versus recalculated metrics differ | Review ratio units, rounding, and denominator definitions |
| Campaign/creative totals differ | Check coverage and exclusivity before treating as an error |
| Missing experiment evidence or conditions | Block a supported-outcome claim; record the evidence gap |

No budget-change threshold, anomaly cutoff, or statistical significance level is established by this draft. Those belong in approved requirements and experiment plans.

## 8. Week 2 Extensions — Not Existing Baseline Fields

The MVP will need Ad Set and Ad performance with explicit hierarchy IDs, account configuration, approval and operation records, and experiment-arm observations. Their schemas are deferred to the Week 2 model upgrade.

At minimum, future operation records must identify the action, target, proposed change, rule-validation result, approval decision, before/after state, and execution result. `Pending` and `Rejected` approvals must never permit execution; approval must bind to the exact proposal being executed. A changed proposal requires renewed approval. Failed validation blocks execution even after approval.

Budget type, currency, configuration version, and budget ownership must be explicit before budget changes are simulated. Experiment observations need arm, assignment unit, reporting interval, and metric numerators/denominators to support the chosen evaluation method. These are design requirements, not claims that the workbook already contains these fields.

## 9. Source Reconciliation Checklist

- [ ] Obtain the unchanged baseline workbook and record its version and provenance.
- [ ] Verify actual sheet names, headers, data types, date coverage, row counts, and grain.
- [ ] Map each source header to a canonical field; record unmapped and missing fields.
- [ ] Confirm reporting timezone, currency, click/purchase definitions, attribution, and reporting time basis.
- [ ] Compare supplied metrics with recalculated metrics using documented precision.
- [ ] Review uniqueness, missingness, hierarchy relationships, and cross-level coverage.
- [ ] Verify experiment records and the availability of arm-level evidence.
- [ ] Approve revisions before marking this dictionary source-verified.

**Review outcome:** Internal design checks are documented in `data_dictionary_review.md`. Source-data verification remains pending.

