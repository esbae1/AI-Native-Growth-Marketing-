# Data Dictionary — Baseline v1.0

**Project:** AI-Native Growth Operations OS · DESKFLOW · Meta Ads  
**Reviewed:** November 12, 2026  
**Source:** `DESKFLOW_simulated_marketing_data_v1.xlsx`  
**Status:** Source structure and stored metric calculations verified. Business definitions and reporting metadata remain partly unresolved.

## 1. Scope and Provenance

This dictionary describes the actual supplied workbook, replacing Draft v0.1's proposed schemas. Source headers are preserved exactly below; no normalized dataset has been created. The source workbook has not been modified.

All records are labeled `SIMULATED`. The file contains no live account connection, execution evidence, or demonstrated causal growth results. Scenario tags and AI text are synthetic annotations, not verified diagnoses or outputs of the project's future agents.

| Sheet | Data rows, excluding header | Columns | Observed grain | Verified unique key |
| --- | --- | --- | --- | --- |
| Campaign Data | 150 | 18 | One campaign per reporting date | (`Date`, `Campaign_ID`) |
| Creative Data | 15 | 11 | One creative record with a campaign association | `Creative_ID` |
| Experiment Log | 6 | 10 | One logged recommendation/observation | `Experiment_ID` |

Campaign reporting covers October 1–30, 2026 inclusive: five campaigns, each with 30 daily records. The workbook contains stored values, not Excel formulas. No Excel error cells were found.

## 2. Shared Types and Conventions

- IDs and labels are text. Join by IDs, not names; preserve leading zeros.
- `Date`, `Launch_Date`, and `Date_Logged` are stored as text in `YYYY-MM-DD` form. A future loader must parse and validate them explicitly.
- Count measures are nonnegative integer-valued numbers. Monetary measures are nonnegative numbers in USD major units, not cents.
- Rate values are fractions: `0.02` means `2%`. ROAS is a multiple, not a percentage.
- Blank means unavailable or undefined; it must not be silently converted to zero.
- Field types describe observed content and intended interpretation, not a formally enforced Excel schema.
- Display rounding must not change calculation inputs. The independent numeric comparison used absolute/relative tolerance of `1e-9` for stored floating-point ratios, not an operational performance threshold.

## 3. Campaign Data

| Source field | Type / unit | Definition and observed constraints |
| --- | --- | --- |
| `Date` | date text | Campaign reporting date, October 1–30, 2026 |
| `Campaign_ID` | text | Stable ID; observed C001–C005; part of unique key |
| `Campaign_Name` | text | Human-readable campaign label, not a join key |
| `Objective` | text | Synthetic objective label; all rows contain `Sales` |
| `Spend_USD` | number, USD | Daily advertising spend; positive in all supplied rows |
| `Impressions` | integer | Daily impression count; positive in all supplied rows |
| `Clicks` | integer | Daily clicks; positive in all supplied rows; exact click type is not documented |
| `Conversions` | integer | Daily attributed conversion count; includes three zero values; purchase-event meaning requires confirmation |
| `Revenue_USD` | number, USD | Synthetic attributed revenue; not profit or incremental revenue; precise event-value definition is undocumented |
| `Currency` | text | All rows contain `USD` |
| `Attribution_Window` | text | All rows contain `7d_click (simulated)`; preserve as source metadata |
| `Data_Type` | text | All rows contain `SIMULATED` |
| `Scenario_Tag` | text | Synthetic scenario annotation; not a measured outcome or model prediction |
| `CTR` | number, fraction | `Clicks / Impressions` |
| `CPC_USD` | number, USD/click | `Spend_USD / Clicks` |
| `CVR` | number, fraction | `Conversions / Clicks`; not session-based onsite CVR |
| `CPA_USD` | nullable number, USD/conversion | `Spend_USD / Conversions`; blank when conversions are zero |
| `ROAS` | number, multiple | `Revenue_USD / Spend_USD` |

### Campaign and Scenario Labels

| Campaign ID | Campaign name | Scenario tag |
| --- | --- | --- |
| C001 | Core_Desk_Organization | Stable baseline |
| C002 | Remote_Work_Problem_Solution | High clicks, low conversion |
| C003 | Broad_Audience_Scale | High spend, low return |
| C004 | Modular_Desk_Setup_Video | Creative fatigue |
| C005 | Minimalist_Desk_Enthusiasts | Promising but low volume |

Tags describe intended synthetic scenarios. Their statistical detectability and whether they match the numeric trends have not been evaluated in this structural review. In future anomaly evaluation, keep tags outside model inputs to avoid leaking the intended answer.

### Conversion Interpretation

The source calls its count `Conversions`, not `Purchases`. The project brief targets purchases, but this dictionary does not silently rename or assert that equivalence. Confirm the event before mapping `Conversions` to `purchases` or `Revenue_USD` to `purchase_revenue`. Until then, CPA means cost per source-defined conversion. It is not customer acquisition cost (CAC), because new-customer counts are absent.

### CPM

CPM is required by the project brief but is not a source column. It may be derived in a future prepared dataset as `1000 * Spend_USD / Impressions`, with unit USD per 1,000 impressions. It has not been added to the baseline.

## 4. Creative Data

This sheet is a creative inventory, not a daily performance table. It has no reporting `Date`, spend, impressions, clicks, conversions, or revenue columns. `Launch_Date` must not be interpreted as a daily reporting key.

| Source field | Type | Definition |
| --- | --- | --- |
| `Creative_ID` | text | Unique creative identifier |
| `Campaign_ID` | text | Campaign association; all supplied IDs exist in Campaign Data |
| `Creative_Name` | text | Human-readable creative label |
| `Format` | text | Observed categories: `Video`, `Carousel`, `Image` |
| `Creative_Angle` | text | Creative approach or value-proposition category |
| `Hook_or_Message` | text | Opening hook or advertising message |
| `Audience_Concept` | text | Intended audience concept; not verified delivery targeting |
| `Launch_Date` | date text | Synthetic creative launch date |
| `Market` | text | Market label |
| `Product_Category` | text | Product-category label |
| `Data_Type` | text | All rows contain `SIMULATED` |

A campaign has multiple creative records. Joining every campaign-day performance row to its creatives duplicates campaign measures; do not sum the joined measures or allocate them equally to creatives. The inventory cannot establish creative-level CTR, CPA, ROAS, fatigue, or creative-to-ad attribution. Performance and exposure data are needed for those tasks.

## 5. Experiment Log

This is a synthetic suggestion and observation log. It is not a completed randomized experiment registry, and it contains no arm-level outcomes.

| Source field | Type | Definition |
| --- | --- | --- |
| `Experiment_ID` | text | Unique log-entry identifier |
| `Date_Logged` | date text | Date the recommendation or observation was logged; not an experiment start date |
| `Campaign_ID` | text | Associated campaign; all supplied IDs exist in Campaign Data |
| `Observed_Problem` | text | Synthetic problem or observation description |
| `AI_Hypothesis` | text | Proposed explanation, not established causation |
| `AI_Recommendation` | text | Suggested next action or investigation, not an executable command |
| `Human_Approval` | text | Observed labels: `Pending`, `Approved for analysis only` |
| `Action_Taken` | text | Narrative action record, not a machine-verifiable execution status |
| `Result_or_Learning` | text | Narrative learning/status, not measured experimental lift |
| `Data_Type` | text | All rows contain `SIMULATED` |

Five entries are `Pending`. EXP005 is `Approved for analysis only`, with `No live account change` and `Baseline observation only`. This does not approve budget or status changes. No supplied row provides evidence of a completed controlled experiment or executed account modification.

A future structured registry needs hypothesis, test variable, arms, assignment method, primary metric, sample requirements, duration, stopping conditions, and evidence references. Evaluation additionally needs arm-level observations. Missing evidence must produce an unavailable or inconclusive result, not an invented winner.

## 6. Relationships and Aggregation

1. Campaign Data is unique by (`Date`, `Campaign_ID`). Creative and experiment campaign references resolve to the campaign IDs in the performance sheet.
2. Creative Data and Experiment Log are both one-to-many relative to campaign identity. Joining them directly to performance can multiply rows; aggregate or select context separately.
3. Sum base measures only across nonoverlapping records with compatible reporting definitions. Recalculate ratios from summed numerators and denominators; never average daily CPA or ROAS without the appropriate weighting.
4. No Ad Set IDs, Ad IDs, account configuration, budget ownership, or structured operation history are present. Do not fabricate that hierarchy from campaign or creative names.
5. Missing dates/values are not zero activity. Complete-period claims require complete coverage.
6. Do not merge incompatible attribution, currency, timezone, or reporting time bases.

## 7. Calculation and Missing-Value Rules

- A zero denominator yields null with reason `zero_denominator`, not zero or infinity. A missing numerator or denominator yields null with reason `missing_input` in future prepared outputs.
- The baseline has three zero-conversion rows and three blank `CPA_USD` cells; these blanks correctly represent undefined CPA. All other inspected source cells are populated.
- Positive spend with zero revenue yields ROAS zero; this differs from undefined CPA when conversions are zero.
- Aggregate CPA = total spend / total conversions; aggregate ROAS = total revenue / total spend.
- Relative change = `(current - previous) / previous`; zero/missing prior values make the relative change unavailable.
- Percentage-point change for rates = `100 * (current - previous)`. Compare equally sized complete periods with matching definitions.
- Source-reported ratios are static values. Future transformations should recalculate them without overwriting the source.

For example, Campaign Data row 2 contains spend 91.02 USD and 8 conversions. CPA = 91.02 / 8 = 11.3775 USD per conversion. Reversing the ratio gives conversions per dollar, not CPA.

## 8. Verification Results and Limits

| Check | Result |
| --- | --- |
| Sheet names, headers, and row counts | Verified from all three sheets |
| Campaign date range and daily coverage | 30 days for each of five campaigns |
| Duplicate campaign-day, creative, or experiment keys | None found |
| Creative/experiment references to unknown campaign IDs | None found |
| Negative base measures or fractional count values | None found in Campaign Data |
| Stored CTR, CPC, CVR, CPA, and ROAS | All 750 row/metric positions match recalculation, including three undefined CPAs |
| Excel error cells | None found |
| Excel formulas | None present; metric values are stored numbers |
| Missing cells | Three CPA blanks, consistent with zero conversions |
| Reporting timezone, extraction timestamp, reporting time basis | Not supplied |
| Exact click type and conversion event | Not supplied |
| Scenario behavior and statistical detectability | Not evaluated in this review |
| Real platform/API compatibility | Not assessed |

## 9. Remaining Metadata and Week 2 Work

- Confirm click type, conversion event, revenue definition, reporting timezone, reporting time basis, and synthetic snapshot timestamp.
- Preserve `7d_click (simulated)` as the observed attribution label; do not silently replace it with a different window or assert additional attribution behavior.
- Add CPM and any approved canonical field mappings in a derived version.
- Design Ad Set/Ad performance, account configuration, structured approvals and operation logs, and experiment observations.
- Bind each future approval to the exact action proposal. Pending, rejected, analysis-only, or invalid proposals must not execute. Changed proposals require renewed approval.
- Define budget rules and experiment criteria in the requirements; this dictionary establishes no arbitrary thresholds.

The baseline is suitable for initial campaign-metric analysis with explicit definition limitations. It is insufficient for creative-level performance attribution, controlled execution, or experiment-effect evaluation. Preserve v1 unchanged and version future upgrades separately.
