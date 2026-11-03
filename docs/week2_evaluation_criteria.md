# LearnPath — Evaluation Criteria

**Version:** 1.1  
**Purpose:** Define how the project objectives, functional requirements, quality requirements, and Responsible AI requirements will be evaluated in V1.

> **Important:** All thresholds in this document are proposed acceptance targets. They are not measured results. Replace them with actual findings only after running the evaluation.

## 1. Evaluation objectives

The evaluation will determine whether LearnPath:

1. retrieves resources from the approved catalogue that are relevant to the learner's topic;
2. ranks resources in a way that reflects the learner's experience level and learning objective;
3. provides traceable source links and explanations supported by retrieved records;
4. handles missing inputs and insufficient evidence safely; and
5. responds reliably and within an acceptable time under the documented test conditions.

## 2. Evaluation plan and acceptance criteria

| Objective / requirement | Evaluation criterion | Capability category | Measurement procedure | Metric | Proposed target | Expected user outcome |
|---|---|---|---|---|---|---|
| O2 / FR3 / NFR1 | Relevance and learner-profile match | Domain-specific matching | Prepare at least 30 test scenarios across Python, data analysis, and introductory ML. Vary topic, experience level, and learning goal. Have a reviewer score each returned resource using the rubric in Section 3. | Relevant@K: number of returned resources scored 2 divided by total returned resources assessed. | ≥ 80% | Learners receive resources that fit their current topic and learning needs. |
| O1 / FR2 | Retrieval correctness | Retrieval capability | For each scenario, inspect the retrieved candidates and verify that every candidate exists in the approved catalogue and is relevant to the query. | Catalogue traceability rate: returned candidates matched to valid catalogue records / all returned candidates. | 100% | Learners are not shown resources invented by the generation model or absent from the approved catalogue. |
| O1 / FR4 / RAI1 | Source traceability | Transparency | Check each recommendation for a source URL and verify that it points to the intended resource. | Valid source-link rate: recommendations with a present, correct source link / all recommendations. | 100% | Learners can open and verify the original resource. |
| O3 / FR4 / RAI1 / RAI2 | Evidence support of explanations | Grounded generation | Review at least 30 generated explanations. Break each explanation into factual claims and label each claim as supported, unsupported, or contradicted by the retrieved record. | Supported-claim rate: supported factual claims / all factual claims assessed; separately count critical fabricated details. | ≥ 95% supported claims; 0 critical fabricated details | Learners receive explanations that do not misrepresent the recommended resources. |
| FR1 / FR5 | Input validation and no-match behaviour | Input handling / instruction following | Test complete inputs, missing fields, invalid values, out-of-domain topics, and queries with no matching catalogue entries. | Expected-behaviour rate: test cases producing the documented response / all test cases. | ≥ 95% | Learners receive clear guidance when information is missing or the catalogue cannot support a recommendation. |
| NFR2 | End-to-end response latency | Performance | Run at least 30 representative queries under the same documented environment. Measure from submission to complete recommendation response. | p90 end-to-end latency. | ≤ 5 seconds | Learners can use the system without excessive waiting. |
| NFR3 | Error handling and reliability | Reliability | Run normal, malformed-input, empty-retrieval, and simulated retrieval/API-failure tests. Record whether the system handles each case without crashing or misleading the user. | Expected handling rate: correctly handled cases / all reliability test cases. | ≥ 95% | The system fails clearly and safely rather than returning misleading content. |
| NFR4 | Catalogue maintainability | Maintainability | Add a correctly formatted test record and update an existing record. Verify that the system can retrieve the changes without redesigning the workflow. | Time to complete a successful catalogue update; note any code changes required. | ≤ 15 minutes per prepared record | Catalogue maintenance remains manageable as the resource collection grows. |
| RAI3 | Data minimization and privacy | Privacy | Review the user interface, application logs, and external model/API payloads. Confirm that only recommendation-relevant fields are collected or transmitted. | Count of unnecessary sensitive-data fields or transmissions. | 0 identified | Learners are not asked to disclose personal information unrelated to resource matching. |

## 3. Recommendation relevance rubric

Score each recommended resource against the learner's submitted topic, experience level, and learning objective.

| Score | Rating | Guidance |
|---|---|---|
| 2 | Relevant | The resource clearly supports the requested topic and is appropriate for the learner's level and stated goal. |
| 1 | Partially relevant | The topic is related, but the level or learning-goal match is incomplete, uncertain, or only indirectly supported by the catalogue metadata. |
| 0 | Not relevant | The resource does not meaningfully support the requested topic or is clearly unsuitable for the stated learning need. |

For the primary acceptance metric, count only score-2 resources as relevant. Report score-1 partial matches separately rather than combining them with fully relevant recommendations.

**Suggested test scenario fields:** scenario ID, topic, experience level, learning objective, expected topic tags, expected difficulty range, returned resource IDs, reviewer score, and reviewer comments.

## 4. Explanation-grounding review

For each selected recommendation:

1. Record the resource ID and the exact catalogue fields retrieved by the system.
2. Copy the generated explanation into the evaluation sheet.
3. Split the explanation into checkable factual claims (for example, topic coverage, prerequisites, difficulty, or learning outcomes).
4. Label each claim:
   - **Supported:** explicitly stated or reasonably entailed by the retrieved catalogue information.
   - **Unsupported:** not established by the retrieved information.
   - **Contradicted:** conflicts with the retrieved information.
5. Treat fabricated titles, providers, URLs, prices, prerequisites, or learning outcomes as critical errors.
6. Calculate the supported-claim rate and count critical errors separately.

A source URL being present does not automatically make the explanation grounded. The explanation itself must agree with the source information available to the system.

## 5. Test-set design

Use at least 30 predefined scenarios, distributed across the supported topics and input variations. Include:

- **Topic coverage:** Python programming, data analysis, and introductory machine learning.
- **Experience levels:** beginner and intermediate.
- **Learning goals:** understand fundamentals, practise a skill, complete a small project, or explore a concept.
- **Normal cases:** the catalogue contains one or more plausible matches.
- **Difficult cases:** a topic has limited catalogue coverage or resources with incomplete metadata.
- **No-match cases:** unsupported topics or queries for which the catalogue has no suitable resource.
- **Input-error cases:** missing topic, missing level, missing goal, or invalid values.

Keep the test scenarios fixed during a comparison so that changes to the retrieval or ranking approach can be evaluated consistently. Record the catalogue version used for each evaluation.

## 6. Reporting results

When testing is complete, report both the measured result and the proposed target. Do not state that a target has been achieved unless the test evidence supports it.

| Metric | Proposed target | Actual result | Evidence / notes |
|---|---:|---:|---|
| Relevant@K | ≥ 80% | To be measured | Reviewer scoring sheet |
| Catalogue traceability rate | 100% | To be measured | Retrieval logs and catalogue IDs |
| Valid source-link rate | 100% | To be measured | Link verification record |
| Supported-claim rate | ≥ 95% | To be measured | Explanation review sheet |
| Critical fabricated details | 0 | To be measured | Explanation review sheet |
| Expected input/no-match behaviour | ≥ 95% | To be measured | Functional test results |
| p90 end-to-end latency | ≤ 5 seconds | To be measured | Timestamped test log |
| Reliability test handling rate | ≥ 95% | To be measured | Error-path test results |
| Unnecessary sensitive-data fields/transmissions | 0 | To be measured | Privacy review |

## 7. Reflection

Recommendation relevance is the most important measure of whether LearnPath addresses its intended problem. A system may retrieve documents quickly and produce fluent explanations, but it will not be useful if the recommended resources do not match the learner's topic, level, and goal. For this reason, relevance should be judged against a predefined rubric rather than inferred from response quality alone.

Grounding is also essential. A valid source link is useful, but it does not prove that the generated explanation accurately describes the linked resource. Human review of factual claims is therefore necessary for the V1 evaluation. Finally, the results must be interpreted in light of the catalogue's limited size: weak coverage may reflect missing catalogue resources as well as retrieval or ranking limitations.
