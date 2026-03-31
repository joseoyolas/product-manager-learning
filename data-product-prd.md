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
- CS team adoption: >60% of CSMs actively using the signal within 60 days of launch
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

## Stakeholders

| Name | Role | Involvement |
|---|---|---|
| CS Leadership | Decision maker | Approve go/no-go at each milestone; define pilot group |
| Data Engineering Lead | Accountable | Pipeline builds, data quality checks, dashboard integration |
| Data Science Lead | Accountable | Model development, backtesting, retraining cadence |
| Sales Ops | Consulted | CRM data access and quality remediation |
| Finance / RevOps | Consulted | Contract data access and definitions |
| CS Ops | Consulted | Support system data access |
| 2–3 pilot CSMs | Informed + testers | Early feedback during pilot phase |

---

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

## Risks

| Risk | Likelihood | Impact | Mitigation |
|---|---|---|---|
| CRM data is incomplete or inconsistently maintained | High | High | Audit CRM data quality before model training; flag accounts with missing fields rather than imputing silently |
| Model accuracy is too low to be trusted by CSMs | Medium | High | Run a backtest on 12 months of historical data before launch; set a minimum precision threshold as a go/no-go criterion |
| CS team ignores the signal and reverts to gut feel | Medium | High | Involve 2–3 CSMs in design and testing; tie adoption metric to team OKRs |
| Input pipeline failure causes stale scores | Medium | Medium | Add monitoring and alerting on pipeline jobs; surface data freshness timestamp in dashboard |
| New accounts have insufficient history to score | High | Low | Display explicit "insufficient data" state rather than a score; define minimum account age threshold |
| Scoring model encodes historical bias (e.g., segments treated unfairly) | Low | High | Review score distributions across segments before launch; document known limitations |

### Risk Summary

The two highest-priority risks are **data quality** (CRM completeness) and **adoption**. Both should be addressed before launch, not after. A technically accurate model that CSMs don't trust or use delivers zero value.

---

## Timeline

| Phase | Activities | Target Date | Owner |
|---|---|---|---|
| **Phase 0 — Discovery** | Data quality audit, CRM completeness review, align on "churned" definition | Week 1–2 | Data Engineering + RevOps |
| **Phase 1 — Data Prep** | Pipeline builds for all 4 input sources, warehouse validation, data quality checks live | Week 3–5 | Data Engineering |
| **Phase 2 — Model Development** | Feature engineering, model training, backtest on 12 months of history, precision threshold review | Week 6–9 | Data Science |
| **Phase 3 — Dashboard Integration** | Score surfaced in CS dashboard, explainability factors visible, freshness timestamp shown | Week 9–11 | Data Engineering + Product |
| **Phase 4 — Pilot** | 5–8 CSMs using signal in their workflow, feedback collected, model adjustments if needed | Week 12–14 | CS Leadership + Data Science |
| **Phase 5 — Launch** | Full CS team rollout, adoption tracking live, retraining cadence established | Week 15–16 | Product + CS Ops |

### Key Milestones

- **End of Week 2:** Go/no-go on data quality — if CRM completeness is below threshold, scope is adjusted before model work begins
- **End of Week 9:** Backtest results reviewed with CS Leadership — minimum precision threshold must be met to proceed to integration
- **End of Week 14:** Pilot retrospective — adoption and feedback determine if full rollout proceeds on schedule

### Assumptions

- Data warehouse access and permissions are in place before Phase 1 begins
- A CS pilot group is identified and committed by end of Week 10
- No major product or pricing changes during model training period (would require retraining)

---

## Open Questions

1. What is the source of truth for "churned" — contract end date or last login?
2. Do we train on all segments or start with mid-market only?
3. Who owns the model retraining cadence — data engineering or data science?
