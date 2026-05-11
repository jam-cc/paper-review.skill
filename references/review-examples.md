# Review Examples

This file contains excerpts adapted from real reviews to illustrate the target style and quality level. Specific method names and identifying details from anonymous-period submissions have been replaced with generic placeholders ("the proposed method", "the proposed component", etc.); the writing structure, evidence patterns, and cited public works are preserved so you can calibrate tone, specificity, and argumentation. Notice that the writing is plain prose with no markdown formatting, no bullet points, no bold or italic markers, and no dashes or colons used for structure.

## Example: Strong Weakness with evidence

1. The central concern is whether the entire retrieval based framework is necessary. The paper frames the task as semantic retrieval over a structured knowledge base, treating this reformulation as the key conceptual contribution. However, recent work strongly suggests that structured semantics may not be needed at all. MuSc [1] achieves zero shot anomaly detection without any training, text prompts, or semantic repository by exploiting mutual scoring among unlabeled test images, reaching 97.8 percent image AUROC and 93.8 percent PRO on MVTec AD. AnomalyDINO [2] uses only DINOv2 features in a one shot setting and reaches 96.6 percent AUROC on MVTec AD. These methods suggest that the knowledge repository around which the proposed method is built may be solving a problem that does not need to be solved.

Why this works: It names the paper's core claim, presents two specific counterexamples with exact numbers, and draws a clear conclusion about what this means for the contribution. The whole argument flows as a single continuous paragraph.

## Example: Catching a framing problem

3. The performance on ImageNet 1k, which is the most widely used zero shot classification benchmark, does not support the paper's efficiency claims. Table 2 shows that the proposed method's high-compute variant at roughly 10 GFLOPs reaches 66.3 percent on ImageNet 1k, while CLIP ViT B 16 at 16.87 GFLOPs reaches 68.3 percent. This is a 2 point drop on the single most scrutinized benchmark in the field. The paper frames its results around the 40 dataset average and retrieval averages where the method does show improvements, but this framing obscures the classification deficit.

At a comparable mid efficiency point, the proposed method reaches roughly 64.7 percent on ImageNet 1k while MobileCLIP S2 at similar compute reaches 64.6 percent. The two methods are essentially tied at the same compute, but MobileCLIP achieves this with a standard architecture and does not require the additional components introduced by the proposed method.

Why this works: It uses the paper's own numbers against its narrative, identifies the specific framing choice, and shows the comparison is unfair by walking through the actual data points.

## Example: Identifying a confound

4. The distillation only training setup makes it impossible to isolate the contribution of the proposed tokenization scheme. The student is trained by distilling from a frozen CLIP ViT L 14 teacher on CC12M for 10 epochs, while the baseline CLIP models in Table 1 and Table 2 were trained from scratch on approximately 400 million image text pairs with contrastive loss. These are fundamentally different training regimes.

CLIP-KD [4] has shown that simple feature mimicry distillation from a large CLIP teacher can improve a ViT B 16 student by over 20 points in zero shot accuracy. This means that some of the performance attributed to the proposed design may actually come from the distillation process itself. The critical missing experiment is a control student without the proposed modifications. If a standard CLIP model with comparable token count is distilled under identical conditions and achieves similar results, then the contribution of the proposed component is minimal.

Why this works: It identifies exactly what variable is confounded, names the specific prior work that demonstrates the risk, and proposes the specific experiment that would resolve the ambiguity.

## Example: Overclaiming detection

2. The use of the term open set in the title is misleading and does not match the system's actual capabilities. The knowledge repository must be populated in advance with category names and descriptors for the target domain. For categories absent from the repository, the system can only produce binary anomaly scores. This means the method operates over a closed vocabulary of categories. A more accurate title would be something like closed vocabulary zero shot detection with retrieval based scoring.

Why this works: It identifies the specific term being misused, explains what the system actually does, and proposes an honest alternative. No dashes, colons, or bullet points needed.

## Example: Self-undermining ablation

2. The headline mechanism is not supported by the paper's own ablation. The method is named after a multi-component update rule, but Table 4 shows that going from the open-loop baseline at the primary consistency metric value 0.639 to a single-component variant at 0.682 captures essentially all of the improvement. Adding the second term yields 0.676, which is actually worse than the single-component variant alone, and the full mechanism reaches 0.684, a gain of 0.002 over the simplest variant. The same pattern holds for the secondary metric, where the simplest variant achieves 61.361 and the full mechanism achieves 60.844, a 0.85 percent relative improvement. The headline contribution is therefore a sub-percent effect on top of what a one-term version already delivers. A more honest presentation would drop the multi-component branding or demote the additional terms to an optional appendix.

Why this works: It walks through the ablation table row by row to show the headline mechanism contributes almost nothing, and proposes an honest renaming. The paper provides the evidence against itself, which is the strongest possible form of this argument.

## Example: Framing mismatch with the actual driver of gains

1. There is a mismatch between the paper's framing and the actual source of the gains. The title and abstract highlight one component as the central contribution, but the ablation in Table 2 tells a different story. The first ablation row uses an existing prompting strategy with the authors' redesigned detector and no headline component. It already reaches 96.4, 95.2, and 96.7 AUROC under the 1 shot, 2 shot, and 4 shot settings on MVTec AD. The full method reaches 96.7, 96.7, and 97.3. The headline component adds only 0.3, 1.5, and 0.6 points. The redesigned detector, not the highlighted component, appears to be the main driver of performance. The paper should reframe its contribution to match what the ablation actually shows.

Why this works: It compares the ablation row without the headline component against the full method, computes the actual delta, and identifies what is really doing the work. Asking the paper to reframe rather than to add new experiments is a constructive resolution.

## Example: Latency-matched baseline missing

3. The paper does not run a latency matched baseline, and this is the single most important missing experiment. Each iteration of the proposed loop is a full denoising pass. The paper reports 20 iterations per image at 4.2 seconds each, which is roughly 84 seconds per image versus 4.2 seconds for the open-loop baseline, a 20 times cost multiplier. The obvious latency matched comparison is best of N re ranking using the same scoring network as the selection criterion, which also multiplies inference cost linearly and requires no change to the base model. For the identity task best of 20 with an ArcFace based ranker is a standard and strong baseline, and for the geometric tasks best of N with the same error metric as selection criterion is trivially implementable. Without this comparison the reader cannot distinguish whether the feedback loop is exploiting a real convergent dynamic or simply amortizing the same compute budget that best of N would spend equally effectively.

Why this works: It quantifies the compute overhead (20x), names the obvious matched-compute alternative (best of N with the same scorer), gives concrete instantiations for each task, and explains why the comparison is necessary to interpret the gains.

## Example: Standard mechanism overclaimed as a contribution

5. The proposed weighting module is presented as a contribution but is a standard mechanism. Its computation consists of global average pooling of patch level similarity scores followed by temperature scaled softmax normalization and weighted summation. This is a standard attention mechanism that has been used extensively in multi modal retrieval and NLP. The ablation shows that replacing the module with mean averaging reduces F1 from 50.2 to 42.8, but this only demonstrates that non uniform weighting is better than uniform weighting, which is expected. The gap does not establish that the named module is a novel or non obvious design. Listing this standard computation as a distinct contribution overstates its significance.

Why this works: It strips the module name to reveal what the computation actually is, identifies the prior art family it belongs to, and explains why the supporting ablation does not establish novelty.

## Example: Good strength

2. The alternating optimization between the prompt refinement module and the learnable tokens is a reasonable design choice. By freezing one component while updating the other, the method avoids the instability of jointly optimizing a discrete text generator and continuous token embeddings. The training procedure is described clearly enough to support reproduction, and the algorithm box is helpful.

Why this works: It identifies a specific mechanism, explains what it achieves, and notes its practical value. Compare with bad strengths like "the paper is well-written" or "the experiments are comprehensive."

## Example: Constructive suggestion as prose

The most important missing experiment is a control distillation baseline. The authors should distill a standard CLIP student with a comparable number of tokens, for example a ViT B 16 with random token dropping or patch merging, under the same CC12M distillation setup. This would isolate the contribution of the proposed tokenization from the contribution of distillation.

The paper should include CF-ViT [1] as a baseline, since it represents the closest prior work on coarse to fine ViT inference. If the proposed method cannot outperform CF-ViT at similar compute budgets, the case for the proposed decomposition over spatial decomposition is weak.

Why this works: It proposes specific, actionable experiments and explains what each one would reveal. Written as flowing paragraphs, not a checklist.
