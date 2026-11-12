# Week 3 Pipeline Decisions

## 1. Project and data requirement

LearnPath is an evidence-based AI learning resource recommendation system. It is intended to help beginner-to-intermediate university students discover learning materials for Python programming, data analysis, and introductory machine learning.

The pipeline prepares a structured resource catalogue that can later support retrieval and recommendation. The exact Topic 2 requirement IDs should be inserted here from the team's approved Topic 2 requirements document; they are not present in the currently available repository evidence.

## 2. Selected source and pathway

- Selected source: `learnpath_resources_first20.csv`
- Dataset: LearnPath Learning Resources
- Selected Topic 3 pathway: **Pathway B — tabular CSV data**
- Prepared output: `data/prepared_table.csv`
- Part C selected-pathway copy: `data/pipeline_v1/tabular/prepared_table.csv`
- Part C manifest: `data/pipeline_v1/tabular/pipeline_manifest.json`

The available notebook evidence shows 20 records and 17 columns. The catalogue covers Python programming (6 records), data analysis (6), and introductory machine learning (8).

The source-selection comparison and supporting evidence from Part B should be added from the team's completed comparison table. That comparison is not included in the currently available notebook/manifest evidence, so this document does not claim that other candidate sources were evaluated here.

## 3. Preparation configuration

The selected preparation operations are:

1. Ingest the source CSV with pandas.
2. Inspect the table shape, columns, data types, missing values, duplicate rows, duplicate resource IDs, and domain distribution.
3. Trim leading and trailing whitespace from text columns.
4. Remove exact duplicate rows.
5. Validate required columns, unique resource IDs, allowed domain values, and HTTP/HTTPS URL formats.
6. Export the prepared table to CSV.
7. Re-read the exported CSV and compare it with the cleaned in-memory table.

No imputation or outlier removal was applied. The inspection found zero missing values and zero exact duplicate rows, so those transformations were not justified by the observed data.

## 4. Pipeline stage contracts and failure responses

| Stage | Contract / expected condition | Failure response |
|---|---|---|
| Ingest | Source file exists and is permitted for use | Stop if the input is missing or permission to use it is unclear |
| Inspect | CSV is readable and has the expected tabular structure | Stop and report unreadable input or unexpected schema |
| Prepare | Selected cleaning operations complete and produce a table | Stop and report the transformation error |
| Validate output | Output exists, is non-empty, and passes schema/ID/domain/URL checks | Stop if output is missing, empty, or validation fails |
| Record lineage | Manifest links the selected source to the exact prepared output | Stop/report if source-output mapping or fingerprints are missing |

## 5. ETL/ELT and execution mode

**ETL is selected for this small batch pipeline.** The CSV is extracted, cleaned and validated before the prepared table is written for downstream use. This keeps the output in a consistent, validated form before it is consumed by later recommendation components.

**Batch processing is selected.** The current input is a finite CSV catalogue of 20 resource records. It does not require continuous event processing or real-time ingestion. The pipeline can be rerun when the catalogue is updated.

## 6. Validation evidence

The notebook reports:
- 20 input records and 17 columns.
- 0 missing values.
- 0 exact duplicate rows.
- 0 duplicate resource IDs.
- 0 invalid domain values.
- 0 invalid URL formats.
- 20 records after preparation.
- Exported table matches the cleaned table.

The repeatability test ran the pipeline twice. Both runs produced 20 records and the same SHA-256 fingerprint:

`0c3526dea06de667353357e1e33b3748f2bb3b6911d8273305c6a5a68f08ee50`

The Week 3 manifest records the source fingerprint, prepared output fingerprint, dimensions, and processing steps.

## 7. Accepted limitations and provenance

The dataset is a small first-20-record catalogue and may not represent the full range of available learning resources. All entries are currently marked `needs_review`. The `access_type` field labels the resources as free, but individual licence terms, current availability, metadata accuracy, and provider permissions still require review. The catalogue should not be described as fully verified or approved until those checks are completed.

The pipeline currently validates URL format, not whether each destination is live or whether the resource content is accurate. Those checks are outside the demonstrated validation results.

## 8. Items to complete from project evidence

- Add the exact Topic 2 requirement IDs and trace each one to the pipeline.
- Add the Part B source comparison and its evidence.
- The prepared CSV is committed at `data/prepared_table.csv` on `main`.
- A selected-pathway output copy and Part C manifest have now been added under `data/pipeline_v1/tabular/`. The manifest is based on the existing notebook execution evidence; rerun the pipeline from repository paths to create fresh end-to-end execution evidence.
