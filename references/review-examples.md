# Review Examples

This file contains excerpts from real reviews to illustrate the target style and quality level. Read these to calibrate your tone, specificity, and argumentation structure. Notice that the writing is plain prose with no markdown formatting, no bullet points, no bold or italic markers, and no dashes or colons used for structure.

## Example: Strong Weakness with evidence

1. The central concern is whether the entire retrieval based framework is necessary. The paper frames anomaly detection as semantic retrieval over a structured defect knowledge base, treating this reformulation as the key conceptual contribution. However, recent work strongly suggests that structured defect semantics may not be needed at all. MuSc [1] achieves zero shot anomaly detection without any training, text prompts, or semantic repository by exploiting mutual scoring among unlabeled test images, reaching 97.8 percent image AUROC and 93.8 percent PRO on MVTec AD. AnomalyDINO [2] uses only DINOv2 features in a one shot setting and reaches 96.6 percent AUROC on MVTec AD. These methods suggest that the defect knowledge repository around which the proposed method is built may be solving a problem that does not need to be solved.

Why this works: It names the paper's core claim, presents two specific counterexamples with exact numbers, and draws a clear conclusion about what this means for the contribution. The whole argument flows as a single continuous paragraph.

## Example: Catching a framing problem

3. The performance on ImageNet 1k, which is the most widely used zero shot classification benchmark, does not support the paper's efficiency claims. Table 2 shows that WaveCLIP high at 10.8 GFLOPs reaches 66.3 percent on ImageNet 1k, while CLIP ViT B 16 at 16.87 GFLOPs reaches 68.3 percent. This is a 2 point drop on the single most scrutinized benchmark in the field. The paper frames its results around the 40 dataset average and retrieval averages where WaveCLIP does show improvements, but this framing obscures the classification deficit.

At the mid efficiency point, WaveCLIP mid at 7.39 GFLOPs reaches 64.73 percent on ImageNet 1k, while MobileCLIP S2 at 7.11 GFLOPs reaches 64.6 percent. The two methods are essentially tied at similar compute, but MobileCLIP achieves this with a standard architecture and does not require wavelet transforms, causal masking, or KV caching.

Why this works: It uses the paper's own numbers against its narrative, identifies the specific framing choice, and shows the comparison is unfair by walking through the actual data points.

## Example: Identifying a confound

4. The distillation only training setup makes it impossible to isolate the contribution of wavelet tokenization. WaveCLIP is trained by distilling from a frozen CLIP ViT L 14 teacher on CC12M for 10 epochs, while the baseline CLIP models in Table 1 and Table 2 were trained from scratch on approximately 400 million image text pairs with contrastive loss. These are fundamentally different training regimes.

CLIP-KD [4] has shown that simple feature mimicry distillation from a large CLIP teacher can improve a ViT B 16 student by over 20 points in zero shot accuracy. This means that some of the performance attributed to wavelet design may actually come from the distillation process itself. The critical missing experiment is a non wavelet control student. If a standard CLIP model with reduced token count is distilled under identical conditions and achieves similar results, then the wavelet contribution is minimal.

Why this works: It identifies exactly what variable is confounded, names the specific prior work that demonstrates the risk, and proposes the specific experiment that would resolve the ambiguity.

## Example: Overclaiming detection

2. The use of the term open set in the title is misleading and does not match the system's actual capabilities. The knowledge repository must be populated in advance with defect type names and descriptors for the target domain. For defect types absent from the repository, the system can only produce binary anomaly scores. This means the method operates over a closed vocabulary of defect types. A more accurate title would be something like closed vocabulary multi type zero shot anomaly detection with retrieval based scoring.

Why this works: It identifies the specific term being misused, explains what the system actually does, and proposes an honest alternative. No dashes, colons, or bullet points needed.

## Example: Good strength

2. The alternating optimization between the Prompt Refiner and the learnable tokens is a reasonable design choice. By freezing one component while updating the other, the method avoids the instability of jointly optimizing a discrete text generator and continuous token embeddings. The training procedure is described clearly enough to support reproduction, and the algorithm box is helpful.

Why this works: It identifies a specific mechanism, explains what it achieves, and notes its practical value. Compare with bad strengths like "the paper is well-written" or "the experiments are comprehensive."

## Example: Constructive suggestion as prose

The most important missing experiment is a non wavelet distillation baseline. The authors should distill a standard CLIP student with a comparable number of tokens, for example a ViT B 16 with random token dropping or patch merging, under the same CC12M distillation setup. This would isolate the contribution of wavelet tokenization from the contribution of distillation.

The paper should include CF-ViT [1] as a baseline, since it represents the closest prior work on coarse to fine ViT inference. If WaveCLIP cannot outperform CF-ViT at similar compute budgets, the case for frequency domain decomposition over spatial decomposition is weak.

Why this works: It proposes specific, actionable experiments and explains what each one would reveal. Written as flowing paragraphs, not a checklist.
