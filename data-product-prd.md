# PRD: Internal Data Product — Customer Churn Signal

**Status:** Draft
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

## Open Questions

1. What is the source of truth for "churned" — contract end date or last login?
2. Do we train on all segments or start with mid-market only?
3. Who owns the model retraining cadence — data engineering or data science?
