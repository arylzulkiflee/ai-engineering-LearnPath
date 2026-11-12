# Week 3 Data Inventory

## Dataset overview

| Item | Description |
|---|---|
| Dataset | LearnPath Learning Resources |
| Source file | `learnpath_resources_first20.csv` |
| Prepared output | `prepared_table.csv` |
| Selected pathway | Tabular CSV data (Topic 3 Pathway B) |
| Records | 20 |
| Columns | 17 |
| Domain coverage | Python programming: 6; Data analysis: 6; Introductory ML: 8 |
| Access classification | Records are labelled free; individual licence terms still require review |
| Verification status | `needs_review` |

## Schema

The dataset contains these 17 fields:

1. `resource_id`
2. `title`
3. `description`
4. `primary_domain`
5. `subtopics`
6. `resource_type`
7. `difficulty_level`
8. `learning_objectives`
9. `prerequisites`
10. `provider`
11. `resource_url`
12. `access_type`
13. `language`
14. `source_page_url`
15. `date_verified`
16. `verification_status`
17. `notes`

## Quality inspection and preparation

| Check / operation | Observed result |
|---|---:|
| Missing values before preparation | 0 |
| Exact duplicate rows | 0 |
| Duplicate resource IDs | 0 |
| Invalid primary-domain values | 0 |
| Invalid URL formats | 0 |
| Rows removed during exact-deduplication | 0 |
| Prepared records | 20 |

Preparation operations:
- Read the source CSV.
- Trim leading and trailing whitespace from text columns.
- Remove exact duplicate rows.
- Validate required fields, unique resource IDs, allowed domain values, and HTTP/HTTPS URL formats.
- Export the prepared table as CSV.
- Re-read the exported file and compare it with the cleaned table.

No missing-value imputation or outlier removal was applied because the inspection found no missing values and the selected preparation method did not require those transformations.

## Provenance and limitations

The Week 3 manifest records the source and prepared-file SHA-256 fingerprints, record count, column count, and processing steps. The prepared output fingerprint recorded in the manifest is:

`0c3526dea06de667353357e1e33b3748f2bb3b6911d8273305c6a5a68f08ee50`

The notebook's repeatability test produced the same fingerprint in both runs.

All resource entries remain marked `needs_review`. A resource being labelled free does not establish that every item is openly licensed or that its metadata and current availability have been independently verified. Review provider terms and confirm each resource before treating the catalogue as approved for production use.

## Repository note

The expected repository location for the prepared output is `data/processed/prepared_table.csv`. Confirm that this file is committed to the repository's `main` branch. The inventory describes the output evidenced by the notebook and manifest; repository visibility must be checked separately.
