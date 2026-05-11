# Examples

This directory is intentionally empty in the initial release. Real reviewing work is confidential and should never be checked into a public repository, even when it appears to consist only of "paper-independent" artifacts like frontier-knowledge documents — those documents are still tied to specific submissions under double-blind review, and the reviewer's analysis itself is intellectual work that the reviewer may not have consented to release.

## What can go here

Only **fully synthetic** content built specifically for documentation purposes:

- A made-up paper title in a made-up subfield with made-up numbers, where every comparison and citation is fictional or refers to long-published public work
- A frontier-knowledge document about a public, settled subfield where there is no specific submission anyone could be reviewing (for example, a frontier doc on classical AdaIN-based style transfer from 2017-2019, when most papers are already published and the field is no longer actively reviewed)
- A walkthrough of how the skill's four phases were used on the synthetic example, with the resulting (synthetic) review

## What must NOT go here

- Any frontier-knowledge document generated while reviewing a real submission, even if anonymized — the choice of which subfield to map and which competitors to flag is itself reviewer IP
- Any review text, even with names and IDs redacted
- Any source PDF of a submission, even with the title page removed
- Any "innovation analysis" or audit notes from a real reviewing batch

If you have a frontier doc from your own real reviewing work that you would like to share, the right channel is to **wait until the reviewing process for that batch is fully concluded** (final decisions out, embargo lifted), and then optionally publish it on your own under your own name — not bundle it with this skill.

## How to contribute a synthetic example

A good synthetic example demonstrates one specific aspect of the skill (Phase 1 frontier mapping, Phase 4 reference pruning, conference-format adaptation, etc.) without referring to any specific real submission. PRs adding such examples are welcome.

The structure for a contributed synthetic example should be:

```
examples/<synthetic-name>/
├── README.md                # What this example demonstrates and why it's synthetic
├── synthetic_paper.md       # The made-up "paper" being reviewed (a short fake abstract + claimed contributions)
├── frontier_<subfield>.md   # Phase 1 output, made-up subfield or settled public subfield
└── review_<id>.txt          # The skill's output on the synthetic paper
```
