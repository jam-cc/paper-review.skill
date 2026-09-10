# Review prose examples

These hand-written examples are fully fictional. Names, tables, and numbers below were created for style guidance, not drawn from submissions or measured model output. Do not reuse their facts in a real review. A longer example with source material is available in [examples/synthetic-ablation.md](../examples/synthetic-ablation.md).

## A concern tied to a specific claim

Fictional source: Table 2 reports a baseline trained on 10,000 examples at 80.0 percent accuracy and a gated model trained on 20,000 examples at 82.0 percent. Section 1 attributes the gain to the gate.

Review paragraph:

The attribution of the accuracy gain to the gating module is not isolated by Table 2. Both the module and training set size change between the two configurations. The difference of 2.0 percentage points therefore measures the combined effect of these changes, rather than the module's individual contribution. This limits the evidence for the claim in Section 1, although it does not show that the module is ineffective.

The paragraph identifies the claim, traces the concern to a table, and keeps its conclusion within what the evidence supports.

## An accuracy and compute tradeoff

Fictional source: On an identical evaluation set and hardware setup, Table 3 reports 84.0 percent accuracy at 40 milliseconds per example for a new method, versus 82.0 percent at 10 milliseconds for a baseline. The abstract claims better accuracy and lower latency.

Review paragraph:

The accuracy improvement in Table 3 comes with higher inference latency under the reported setup. The new method gains 2.0 percentage points while increasing latency from 10 to 40 milliseconds per example. These measurements support an accuracy and latency tradeoff, but contradict the abstract's claim of lower latency relative to this baseline. The result may still be useful where accuracy takes priority; the contribution should be described with that condition in view.

This distinguishes a useful result from an unsupported framing. It does not assume that lower compute matters more than accuracy for every use case.

## A concrete strength

Fictional source: Section 4 lists five training seeds, preprocessing settings, and data splits. Table 1 reports both the mean and standard deviation of each method's accuracy.

Review paragraph:

The evaluation reports variation across five training seeds alongside the mean accuracy for each method. Section 4 also specifies preprocessing and data splits. These details help readers assess whether the reported gains are stable and identify the conditions needed to reproduce the comparison.

Credit the evidence provided; do not infer successful reproduction or statistical significance from reporting alone.

## Questions and suggestions with different purposes

For the gating example above:

Questions. Was the baseline evaluated with 20,000 training examples under the same training settings? If that result exists, it would clarify how much of the reported gain remains when the amount of training data is held fixed.

Suggestions. Prioritize the comparison needed to separate the data effect from the module effect. In the meantime, describe the gain as a result of the full system and reserve a module-specific claim for an experiment that isolates it.

The question seeks information that could change the assessment. The suggestion explains a revision priority. Neither requires a new external citation.

## When an external source is necessary

A novelty objection needs more than a familiar method name. Identify what the earlier work actually does, the scope in which the mechanisms overlap, and a source the reader can inspect. If the source is inaccessible, preserve the uncertainty rather than writing a confident claim of equivalence. Follow [reference verification guidance](hallucination-patterns.md) before including the citation.
