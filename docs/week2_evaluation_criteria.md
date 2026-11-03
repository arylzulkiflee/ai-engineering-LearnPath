# LearnPath — Evaluation Criteria

**Version:** 1.0  
**Purpose:** Define how the key requirements and project objectives will be evaluated in V1.

The thresholds below are proposed project acceptance targets, not measured results. They should be validated using the actual implementation and evaluation dataset.

| Objective / Requirement ID | Evaluation criterion | Capability bucket | Measurement method | Metric | Proposed usefulness threshold | User / business outcome |
|---|---|---|---|---|---|---|
| O2 / FR3 / NFR1 | Recommendation relevance and learner-profile match | Domain-specific capability | Prepare at least 30 test scenarios covering Python, data analysis, and introductory ML, with different levels and goals. Have a reviewer judge whether each returned resource fits the input using a relevance rubric. | Relevant recommendations / all evaluated recommendations | ≥ 80% | Learners spend less time comparing unsuitable materials and receive resources appropriate to their current level and goal. |
| O1 / FR2 / FR4 / RAI1 | Retrieval coverage and source traceability | Domain-specific capability | For each test query, inspect retrieved catalogue records and verify that every recommendation displays a valid source link and corresponds to a retrieved record. | Recommendation-to-record match rate; source-link presence rate | 100% match to retrieved records; 100% source-link presence | Learners can open and verify the original learning material instead of relying only on generated text. |
| O1 / FR4 / RAI2 | Evidence support of generated explanations | Generation capability | Review at least 30 explanations against the retrieved resource metadata/content. Mark each factual claim as supported, unsupported, or contradicted. | Supported-explanation rate; critical fabricated-detail count | ≥ 95% supported; 0 critical fabricated details | Learners receive understandable reasons for recommendations without being misled by invented resource details. |
| O1 / FR1 / FR5 | Input handling and no-match behaviour | Instruction-following capability | Test complete inputs, missing fields, out-of-domain topics, and queries with no matching catalogue entries. | Correct input/no-match handling rate | ≥ 95% | Learners receive clear guidance when the system needs more information or cannot find a suitable resource. |
| O3 / NFR2 | Response latency | Cost and latency | Run at least 30 representative queries and record end-to-end response time under the same test environment. | p90 response latency | ≤ 5 seconds | Learners can use the system interactively without excessive waiting. |

## Evaluation rubric for recommendation relevance

Score each recommended resource against the learner's input:

- **2 — Relevant:** Topic, level, and learning goal are all a good match.
- **1 — Partially relevant:** The topic matches, but level or goal alignment is unclear or incomplete.
- **0 — Not relevant:** The resource does not meaningfully match the requested topic.

For the primary relevance metric, count a recommendation as relevant when it receives a score of 2. Record partial matches separately so the results remain interpretable.

## Reflection

Recommendation relevance is the most important criterion for determining whether LearnPath is useful to its intended users. The main purpose of the system is to help beginner and intermediate university students find free learning materials that fit their topic, current experience level, and learning objectives. Even if the system responds quickly and produces clear explanations, it will not solve the original problem if the recommended resources are unsuitable.
