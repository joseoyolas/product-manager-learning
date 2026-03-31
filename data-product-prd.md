# PRD: Internal Data Product — Customer Churn Signal

**Status:** In Review
**Author:** Jose Oyola
**Last Updated:** 2026-03-30
**Version:** 0.1

---

## Problem Statement

Our customer success team currently has no reliable early signal for churn risk. They rely on reactive support tickets and anecdotal feedback. By the time risk is identified, the window to intervene is often closed.

## Goal

Build an internal data product that surfaces churn risk scores for accounts at least 30 days before predicted churn, giving CS teams actionable lead time.

## Success Metrics

- 80% of churned accounts appear in the top-20% risk tier at least 30 days before churn
- CS team adoption: >70% of CSMs actively using the signal within 60 days of launch
- Reduction in reactive escalations by 25% within one quarter

## Users

| User | Need |
|---|---|
| Customer Success Managers | Know which accounts need attention this week |
| CS Leadership | Portfolio-level view of churn risk by segment |
| Data team | Reliable, governed input data and feedback loop |

## Scope (v1)

**In scope:**
- Churn risk score per account, updated daily
- Top risk factors driving each score (explainability)
- Integration into existing CS dashboard tool

**Out of scope:**
- Automated outreach or action triggers
- Predictive expansion/upsell signals (future phase)

## Technical Requirements

### Data Inputs

| Source | Data Elements Needed | Owner |
|---|---|---|
| Product event logs | Login frequency, feature usage, session duration, API call volume | Data Engineering |
| CRM data | Account tier, ARR, CSM assignment, open opportunities, health score | Sales Ops |
| Contract data | Contract start/end dates, renewal dates, add-on history, payment status | Finance / RevOps |
| Support system | Ticket volume, severity, time-to-resolution, escalation flags | CS Ops |

All input data must be available in the central data warehouse (not pulled ad hoc from source systems).

### Refresh Cadence

- **Churn risk scores:** Updated daily, processed overnight (target: available by 7am local time for CS team)
- **Underlying input data:** Must be no more than 24 hours stale at time of model run
- **Model retraining:** Monthly, or triggered manually after significant product/pricing changes

### Latency Requirements

| Interaction | Acceptable Latency |
|---|---|
| Risk score load in CS dashboard | < 3 seconds (p95) |
| Full account risk factor breakdown | < 5 seconds (p95) |
| Batch score refresh (overnight job) | Must complete within 4-hour window (2am–6am) |

### Data Quality Requirements

- Input pipelines must have data quality checks with alerting on failure — a failed pipeline should not silently produce stale scores
- Risk scores must include a `data_freshness_timestamp` so CSMs can see when the score was last calculated
- Null or missing scores (e.g., new accounts with insufficient history) must be surfaced explicitly, not defaulted to "low risk"

---

## Open Questions

1. What is the source of truth for "churned" — contract end date or last login?
2. Do we train on all segments or start with mid-market only?
3. Who owns the model retraining cadence — data engineering or data science?
