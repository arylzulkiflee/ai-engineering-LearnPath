# LearnPath — V1 Architecture Decisions & Traceability

**Version:** 1.0  
**Status:** Proposed conceptual architecture

## 1. Architecture needs derived from requirements

| Requirement ID | What does the requirement need? | Architecture response |
|---|---|---|
| FR1 | Learner topic, experience level, and goal must be captured. | Input interface and input validation. |
| FR2 / FR3 | Relevant catalogue resources must be retrieved and matched to the learner's inputs. | Approved resource catalogue, retrieval component, and recommendation-matching/ranking capability. |
| FR4 / RAI1 | Recommendations need explanations and traceable sources. | Language-model generation grounded in retrieved records, plus source metadata/link propagation to the response. |
| FR5 / RAI2 | Empty or insufficient evidence must not lead to invented recommendations. | Evidence sufficiency check and no-match response path; generation is constrained to retrieved evidence. |
| NFR2 | Response time must be measured and remain within the target. | Evaluation/measurement point across the end-to-end request path. |
| RAI3 | Sensitive information should not be requested or exposed. | Data-minimizing input design and privacy checks before any external model/API request. |

## 2. V1 conceptual architecture

See week2_architecture_v1.svg for the diagram. The main flow is:

1. Learner submits topic, experience level, and learning objective through the application interface.
2. Input validation checks that required fields are present and appropriate.
3. The retrieval component searches the approved learning-resource catalogue.
4. A relevance-matching component ranks candidate resources against the learner's topic, level, and goal.
5. An evidence sufficiency check determines whether enough relevant catalogue information exists.
6. If evidence is sufficient, the language-model service generates a concise explanation grounded in the retrieved records. If evidence is insufficient, the application returns a no-match or limited-results response.
7. The response formatter returns resource title, provider, source link, and match explanation to the learner.
8. Evaluation checks measure relevance, grounding, response latency, and correct no-match handling.

The architecture labels components by capability rather than committing to a specific model, vector database, framework, or deployment platform.

## 3. Architecture Decision Records (ADR)

| Decision | Requirement / constraint | Alternatives considered | Selected option | Trade-off accepted |
|---|---|---|---|---|
| ADR1 — Use approved catalogue retrieval | FR2, FR4, RAI1; recommendations and explanations must be grounded in known free learning resources. | Generate from model knowledge only; unrestricted web search; retrieve from an approved catalogue. | Retrieve from an approved catalogue and pass retrieved records to the recommendation/generation flow. | Catalogue curation and updates require effort, and coverage is limited; in return, resource provenance and evidence control are improved. |
| ADR2 — Add evidence sufficiency and no-match handling | FR5 and RAI2; the system must not invent recommendations when evidence is missing. | Always generate an answer; rely only on a general prompt instruction; explicitly check retrieval evidence and return no-match when insufficient. | Add an explicit evidence sufficiency check and a no-match response path before explanation generation. | Some potentially useful results may be withheld (false no-match), and the flow has an additional processing step; this reduces unsupported recommendations. |
| ADR3 — Preserve source metadata in the response | FR4 and RAI1; learners need to verify recommendations. | Return generated prose only; return resource records with links and grounded explanations. | Keep title, provider, source URL, and retrieved evidence associated with each recommendation through response formatting. | The response structure is more complex and depends on complete catalogue metadata; learners gain traceability. |
| ADR4 — Minimize learner input data | RAI3; sensitive personal data is not needed for this task. | Collect detailed personal profiles; collect only topic, experience level, and learning goal. | Collect only the three recommendation inputs required by FR1. | Less personal information is available for personalization, but privacy exposure is reduced. |

## 4. Initial requirements traceability matrix

| Objective | Requirement | Evaluation criterion | Architecture element | Later evidence |
|---|---|---|---|---|
| O1 | FR2 / RAI1 | Retrieval coverage and source traceability | Approved catalogue, retrieval component, source metadata propagation | Test-query retrieval records; verify every result maps to a catalogue record and has a source link. |
| O2 | FR3 / NFR1 | Recommendation relevance and learner-profile match | Input interface, matching/ranking capability | Labelled test set with relevance scores for topic, level, and goal. |
| O1 / O3 | FR4 / RAI2 | Evidence support of generated explanations | Evidence sufficiency check, grounded generation, response formatter | Human review of generated explanations against retrieved evidence; count unsupported claims. |
| O3 | NFR2 | Response latency | End-to-end evaluation point | Latency log for at least 30 representative queries; calculate p90. |
| O2 | FR5 | Input and no-match behaviour | Input validation and no-match response path | Test missing inputs, out-of-domain queries, and empty retrieval results. |

## 5. Final architecture review

- Each major component supports one or more requirements listed above.
- The approved catalogue and retrieval path support evidence-based recommendations.
- Evidence sufficiency and no-match handling address unsupported recommendations.
- Source metadata is preserved so learners can verify the recommended resources.
- The design remains conceptual and does not prematurely select specific technologies.
- A simpler flow without retrieval, evidence checks, and source metadata would not adequately satisfy FR2, FR4, FR5, RAI1, and RAI2.

## Reflection

The most important trade-off is the decision to use an approved catalogue with an explicit evidence sufficiency check instead of allowing the AI model to answer from general knowledge. This decision is driven by FR2, FR4, FR5, RAI1, and RAI2. It adds catalogue-maintenance work and may return fewer recommendations when the catalogue has limited coverage, but it makes recommendations easier to verify and reduces the risk of fabricated resource information.
