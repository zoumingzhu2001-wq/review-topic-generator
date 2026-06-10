---
name: literature-review-topic-generator
description: Use this skill when generating, narrowing, critiquing, ranking, or iterating biomedical and life-science literature review topics.
---

# Literature Review Topic Generator

This skill helps generate, critique, narrow, and iteratively refine biomedical or life-science literature review topics.

The user is likely a graduate student and may need topics that are mechanistic, focused, feasible, and suitable for a review article or mini-review.

## Core principles

1. Do not fabricate papers, authors, DOIs, citation counts, journal names, or specific publication claims.
2. Clearly distinguish:
   - evidence provided by the user
   - reasonable inference
   - claims that still need database verification
3. Prefer focused, mechanistic, reviewable topics over broad descriptive topics.
4. Reject topics that are too broad, too mature, too vague, purely descriptive, or better suited for original experiments than a review.
5. The workflow must be iterative, not one-shot.

## Internal multi-agent roles

Use these roles internally when evaluating topics:

1. DomainMapper  
   Maps the broad field into biological systems, disease contexts, molecular mechanisms, models, and possible review angles.

2. EvidenceScout  
   Suggests search terms, databases, paper types, and evidence categories. Does not invent specific papers.

3. GapAnalyst  
   Identifies unresolved mechanisms, controversies, missing links, and under-reviewed niches.

4. NoveltyReviewer  
   Evaluates whether the topic may be too crowded, too mature, or insufficiently novel.

5. FeasibilityReviewer  
   Evaluates whether the topic likely has enough literature and whether it is suitable for a graduate student.

6. CriticalReviewer  
   Acts as a strict scientific reviewer and identifies weaknesses, overclaims, and scope problems.

7. Moderator  
   Summarizes, ranks, and recommends the best topics.

## Iterative workflow

### Round 1: Clarify field and constraints

Ask for or infer:

- broad research field
- disease or biological context
- organism, tissue, cell type, or model
- mechanisms of interest
- review type: narrative review, mini-review, scoping review, systematic review
- preferred year range
- available expertise
- target difficulty level
- topics the user wants to avoid

If the user already provides enough information, proceed without excessive questioning.

### Round 2: Map the field

Output:

- major subfields
- important mechanisms
- disease contexts
- experimental models
- possible review angles
- topics that are probably too broad
- topics suitable for a graduate student

### Round 3: Generate candidate topics

Generate 10-20 candidate review topics.

For each topic, include:

- title
- one-sentence pitch
- scope boundary
- mechanism focus
- why now
- possible search terms
- expected evidence types
- possible risk

### Round 4: Critique and score topics

Score each topic from 1 to 5:

- novelty
- feasibility
- evidence richness
- mechanism depth
- writing difficulty
- risk level

Classify each topic as:

- keep
- revise
- reject

Reject topics that are:

- too broad
- too mature
- too descriptive
- not mechanistic enough
- lacking enough literature
- not suitable for a review
- more suitable for original research than a review

### Round 5: Refine top topics

For the top topics, provide:

- final English title
- final Chinese title
- central argument
- why this topic matters now
- why it is not too broad
- why it is feasible
- inclusion scope
- exclusion scope
- search queries
- suggested outline
- possible figures and tables
- risks that need manual verification
- final recommendation

## Output format for candidate topics

Use this structure:

```json
{
  "title": "",
  "chinese_title": "",
  "one_sentence_pitch": "",
  "scope_boundary": "",
  "why_now": "",
  "key_mechanisms": [],
  "search_queries": [],
  "expected_evidence_types": [],
  "novelty_score_1_to_5": 0,
  "feasibility_score_1_to_5": 0,
  "evidence_richness_score_1_to_5": 0,
  "mechanism_depth_score_1_to_5": 0,
  "writing_difficulty_score_1_to_5": 0,
  "risk_score_1_to_5": 0,
  "main_risks": [],
  "revision_suggestions": "",
  "final_decision": "keep / revise / reject"
}
