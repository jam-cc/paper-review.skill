# Reference Hallucination Patterns

This document catalogs common LLM hallucination patterns in academic references, based on real cases encountered during ECML PKDD 2026 reviewing.

## Most Common: Wrong Author Lists

The most frequent hallucination type. The paper title and venue are correct, but authors are partially or completely fabricated.

**Real case:** VisualAD paper
- Hallucinated authors: "Xinwei Liu, Yuwei Zhao, Hao Chen, Zhaoxiang Zhang, and Xiaogang Wang"
- Real authors: "Yanning Hou, Peiyuan Li, Zirui Liu, Yitong Wang, Yanran Ruan, Jianfeng Qiu, and Ke Xu"
- Every single author was wrong.

**Real case:** MuSc paper
- Hallucinated: had wrong co-authors mixed in
- Pattern: first author was correct, but remaining authors were fabricated

## Second Most Common: Wrong Venue

The paper exists but is attributed to the wrong conference.

**Real case:** FAPrompt
- Hallucinated venue: CVPR 2025
- Real venue: ICCV 2025
- Pattern: plausible venue substitution (both are top CV conferences)

## Third: Wrong Year

Off by one year is the most common pattern.

## Fourth: Entirely Fabricated Papers

The paper title sounds plausible but doesn't exist at all. This is rarer but more dangerous because there's nothing to partially match against.

## Verification Checklist

For each reference:
1. Search `"exact title"` — does the paper exist?
2. Check author list against DBLP or the paper's own PDF
3. Verify venue name and year
4. If the paper is from arXiv, note the arXiv ID for easy verification

## Statistics from Our Experience

In a batch of 4 reviews with ~20 total references:
- ~65% of LLM-generated author lists contained errors
- ~15% had venue errors
- ~5% were entirely fabricated
- Only ~30% were fully correct as generated

The takeaway: **always verify, never trust LLM-generated references.**
