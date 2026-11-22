# Week 4 Data Quality Report

**Project:** LearnPath: An Evidence-Based AI Learning Resource Recommendation System Using RAG  
**Audited output:** `data/pipeline_v1/tabular/prepared_table.csv` (Topic 3 Part C, Pathway B)  
**Matching manifest:** `data/pipeline_v1/tabular/pipeline_manifest.json`

> **Evidence note:** This report uses recorded Topic 3 results. Checks were not independently rerun during drafting. Original-provider and licence verification items are marked N.V. (not verified).

## 1. Project and Data Overview

LearnPath recommends free learning resources to beginner-to-intermediate university students learning Python programming, data analysis and introductory machine learning. The audit focuses on the tabular catalogue used to support resource retrieval and learner-profile matching.

**Requirements:** FR2 (retrieve relevant candidates from the approved catalogue) and FR3 (organize/rank resources according to topic, experience level and learning objective).  
**Scope:** 20 records, 17 columns; the whole available dataset for recorded automated checks.  
**Selection method:** all available records. Manual provider-content verification was not completed.  
**Limitations:** a small first-20 candidate catalogue; no independent verification against provider pages during this audit.

## 2. Five-Dimension Quality Scorecard

| Dimension | Finding / evidence | Decision |
|---|---|---|
| Completeness | 0 missing values recorded across 20 rows / 17 columns | Pass |
| Consistency | 0 missing values recorded across 20 rows / 17 columns | Pass |
| Accuracy | Metadata and descriptions not checked against authoritative provider pages | Review required |
| Timeliness | `date_verified` recorded as 2026-11-11; live status/content freshness N.V. | Warning |
| Uniqueness | 0 duplicate rows and 0 duplicate IDs recorded; 20 retained | Pass |

## 3. Validation Findings

| Rule | Condition / scope | Recorded result | Action |
|---|---|---|---|
| V1: Required-field completeness | Required fields across 20 rows, 17 columns | Pass: 0 missing values | Repeat on updates |
| V2: Domain validity | `primary_domain` must be `python_programming`, `data_analysis`, or `introductory_ml`; all 20 rows | Pass: 0 missing values (as recorded in the lab report) | Reject or review invalid future values |

## 4. Duplicate, Anomaly and Coverage Findings

| Category | Count | Share |
|---|---:|---:|
| Python Programming | 6 | 30% |
| Data analysis | 6 | 30% |
| Introductory ML | 8 | 40% |
| Beginner | 16 | 80% |
| Intermediate | 4 | 20% |
| `needs_review` | 20 | 100% |

No duplicate rows or IDs were recorded. Coverage is limited to the three supported domains and is mostly beginner-level. Advanced levels and other topics are not established.

## 5. Bias Assessment

This is a plausible representation/coverage risk, not proven unfairness: selection of the first 20 candidate resources creates a small catalogue with uneven domain counts and 80% beginner entries. This may leave fewer options for intermediate learners or unsupported topics and lead to limited or less suitable recommendations.

**Control:** expand the catalogue deliberately across supported domains and levels, document selection criteria, and use no-match/limited-results responses where evidence is insufficient. No model-level fairness or accuracy claim is made.

## 6. Privacy, Rights and Governance

**Source:** `learnpath_resources_first20.csv`; source and output SHA-256 fingerprints are recorded in manifests.  
**Rights/licences:** N.V.; free access does not establish permission or an open licence.  
The inspected schema describes resource metadata and does not appear to contain learner personal data. Consent is not applicable to catalogue resource metadata in the same way as personal-data collection, but source rights still require review.

**Safeguard:** verify provider page, metadata, availability and licence/terms for every resource before treating the catalogue as approved.

## 7. Mitigation and Impact on Requirements

1. **Priority 1 — Provider and rights verification:** Verify title, provider, topic, level, prerequisites, learning outcomes, URL/access and licence/terms; update `verification_status` and `date_verified`. Maintainer action is required before production. Impacts FR2, FR3, RAI1 and RAI2.
2. **Priority 2 — Coverage expansion:** Expand reviewed catalogue coverage, especially intermediate resources, and document selection criteria; rerun completeness/domain/uniqueness/URL-format checks after updates. Impacts FR3 and NFR1.

**Residual risks:** small catalogue, uneven level coverage, unverified metadata/licences, and URL-format checks that do not establish live availability or content accuracy.

## 8. Quality-Gate Decision

**Final decision: GO WITH CONDITIONS.**

**Evidence:** recorded checks found 0 missing values, duplicate rows/IDs, invalid domains or invalid URL formats; exported output matched the cleaned dataframe; repeatability produced the same 20-record fingerprint twice.

**Conditions:**
- Do not treat the catalogue as production-approved.
- Verify metadata, availability and licence terms.
- Expand/review coverage before making broad relevance claims.
- Re-review after catalogue changes, access changes or validation failures.

**Re-review evidence required:** completed provider/rights review, updated verification fields, successful checks, and documented coverage across topics and levels.
