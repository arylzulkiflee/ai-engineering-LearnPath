# Topic 4 Lab Report — Data Quality, Bias, and Responsible Data

Project: LearnPath: An Evidence-Based AI Learning Resource Recommendation System Using RAG

Audited output: data/pipeline_v1/tabular/prepared_table.csv (Topic 3 Part C, Pathway B)

Matching manifest: data/pipeline_v1/tabular/pipeline_manifest.json

Artefacts: https://github.com/arylzulkiflee/ai-engineering-LearnPath (data/ for the Week 3 input and Week 4 cleaned corpus, docs/ for validation, duplicate and coverage evidence, notebooks/week4_quality.ipynb for the checks)

Evidence note: Automated checks were rerun in week4_quality.ipynb (Colab). The notebook first confirmed that the audited file matches its manifest (20 records, SHA-256 match). Manual review covered all 20 records against the table's own fields only. Verification against original provider pages, live link status and licence terms was not done and is marked N.V.

## 1. Project and Data Overview

LearnPath recommends free learning resources to beginner-to-intermediate university students learning Python programming, data analysis and introductory machine learning. The audit covers the tabular catalogue that supports resource retrieval and learner-profile matching.

Requirements: FR2 (retrieve relevant candidates from the approved catalogue) and FR3 (organise/rank resources by topic, experience level and learning objective).

Scope: 20 records, 17 columns. This is the whole available dataset, so all checks ran on every record and no sampling was used.

Limitations: a small first-20 candidate catalogue. With n = 20, one record is 5%, so percentages are fragile and small count differences should be read as signals, not findings.

## 2. Five-Dimension Quality Scorecard

Ratings are on a 1–5 scale, or N.V. where evidence does not exist. They reflect the evidence below and should be adjusted by the author's own judgement.

| Dimension | Rating | Finding / evidence | Decision |
| --- | --- | --- | --- |
| Completeness | 4 | 0 blank or placeholder values (e.g. "N/A", "unknown") in the 11 key fields checked; 0 missing values across 20 rows × 17 columns in Topic 3. Field-level completeness is good, but coverage against scope is partial (see §4). | Pass (field level); coverage gaps noted |
| Consistency | 3 | Level, domain, access and language values all sit inside controlled vocabularies (0 violations). However, the rule found 1 level/prerequisite mismatch (LP0002), and manual review found 2 more cases the rule missed (LP0015, LP0020), plus several prerequisite entries that do not state a prerequisite (§3). date_verified is populated for all records while verification_status is needs_review for all, so the meaning of the date is unclear. | Review required |
| Accuracy | N.V. | Metadata, descriptions and level labels were not checked against authoritative provider pages. | Review required |
| Timeliness | 3 | date_verified = 2026-11-11 for all 20 records (0 beyond the 180-day warning window; 13 days old at time of review). verification_status = needs_review for 20/20 (100%). Live availability and content freshness N.V. | Warning |
| Uniqueness | 4 | 0 exact duplicates, 0 duplicate IDs, 0 duplicate resource_url, 0 near-duplicate titles (similarity > 0.85); 20 of 20 retained. Overlap in topic between different resources is not covered by these checks (§4). | Pass |

## 3. Validation Findings

| Rule | Condition / scope | Failures | Action |
| --- | --- | --- | --- |
| V1 Required-field completeness | 11 key fields non-blank and not placeholder, all 20 rows | 0 (0%) | Hard fail; repeat on updates |
| V2 Domain validity | primary_domain ∈ {python_programming, data_analysis, introductory_ml} | 0 invalid | Hard fail on invalid future values |
| V3 Level validity | difficulty_level ∈ {beginner, intermediate, advanced} | 0 invalid | Hard fail |
| V4 Description length | description ≥ 40 characters (observed range 61–103, mean 78.8) | 0 | Warning. The threshold sits below the observed minimum, so it detects only empty or fragment records. |
| V5 URL format and uniqueness | resource_url and source_page_url start with http(s); resource_url unique | 0 | Hard fail. Format validity does not prove the link is live. |
| V6 Date validity and age | date_verified parses; ≤ 180 days old | 0 unparseable, 0 stale | Warning |
| V7 Level vs prerequisite | beginner records whose prerequisites mention "experience" | 1 (5%): LP0002 | Manual review |
| V8 Verification status | verification_status = verified | 20 (100%) not verified | Warning; carried into mitigation |
| V9 Access type | access_type indicates free | 0 not free | Review; "free" is not a licence |

No missing, corrupted (replacement characters), noisy-whitespace, stale-by-date or malformed-URL cases were found. The inconsistent and uncertain cases are the level/prerequisite items below.

### Triage of flagged and manually observed cases

All decisions below are provisional until the provider pages are checked. No record was removed or quarantined.

| Record | Trigger | Observation | Interpretation | Decision |
| --- | --- | --- | --- | --- |
| LP0002 Google's Python Class | V7 | Labelled beginner; prerequisite reads "Some programming experience." | Uncertain: either a valid beginner-friendly course that expects light prior exposure, or a level mislabel | Retain; verify level against the provider page |
| LP0015 ML in Python with scikit-learn (MOOC) | Manual (not caught by V7) | Labelled beginner; prerequisite reads "Basic Python; NumPy and pandas recommended" | Uncertain: borderline beginner label | Retain; review level |
| LP0020 Introduction to Machine Learning | Manual | Labelled intermediate; "Course page lists no prerequisites." | Uncertain: level label has no supporting prerequisite evidence | Retain; review level |

What the rules cannot catch: V7 matches only the keyword "experience", so it missed LP0015. Several prerequisite entries do not state a prerequisite at all, for example LP0003–LP0005 ("Beginner-oriented."), LP0006 ("Introductory course."), LP0013 ("Review recommended prerequisites.") and LP0016 ("Check curriculum for setup prerequisites."). Whether a prerequisite is meaningful, and whether a level label is accurate, are accuracy and consistency questions that need a human with the source page.

## 4. Duplicate, Anomaly and Coverage Findings

Duplicates: exact-content hash, resource_url and near-title checks found no duplicates; provenance would have been logged in week4_duplicate_review.csv (empty result). These checks do not prove semantic uniqueness. Resources with overlapping topics, such as LP0007 and LP0011 (both pandas) or LP0014 and LP0015 (both scikit-learn), are different resources and were retained. Whether they add useful variety or redundancy is not established.

Anomalies: none found in description length (61–103 characters). All descriptions are single short sentences, which may give limited signal for later retrieval. This is a potential risk to test in Topic 7/8, not a measured defect.

### Coverage against application scope

| Category | Count | Share |
| --- | ---: | ---: |
| Python programming | 6 | 30% |
| Data analysis | 6 | 30% |
| Introductory ML | 8 | 40% |
| Beginner | 16 | 80% |
| Intermediate | 4 | 20% |
| Advanced | 0 | 0% |
| Course | 13 | 65% |
| Tutorial | 3 | 15% |
| Book | 2 | 10% |
| Documentation | 2 | 10% |
| Language: English | 20 | 100% |
| verification_status = needs_review | 20 | 100% |

### Domain × level

| Domain | Beginner | Intermediate |
| --- | ---: | ---: |
| Data analysis | 6 | 0 |
| Introductory ML | 5 | 3 |
| Python programming | 5 | 1 |

Provider spread is wide: 16 distinct providers, with the highest being Kaggle (3), Google (2) and Udacity (2), and no single-provider concentration signal. Coverage is limited to the three supported domains and is mostly beginner-level. There are no intermediate data-analysis resources and only one intermediate Python resource. Whether this is a genuine gap depends on how many intermediate learners the system is meant to serve; this is a signal to investigate, not proof of inadequate coverage. Advanced levels and other topics are outside the stated scope and not established.

## 5. Bias Assessment

These are potential representation or measurement risks identified from the data, not proven unfairness. No model-level fairness or accuracy claim is made; system-level testing is deferred to Topic 8.

| Risk | Evidence | Propagation path | Control / test |
| --- | --- | --- | --- |
| Representation (level coverage) | 16/20 beginner; 0/6 intermediate data-analysis; 1/6 intermediate Python | Source selection (first 20 candidates) → prepared catalogue → intermediate-level queries have few or no matching candidates → learner may receive beginner material or a weak match | Expand the catalogue deliberately by domain and level, document selection criteria, and return a no-match/limited-results response when evidence is insufficient. Topic 8: test level-conditioned queries, especially intermediate data analysis. |
| Representation (language) | 20/20 resources are English | Source selection → catalogue with no non-English options → learners who need other languages receive nothing suitable | Only a risk if non-English learners are in scope; scope to be confirmed. If so, add multilingual sources or state the English-only limitation. |
| Measurement (level label) | difficulty_level is a single label. The assignment method is not recorded in the audited table, and 3 records show label/prerequisite tension (§3). | Proxy label (not learner outcomes) → used by FR3 ranking → mismatched level recommendations | Verify against provider pages, record how the level was assigned, and add a level-consistency review step. Topic 8: check recommended level against learner level. |

## 6. Privacy, Rights and Governance

| Question | Evidence | Status | Action / owner |
| --- | --- | --- | --- |
| Source owner, version/date and provenance traceable? | provider, resource_url, source_page_url and date_verified populated for all 20 records; source and output SHA-256 fingerprints recorded in manifests (output hash re-verified in Week 4) | OK | — |
| Rights/licence terms known and suitable? | access_type = free for 20/20; the manifest states individual licence terms require review. Free access does not establish permission or an open licence. | To verify (N.V.) | Maintainer: check licence/terms for every provider before production use |
| Personal or sensitive data present? | Schema describes resource metadata only. Free-text fields (description, notes) were not specifically scanned for personal data. | No indication; not scanned | Scan free-text fields on update |
| Consent/collection basis | Not applicable to catalogue resource metadata in the way it is for personal data; source rights still require review | N/A | — |
| Update, superseded-version, retention and removal responsibility defined? | Not evidenced in the artefacts reviewed | Gap | Maintainer to assign owner and review cadence |

## 7. Mitigation Plan, Residual Risk and Impact on Requirements

| Priority | Issue | Evidence | Action | Owner / timing | Residual risk | Impacts |
| ---: | --- | --- | --- | --- | --- | --- |
| 1 | Unverified metadata and licences | 20/20 needs_review; licences N.V.; 3 level/prerequisite tensions | Verify title, provider, topic, level, prerequisites, learning outcomes, URL/access and licence/terms against provider pages; update verification_status and date_verified; clarify what date_verified records | Maintainer, before production | Provider content may have changed after verification | FR2, FR3, RAI1, RAI2 |
| 2 | Narrow level coverage | 80% beginner; 0 intermediate data analysis | Expand reviewed catalogue, especially intermediate; document selection criteria; rerun completeness/domain/uniqueness/URL-format checks | Before broad relevance claims | Small catalogue remains a limit | FR3, NFR1 |
| 3 | Prerequisite field quality | Several entries state no concrete prerequisite | Rewrite prerequisites as concrete statements; strengthen rule V7 beyond one keyword | Next catalogue revision | Rule-based checks still miss judgement cases | FR3 |
| 4 | Language and free-text governance | 20/20 English; free-text not scanned | Confirm language scope; scan free-text fields | Before release | Scope question may remain open | RAI1 |

Residual risks: small catalogue, uneven level coverage, unverified metadata and licences, and URL-format checks that do not establish live availability or content accuracy.

## 8. Quality-Gate Decision

**GO WITH CONDITIONS.**

Evidence: the reruns found 0 blank or placeholder values, duplicate rows/IDs/URLs, invalid domains or levels, malformed URLs, or stale dates; the cleaned corpus (20 records, none removed) reloaded consistently across the CSV, JSONL and PKL outputs; and the Week 3 file matched its manifest by record count and SHA-256.

Conditions:

1. Do not treat the catalogue as production-approved.
2. Verify metadata, availability and licence terms against provider pages and update the verification fields.
3. Resolve the level/prerequisite review cases (LP0002, LP0015, LP0020).
4. Expand and review coverage, especially intermediate data analysis, before broad relevance claims.
5. Re-review after catalogue changes, access changes or validation failures.

Re-review evidence: completed provider/rights review, updated verification fields, successful checks, and documented coverage across topics and levels.
