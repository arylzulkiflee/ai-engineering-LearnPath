# Requirements Specification — LearnPath

**Project:** LearnPath: An Evidence-Based AI Learning Resource Recommendation System Using Retrieval-Augmented Generation (RAG)  
**Version:** 1.1  
**Status:** Proposed V1 requirements

## 1. Project framing and application scope

### 1.1 Problem statement

Free learning resources for Python programming, data analysis, and introductory machine learning are available across books, documentation websites, tutorials, and online learning platforms. However, learners must often search across multiple sources and decide for themselves whether a resource matches their current knowledge, learning goal, and preferred level of difficulty. This creates an additional resource-selection task before learning can begin.

Research on how community-college STEM students evaluate online learning materials highlights that learners face multiple search results and must apply criteria to sort and select resources (Nguyen et al., 2022). In a Malaysian open and distance learning (ODL) context, Xue et al. (2025) report challenges that include information overload and difficulty independently finding relevant resources. These studies provide context for the resource-discovery problem, although they do not establish that every university learner experiences the same difficulty.

LearnPath will address this problem within a deliberately limited domain by helping learners discover approved free resources based on their selected topic, experience level, and learning objective.

### 1.2 Evidence and existing approaches

| Evidence / approach | What it demonstrates | Limitation relevant to LearnPath |
|---|---|---|
| Nguyen et al. (2022), study of how STEM students evaluate online resources | Students must evaluate and select among multiple online resources using criteria relevant to their learning. | The study examines resource evaluation behaviour; it does not test LearnPath or prove that a particular recommendation algorithm will improve learning outcomes. |
| Xue et al. (2025), Malaysian ODL case study | Participants described information overload and difficulty independently searching for relevant learning resources. | The qualitative study involved a small, single-institution context; its findings should not be generalized to all learners. |
| Coursera | Provides structured online courses, programmes, and learning content within its platform. | LearnPath's V1 aims to recommend selected free resources across a small approved catalogue, rather than only help users browse one course platform. |
| Class Central | Aggregates courses from multiple providers and offers discovery and filtering features. | Learners may still need to compare options and judge how well an item matches their particular topic, level, and goal. |
| Li et al. (2025), systematic survey of RAG in education | Reviews 51 studies and describes educational RAG applications, technical approaches, and continuing challenges such as hallucination and knowledge freshness. | The survey establishes RAG as a relevant design approach, not evidence that LearnPath itself is accurate or effective. |

### 1.3 Engineering gap

Existing course platforms and aggregators help learners discover learning content, while general-purpose generative AI can produce explanations and suggestions. However, a recommendation workflow for this project needs to do more than produce plausible-sounding suggestions. It must connect each suggested resource to an approved catalogue record, compare the record with the learner's topic, level, and goal, preserve source details, and avoid making claims that the available evidence cannot support.

The engineering gap addressed by LearnPath is therefore the integration of **catalogue-based retrieval, learner-profile matching, evidence-aware ranking, and traceable explanations** in one small, testable system. This is a project-specific design gap, not a claim that no existing product offers similar functionality.

### 1.4 Project objectives

- **O1:** Develop a RAG-based learning-resource recommendation system that retrieves candidate resources from an approved catalogue of free learning materials.
- **O2:** Implement a matching and ranking mechanism that considers the learner's selected topic, experience level, and learning objective.
- **O3:** Generate explanations grounded in retrieved catalogue information and provide source links so learners can verify recommendations.
- **O4:** Evaluate recommendation relevance, retrieval traceability, response performance, and the factual support of generated explanations.

### 1.5 Application scope

- **Intended users:** Beginner to intermediate university students who are learning programming and data-related topics independently.
- **Supported topics in V1:** Python programming, data analysis, and introductory machine learning.
- **Learner inputs:** Topic, experience level, and learning objective.
- **System outputs:** A ranked or organized list of matching resources, resource title, provider/source, source link, and a concise explanation of the match.
- **Initial catalogue examples:** Automate the Boring Stuff with Python, Python Data Science Handbook, and scikit-learn documentation. Each entry should have verified metadata such as title, provider, URL, topic tags, level, learning outcomes, and access/price status.
- **Operating context:** A student-facing application for self-directed learning.
- **Key constraints:** Recommendation quality depends on catalogue coverage and metadata quality. Resource links, access conditions, and content may change and should be checked during catalogue maintenance.
- **Out of scope for V1:** Paid-resource recommendations; unrestricted live web search; enrolment or account creation on third-party platforms; learner grading; guarantees of learning success; and recommendations unsupported by the approved catalogue.
- **Assumption to validate:** The selected catalogue contains enough variety to support meaningful recommendations across the three stated topics and multiple experience levels.

## 2. Functional requirements

| ID | Requirement — The system SHALL | Acceptance evidence | Out of scope / boundary |
|---|---|---|---|
| FR1 | Accept a learner's topic, experience level, and learning objective as recommendation inputs. | A valid input is captured and passed to the recommendation workflow. | The system shall not require sensitive personal information. |
| FR2 | Retrieve candidate resources from the approved catalogue that are relevant to the selected topic. | Each returned candidate can be traced to a catalogue record. | V1 shall not recommend items outside the approved catalogue. |
| FR3 | Rank or organize retrieved resources according to their match with the learner's topic, experience level, and learning objective. | Ranking results can be compared with a human-labelled relevance judgement. | The system shall not claim suitability when the record does not support the match. |
| FR4 | Present each recommendation with its title, provider/source, resource link, and an explanation based on retrieved information. | The response displays the required fields and the explanation can be checked against its source record. | The system shall not invent titles, providers, URLs, prerequisites, prices, or learning outcomes. |
| FR5 | Indicate when the catalogue contains insufficient relevant evidence and return a clear no-match or limited-results response. | Out-of-domain and no-result test cases produce an explicit no-match/limited-results response. | The system shall not generate a confident recommendation when supporting evidence is unavailable. |

## 3. Non-functional requirements

The values below are **proposed acceptance targets**, not achieved results. They must be tested against the implemented system and reported as measured results only after testing.

| ID | Quality attribute | Requirement | Verification method | Metric and proposed target |
|---|---|---|---|---|
| NFR1 | Recommendation relevance | Recommendations shall be relevant to the submitted topic, level, and goal. | Evaluate at least 30 predefined learner scenarios. Have a reviewer score each returned resource using the rubric in the Evaluation Criteria document. | Relevant@K: at least 80% of returned resources receive the highest relevance score. Report partial matches separately. |
| NFR2 | Performance | The system shall return recommendations within an acceptable interactive time under the stated test environment. | Run at least 30 representative queries and record end-to-end latency, including retrieval and generation. | p90 end-to-end latency ≤ 5 seconds. Record hardware, network, model/API, and test conditions. |
| NFR3 | Reliability | The system shall handle valid inputs, empty retrieval results, malformed inputs, and simulated retrieval failures without crashing or returning misleading recommendations. | Run a documented set of normal and error-path tests. | At least 95% of test cases handled as expected. Report the numerator and denominator. |
| NFR4 | Maintainability | A developer shall be able to add or update a catalogue record without redesigning the recommendation workflow. | Add or update one test resource and verify that the application can retrieve it without unrelated code changes. | Successful update within 15 minutes for a prepared, correctly formatted record. |

## 4. Responsible AI requirements

| ID | Risk / principle | Requirement | Verification method | Proposed target |
|---|---|---|---|---|
| RAI1 | Transparency and grounding | Every recommendation shall include a traceable source link, and its explanation shall be supported by retrieved catalogue information. | Inspect at least 30 generated recommendations. Check source-link presence and assess factual claims against the corresponding catalogue record. | 100% source-link presence; at least 95% of assessed factual claims supported. |
| RAI2 | Hallucination and reliability | The system shall not fabricate resource titles, providers, URLs, prices, prerequisites, or learning outcomes. It shall communicate when evidence is insufficient. | Test nonexistent topics/resources and catalogue records with missing fields; compare output claims with source records. | Zero critical fabricated resource details in the test set. |
| RAI3 | Privacy and data minimization | The system shall collect only the information required to make recommendations and shall not request sensitive personal information. | Review input fields, application logs, and any data sent to external model/API services. | No unnecessary sensitive-data fields or transmissions identified. |

## 5. Key assumptions, dependencies, and risks

- **Catalogue completeness:** A small catalogue may not cover every topic, level, or goal. The system must support limited-results and no-match outcomes.
- **Metadata quality:** Ranking depends on accurate topic tags, difficulty levels, learning outcomes, and access information. Records should be checked before inclusion.
- **Source freshness:** External pages and access conditions can change. Catalogue entries need a documented review/update process.
- **Generation dependency:** If an external language model is used, availability, latency, cost, and data-handling terms may affect the system.
- **Evaluation subjectivity:** Relevance judgements can differ between reviewers. Use a written rubric and, where possible, a second reviewer for a subset of cases.
- **RAG is not a guarantee:** Retrieval can reduce dependence on unsupported model knowledge, but incorrect, incomplete, or poorly matched retrieved records can still produce weak recommendations.

## 6. Reflection

The most challenging Responsible AI requirement to evaluate is RAI1. Checking whether a source link is present is straightforward, but checking whether an explanation is genuinely supported requires reviewing the factual claims against the retrieved record. A link by itself does not prove that the explanation is accurate. LearnPath therefore needs a small, manually reviewed evaluation set and a consistent claim-support rubric. Another important limitation is catalogue coverage: a system can be transparent and technically reliable while still being unhelpful if its approved catalogue does not contain suitable resources.

## References

Li, Z., Wang, Z., Wang, W., Hung, K., Xie, H., & Wang, F. L. (2025). Retrieval-augmented generation for educational application: A systematic survey. *Computers and Education: Artificial Intelligence, 8*, Article 100417. https://doi.org/10.1016/j.caeai.2025.100417

Nguyen, T.-H., Gutierrez, R. A. E., & Smith, J. (2022). Evaluating on-line resources: How community college students in STEM sort and select material on the Internet. *Community College Review, 50*(3), 292–315. https://doi.org/10.1177/00915521221087286

Xue, Y., Khalid, F., & Karim, A. A. (2025). Cognitive and behavioral engagement challenges in open and distance learning and potential solutions from artificial intelligence. *Frontiers in Education, 10*, Article 1610148. https://doi.org/10.3389/feduc.2025.1610148

## Useful project/data-source links

- Coursera: https://www.coursera.org/
- Class Central: https://www.classcentral.com/
- Automate the Boring Stuff with Python: https://automatetheboringstuff.com/
- Python Data Science Handbook: https://jakevdp.github.io/PythonDataScienceHandbook/
- scikit-learn documentation: https://scikit-learn.org/stable/
