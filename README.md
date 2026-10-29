# LearnPath: Free Learning Resource Finder

> Recommends free courses, tutorials, and documentation that match a learner's topic and level,
> grounded in open course outlines and explaining why each resource was picked.

## Problem Statement
Free learning resources are spread across many platforms and vary in level, quality, and how up to date they are. Beginners spend a long time searching and often cannot tell whether a resource suits their level. LearnPath helps learners find suitable free resources for a topic such as "beginner Python for data analysis" and explains each recommendation.

## Evidence of the Problem
- Class Central's catalogue alone lists over 100,000 online courses from many providers, so choosing among them is hard (Class Central Help Center, accessed 29 Oct 2026).
- Coursera reports that nearly two-thirds of its learners are not sure which skills they need to reach their goals (Coursera Blog, "From Catalog to Compass", accessed 29 Oct 2026). This is a company statement, not independent research.


## Initial Related Work / Existing Approaches
- **Platform search and recommendations (Coursera, edX):** recommend within their own catalogue, and on edX free access is limited while graded work and certificates require payment. They do not compare free resources across providers.
- **Course aggregators (Class Central):** collect courses from many providers with filters for level, duration, language, and free or paid, plus reviews. The learner still chooses filters and reads reviews, and results do not explain why a course fits.
- **Learning roadmaps (roadmap.sh):** community-curated role and skill paths with linked resources and progress tracking. Each roadmap is a fixed path, not tailored to the learner's current level, and linked resources are not matched or explained.

## Engineering Gap / Limitation
Existing tools either stay within one platform, depend on the learner's own filtering and judgement, or follow a fixed path. There is no simple way to get recommendations across free resources that are grounded in course outlines and come with an explanation of why each one matches the learner's topic and level.

## Project Objectives
- O1: Design a document-grounded assistant that recommends free learning resources by topic and level.
- O2: Develop the solution with retrieval, an explanation for each recommendation, and guardrails against unsupported or invented resources.
- O3: Evaluate relevance, level match, groundedness, latency, and safety.

## Target Users
Beginner to intermediate university students learning programming and data skills outside class.

## Data Sources
- MIT OpenCourseWare course outlines and syllabi (ocw.mit.edu)
- freeCodeCamp curriculum (github.com/freeCodeCamp/freeCodeCamp)
- Harvard CS50 course outlines (cs50.harvard.edu)

Licences and terms are checked before use. Where redistribution is not allowed, the repo links to the sources instead of storing copies.

## Responsible AI Risks
- **Hallucination:** the assistant may invent a course or URL, so every recommendation must come from a retrieved document.
- **Outdated or low-quality resources:** content may no longer be maintained, so each result shows its source and date where available.
- **Bias:** results may favour English-language or well-known platforms.

## Repository Structure
| Folder | Purpose |
|--------|---------|
| docs/ | Project scope and design documents |
| data/ | Small self-written files such as test questions |
| src/ | Application code |
| notebooks/ | Experiments |
| evaluation/ | Test sets and results |
| prompts/ | Prompt versions |

## Weekly Progress
| Week | Focus | Deliverable | Status |
|------|-------|-------------|--------|
| W1 | System map + project scope | README + docs | ✅ |
