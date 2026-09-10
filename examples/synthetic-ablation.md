# Synthetic example: separating a module effect from a data effect

Everything below is fictional and written for this repository. It contains no real submission, external citation, or model benchmark result. The illustrative review is hand-written.

## Source material

Title: A Gated Module for Toy Classification

Claim in Section 1: “Our new gating module improves accuracy over the baseline.”

Section 3: Both systems use the same evaluation split of 2,000 examples. The paper reports one run per configuration. It does not report repeated-run variability. The baseline uses 10,000 training examples and the full system uses 20,000. Training settings other than the module and training set size are described as unchanged.

Table 2:

| Configuration | Gating module | Training examples | Accuracy |
|---|---|---|---|
| Baseline | No | 10,000 | 80.0 percent |
| Full system | Yes | 20,000 | 82.0 percent |

No module-free result with 20,000 training examples is supplied.

## Illustrative review excerpt

Scope. This review checks only the supplied fictional excerpt. No external literature search or novelty assessment was performed.

Summary. The paper studies a gating module for toy classification. It reports 82.0 percent accuracy for the full system and 80.0 percent for the baseline on the same evaluation split, with different training set sizes.

Strengths. Table 2 states the training set size for both configurations, which makes the difference in data use visible. Evaluating on a shared split also avoids changing that part of the comparison.

Weaknesses. The attribution of the improvement to the gating module is not isolated by Table 2. Both the presence of the module and the amount of training data change: the baseline uses 10,000 examples and the full system uses 20,000. The reported difference of 2.0 percentage points therefore measures the combined change. It does not establish the module's individual contribution, which is the main claim in Section 1. This is a limitation of the evidence, not proof that the module has no effect.

Questions. Was the baseline also evaluated with 20,000 training examples? If so, how did that configuration perform under the same training settings?

Suggestions. Prioritize a comparison that holds the training data fixed when assessing the module. Until that result is available, describe the gain as a property of the full system. Repeated runs would also help assess how much of the reported difference is stable.

## What this demonstrates

One substantive weakness is sufficient for this narrow excerpt. The review uses the supplied numbers, separates an unsupported attribution from a disproven method, and asks a question that could change the assessment. No external reference is needed, so it has no References section.
