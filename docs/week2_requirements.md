# Requirements Specification — LearnPath

**Project:** LearnPath: An Evidence-Based AI Learning Resource Recommendation System Using Retrieval-Augmented Generation  
**Version:** 1.0  
**Status:** Proposed V1 requirements

## Section 0 — Project Framing & Application Scope

### Problem Evidence
- Interviews with 12 STEM students reported disorganized online learning resources, too many choices, and difficulty assessing trustworthiness and usefulness (Nguyen et al., 2022), as summarized in the project README.
- A 2025 Malaysian open and distance learning (ODL) study is cited in the README as identifying information overload and difficulty independently finding relevant learning resources among students.

*Evidence note:* Add full bibliographic details and links for both studies before final submission. The README currently provides incomplete citation metadata.

### Existing Approaches
1. **Coursera** — provides online courses, professional certificates, and learning programmes. Its recommendations primarily concern content within its own platform.
2. **Class Central** — aggregates courses from multiple providers and offers filters such as level, duration, language, price, and reviews. Learners still need to compare results themselves.
3. **RAG in Education (Li et al., 2025)** — a systematic survey of 51 studies describing educational uses of RAG, including interactive learning systems, educational content generation, and personalized learning.

### Project / Engineering Gap
Learners still need to compare resources across sources, while generic AI recommendations may be irrelevant or unsupported if they are not grounded in verified resource information. LearnPath addresses this practical gap by retrieving from an approved resource catalogue and explaining recommendations using retrieved evidence.

### Project Objectives
- **O1:** Develop a RAG-based AI learning resource recommendation system that retrieves information from an approved catalogue of free learning resources.
- **O2:** Implement personalized recommendations that match resources to users' selected topics, experience levels, and learning objectives.
- **O3:** Evaluate recommendation relevance, matching accuracy, and the reliability of AI-generated explanations based on retrieved information.

### Application Scope
- **Intended users:** Beginner to intermediate university students learning programming and data skills outside class.
- **Supported tasks:** Accept a learning topic, experience level, and learning goal; retrieve relevant catalogue entries; rank and present suitable free resources; explain why each resource matches; show source details and links.
- **Information / knowledge domain:** Free learning materials for Python programming, data analysis, and introductory machine learning. Initial approved sources listed in the README are Automate the Boring Stuff with Python, Python Data Science Handbook, and scikit-learn documentation.
- **Operating context:** A student-facing application used by university learners for self-directed learning.
- **Important constraints:** V1 depends on the coverage, accuracy, freshness, and metadata quality of a small approved catalogue. Recommendations are limited to free resources represented in that catalogue. Resource availability and terms may change.
- **Explicit out-of-scope tasks:** Paid-course recommendations; unrestricted web search; enrolment or account creation on third-party platforms; grading or assessment of learners; guaranteeing learning outcomes; and presenting generated claims that cannot be supported by retrieved catalogue information.
- **Evaluation focus:** Determine whether recommendations are relevant to the learner's topic, level, and goal, and whether each explanation is supported by the retrieved resource information.

## Section 1 — Functional Requirements

| ID | Requirement — The system SHALL | Scope boundary — SHALL NOT |
|---|---|---|
| FR1 | Accept a learner's topic, experience level, and learning objective as recommendation inputs. | Require sensitive personal information or collect unrelated profile data. |
| FR2 | Retrieve candidate learning resources from the approved catalogue that are relevant to the learner's selected topic. | Recommend resources outside the approved catalogue in V1. |
| FR3 | Rank or organize retrieved resources according to their match with the learner's topic, experience level, and learning objective. | Claim that a resource is suitable when its catalogue metadata does not support the match. |
| FR4 | Present each recommendation with its title, provider/source, resource link, and an explanation of why it matches the learner's inputs, using retrieved information. | Invent resource details, URLs, prerequisites, prices, or course outcomes. |
| FR5 | Indicate when the catalogue does not contain enough relevant information and provide a clear no-match or limited-results response. | Generate a confident recommendation when supporting catalogue evidence is unavailable. |

## Section 2 — Non-Functional Requirements

The following are proposed V1 acceptance targets. They should be checked against the actual implementation and adjusted if the project evaluation shows they are not feasible.

| ID | Quality attribute | Requirement | Measurement method | Metric | Proposed threshold |
|---|---|---|---|---|---|
| NFR1 | Relevance / correctness | Recommendations shall match the submitted topic, level, and goal. | Evaluate a labelled set of at least 30 learner-input scenarios; manually judge each returned resource against a defined relevance rubric. | Relevant recommendations / all evaluated recommendations | ≥ 80% |
| NFR2 | Performance | The system shall return a recommendation response within an acceptable interactive time. | Run at least 30 representative queries and record end-to-end response time. | p90 response latency | ≤ 5 seconds |
| NFR3 | Reliability | The system shall handle valid inputs and catalogue retrieval failures without crashing or returning a misleading recommendation. | Run normal, empty-result, malformed-input, and simulated retrieval-failure test cases. | Successful handling rate | ≥ 95% of test cases |
| NFR4 | Maintainability | An administrator/developer shall be able to add or update a catalogue resource without redesigning the recommendation workflow. | Perform a controlled add/update test and record completion time and whether unrelated components require changes. | Successful update time | ≤ 15 minutes per resource |

## Section 3 — Responsible AI Requirements

| ID | Responsible AI risk | Requirement | Measurement method | Metric | Proposed threshold |
|---|---|---|---|---|---|
| RAI1 | Transparency / grounding | The system shall provide a traceable source link for every recommended resource and ground its explanation in retrieved catalogue information. | Inspect at least 30 generated recommendations; verify link presence and whether each explanation is supported by the corresponding retrieved record. | Source-link presence; explanation support rate | 100% link presence; ≥ 95% supported explanations |
| RAI2 | Reliability / hallucination | The system shall not invent resource titles, providers, URLs, prices, prerequisites, or learning outcomes; it shall state when evidence is insufficient. | Test with queries for nonexistent resources and catalogue entries with missing fields; compare output with the catalogue. | Unsupported-claim rate | 0% critical fabricated resource details |
| RAI3 | Privacy | The system shall not request or expose sensitive personal information for resource recommendations. | Review input fields, logs, and any external model/API payloads using privacy test cases. | Unnecessary sensitive-data fields or transmissions | 0 |

## Reflection
The most difficult Responsible AI requirement to measure technically is RAI1, because checking that a source link exists is straightforward, but determining whether a generated explanation is genuinely supported by the retrieved resource information requires a human-reviewed test set and a consistent assessment rubric. This is important because a link alone does not prove that the explanation is accurate.
