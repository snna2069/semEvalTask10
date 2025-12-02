# SemEval-2026 Task 11 - Disentangling Content and Formal Reasoning in Scientific Statements

This repository contains our system submission for SemEval-2026 Task 11, which focuses on evaluating scientific statements through:

Subtask A — Validity Classification
Determine whether a scientific claim is logically valid.

Subtask B — Plausibility Prediction
Produce a short, plausible scientific explanation for a related hypothesis.

Our approach evaluates multiple transformer models for Subtask A and introduces a span-based plausibility prediction model for Subtask B.

Project Overview

Scientific claims in this task are short, dense, and often contain implicit assumptions or incomplete reasoning. Our system addresses these challenges with a two-phase pipeline:

Model Comparison for Validity (Subtask A)
Evaluate four pretrained transformer encoders:

XLM-RoBERTa-base

mDeBERTa-v3-base

distilXLM-RoBERTa

RemBERT

mDeBERTa-v3-base achieves the highest and most stable performance and becomes the backbone model.

Span-based Plausibility Model (Subtask B)
Extend DeBERTa-v3-base with a span-extraction head to generate concise scientific explanations.

System Architecture
1. Validity Classification (Subtask A)

Each input scientific claim is fed to one of the evaluated encoders using a standard classification head.

Key insights:

mDeBERTa-v3’s disentangled attention improves recognition of scientific relationships.

distilXLM-RoBERTa performs moderately but lacks depth for nuanced reasoning.

RemBERT struggles due to overcapacity vs dataset size.

2. Plausibility Prediction (Subtask B)

We reformulate the task as span extraction rather than classification:

[Scientific Claim] + [Related Hypothesis] → DeBERTa-v3 → (start, end) prediction


Unlike normal QA, the target span is a canonical scientific explanation and may not appear verbatim in the input, forcing the model to infer domain-consistent content.

Training details:

Max seq length: 256–384

LR: 2e-5

Batch size: 8

Cross-entropy over start/end logits

Gradient clipping + warmup for stability

## Results:
Subtask A — Validity Classification
| Model               | Acc       | Prec      | Rec       | F1        |
| ------------------- | --------- | --------- | --------- | --------- |
| XLM-RoBERTa-base    | 71.88     | 86.05     | 63.70     | 73.27     |
|**mDeBERTa-v3-base** | **83.33** | **90.38** | **81.03** | **85.45** |
| distilXLM-RoBERTa   | 79.17     | 79.19     | 79.17     | 79.16     |
| RemBERT             | 63.19     | 67.28     | 63.19     | 60.88     |


Winner: DeBERTa-v3-base → best accuracy, stability, and convergence.

Subtask B — Plausibility Prediction

| Model                            | F1        | Accuracy  |
| -------------------------------- | --------- | --------- |
|**mDeBERTa-v3-base (span-based)** | **77.76** | **77.78** |


## Error Analysis
1. Ambiguous or underspecified scientific language

Vague statements cause misclassification due to lack of explicit cues.

2. Generic predictions

The model sometimes defaults to high-frequency scientific explanations when input cues are weak.

3. Span boundary issues

Predicted spans may be slightly misaligned (too broad/narrow).


## Limitations

- Dataset size is moderate; larger domain corpora may help.

- Plausible spans are not always contiguous → generative models could improve results.

- Domain-style biases from scientific text may affect generalization.


Team Members:
Dnyaneshwari Rakshe, Sneha Nagaraju