# LearnPath: An Evidence-Based AI Learning Resource Recommendation System Using Retrieval-Augmented Generation

> Recommends free courses, tutorials, and documentation that match a learner's topic and level,
> grounded in open course outlines and explaining why each resource was picked.

## Problem Statement
Free learning resources for Python, data analysis, and introductory machine learning are scattered across different platforms, making it difficult for beginners to find materials that match their skill levels and learning goals. Manually searching and comparing resources can also be time-consuming.LearnPath uses Retrieval-Augmented Generation (RAG) to recommend suitable free learning resources from an approved catalogue based on learners' topics, experience levels, and goals. It also provides evidence-based explanations for each recommendation, helping learners find relevant and reliable learning materials more efficiently.

## Evidence of the Problem
- Interviews with 12 STEM students found that online learning resources were often disorganized and there were too many choices. Students struggled to assess trustworthiness and usefulness Nguyen et al. (2022)
- Identified information overload and difficulties independently finding relevant learning resources among students. (2025 Malaysian ODL study)


## Related Work / Existing Approaches
- [Coursera](https://www.coursera.org/) provides online courses, professional certificates, and learning programmes. It helps learners discover educational content based on their interests and learning goals. However, its recommendations mainly focus on courses available within its own platform rather than a broader catalogue of learning resources.
- [Class Central](https://www.classcentral.com/) collect courses from many providers with filters for level, duration, language, and free or paid, plus reviews. The learner still chooses filters and reads reviews, and results do not explain why a course fits.
- Retrieval-Augmented Generation (RAG) in Education Li et al. (2025) conducted a systematic survey of RAG applications in education, reviewing 51 studies. Their research found that RAG can support interactive learning systems, educational content generation, and personalized learning.

## Engineering Gap 
Lack of evidence-based recommendations: AI-generated recommendations may include irrelevant or unsupported information if they are not grounded in verified resource data.

## Project Objectives
1. Develop a RAG-based AI learning resource recommendation system that retrieves information from an approved catalogue of free learning resources.
2. Implement a personalized recommendation mechanism that matches learning resources to users' selected topics, experience levels, and learning objectives.
3. Evaluate the system's recommendation relevance, matching accuracy, and reliability of AI-generated explanations based on retrieved information.

## Target Users
Beginner to intermediate university students learning programming and data skills outside class.

## Data Sources
- Automate the Boring Stuff with Python (https://automatetheboringstuff.com/)
- Python Data Science Handbook (https://github.com/jakevdp/PythonDataScienceHandbook)
- scikit-learn documentation (https://scikit-learn.org)
  




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

### Week 3 Progress

| W3 | Tabular data engineering and repeatable preparation | Pipeline notebook, data inventory, pipeline decisions, prepared CSV, manifest, architecture diagram | In progress |
