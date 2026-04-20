# Entity-Aware Progressive Query Rewriting for Lightweight Agentic RAG

This project explores a simple but practical idea: multi-hop RAG systems often fail not because retrieval is weak, but because the queries themselves are poorly formed after decomposition.

When a complex question is broken into sub-questions, important entities are often dropped (e.g., replacing “J. R. R. Tolkien” with “he”). This leads to weak retrieval and breaks the reasoning chain.

This repository implements a lightweight approach to fix that using entity-aware progressive query rewriting, designed to run entirely on free-tier Google Colab.

## Overview

The system compares three pipelines:

- **Vanilla RAG**  
  Uses the original question directly for retrieval.
- **Decomposition RAG**  
  Breaks the question into sub-questions and retrieves evidence for each.
- **Entity-Aware Progressive RAG (proposed)**  
  Rewrites sub-questions by restoring missing entities before retrieval and updates an entity memory across steps.

## Key Idea

Instead of treating retrieval as a one-shot step, this project models it as a progressive process:

Decompose question →  
Rewrite sub-question using known entities →  
Retrieve →  
Extract answer →  
Update entity memory →  
Repeat

This makes the pipeline behave more like a lightweight agent, without requiring large models or complex infrastructure.

## Dataset

**HotpotQA (distractor split)**  
A subset is used to keep the system runnable on free Colab.

## Tech Stack

- sentence-transformers (dense retrieval)
- rank-bm25 (sparse retrieval)
- transformers (decomposition + QA)
- spaCy (entity extraction)
- datasets (HotpotQA)
- scikit-learn (similarity + utilities)

All components are chosen to run within limited compute constraints.

## Results (summary)

| Method | EM | F1 | Supporting Title Recall |
| --- | --- | --- | --- |
| Vanilla RAG | 0.2417 | 0.2956 | 0.6542 |
| Decomposition RAG | 0.2417 | 0.2914 | 0.6583 |
| Entity-Aware RAG | 0.2417 | 0.2970 | 0.6583 |

## Observations

- Decomposition alone does not consistently improve answer quality.
- Entity-aware rewriting provides a small but consistent F1 improvement.
- Improvements are more noticeable in bridge-style multi-hop questions.
- Retrieval failures are often due to underspecified queries, not just ranking quality.

## Repository Structure

```text
.
├── notebook.ipynb
├── vanilla_results.csv
├── decomposition_results.csv
├── entity_aware_results.csv
├── summary_table.csv
└── README.md
```

## How to Run

1. Open the notebook in Google Colab.
2. Run all cells in order.
3. Outputs will include:
   - evaluation metrics
   - result CSV files
   - qualitative examples

No GPU is strictly required, but using one speeds up embedding and QA steps.

## Limitations

- Uses small models → limited reasoning capability
- Rule-based rewriting → not robust to all cases
- Evaluation on a subset of HotpotQA
- Does not match performance of large-scale RAG systems

The goal is not state-of-the-art performance, but to study query formulation effects in a controlled, low-resource setting.

## Future Work

- Learned query rewriting instead of rule-based
- Better decomposition strategies
- Stronger reranking
- Larger evaluation datasets
- Integration with stronger LLMs for generation

## Author

Angelo Nelson
