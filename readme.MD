# Silver Layer Take-Home Exercise

This workspace contains the input artifacts for a take-home data exercise based on a fictional architecture and engineering firm. The goal is to inspect the silver-layer data, identify and explain data quality issues, clean the data into something dashboard-ready, and propose at least one useful new metric.

## Objective

Use the provided files to:

1. Investigate data quality problems in the source data.
2. Clean and reconcile the datasets.
3. Compare the cleaned data to the provided dashboard output.
4. Propose a new metric that would improve project portfolio reporting.
5. Document assumptions, tradeoffs, and next steps.

The original exercise guidance suggests a target of about 90 minutes, with a hard stop at 2 hours.

## Files

### `project_financials.csv`

Project-level financial and schedule data.

Columns:

- `project_id`
- `project_name`
- `sector_code`
- `office`
- `pm_name`
- `client_name`
- `contract_value`
- `costs_to_date`
- `billed_to_date`
- `percent_complete`
- `start_date`
- `target_end_date`

### `staffing_hours.csv`

Project staffing data at a weekly grain, with budgeted and actual hours by employee and role.

Columns:

- `project_id`
- `employee_name`
- `role`
- `week_ending`
- `hours_budget`
- `hours_actual`

Note: this file also contains summary-style rows where `employee_name` and `role` are blank, which likely represent aggregate monthly totals rather than employee-level detail.

### `dashboard-screenshot.html`

An HTML reconstruction of the weekly performance dashboard. This serves as the expected business-facing output and a reference point for validation.

### `insructions.txt`

The original take-home prompt. The filename contains a typo and is intentionally left as-is in the repo.

## Early Data Quality Signals

Several issues are visible immediately and should be addressed or explicitly documented during cleanup:

- Duplicate or conflicting project keys for the Northshore data center project: `MCHC-2024-017` and `MC-HC-2024-017`.
- A project name variant for the same Northshore project: `Northshore Data Center Fit-Out` vs `Northshore Data Ctr Fit-Out`.
- Missing `contract_value` for `WP-2024-031`.
- A sector code mismatch: `MC-R` appears in `project_financials.csv`, while the dashboard sector summary shows `MC`, `HE`, `HC`, `WP`, `SCI`, and `INT`.
- Mixed-grain records in `staffing_hours.csv` because employee-level rows and aggregate summary rows appear in the same file.

## Suggested Workflow

1. Profile both CSV files for nulls, duplicates, invalid categories, and grain mismatches.
2. Standardize project identifiers and project names before joining datasets.
3. Separate employee-level staffing rows from summary rows if they serve different reporting purposes.
4. Recalculate core portfolio metrics such as contract value, margin, and risk flags from cleaned data.
5. Compare results against the dashboard reference and explain any differences.

## Useful Validation Questions

- Which records should count as active projects?
- How should missing contract value be treated in portfolio totals?
- Is `MC-R` a valid new sector, or should it map to `MC`?
- Should duplicate project IDs be merged, dropped, or escalated as unresolved source issues?
- Are the blank-name staffing rows legitimate summaries or erroneous records?

## Candidate Metric Ideas

If you need to propose a new metric, reasonable options include:

- Budgeted vs actual labor variance by project.
- Burn efficiency: percent complete compared with percent of contract consumed.
- Billing lag: billed-to-date minus cost progress or completion progress.
- PM load index: number of active projects and labor variance per project manager.

## Deliverable Guidance

A strong submission usually includes:

- Cleaned datasets or transformation logic.
- A short write-up of issues found and how they were handled.
- Recreated or improved reporting outputs.
- A clearly defined proposed metric with business rationale.
- Any limitations and what would be investigated next with more time.
