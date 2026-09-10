# paper-review.skill

> 把"做一篇严谨、证据充分、不水的会议审稿"这件事，蒸馏成一个 AI Skill。

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Claude Code](https://img.shields.io/badge/Claude%20Code-Skill-blueviolet)](https://claude.ai/code)
[![Cowork](https://img.shields.io/badge/Cowork-Ready-green)](https://claude.com)

把一篇 ML/AI 会议论文的 PDF 丢给 Claude，得到一份**结构化、有证据、引用真实**的审稿意见——summary / strengths / weaknesses / questions / suggestions / references 一应俱全，weakness 用论文自己的数字说话，question 是真正问作者的疑问而不是要求修改清单，引用每条都经 web search 验证不会幻觉。

适用于 NeurIPS / ICML / ICLR / CVPR / ECCV / AAAI / ACM MM / ECML PKDD 等会议的审稿场景。

[安装](#安装) · [使用](#使用) · [设计原则](#设计原则) · [详细安装说明](INSTALL.md) · [**English**](README_EN.md)

---

## 为什么写这个 skill

LLM 写审稿有几个反复出现的坑：

1. **没看清论文就开喷**——上来就写"实验不充分"、"对比不全面"，但说不出具体哪个实验缺、缺什么。
2. **新意论断没参照系**——说"这个方法不新"，但没列出哪些 prior work 已经做过类似的事，差别在哪。
3. **引用全是幻觉**——根据我们的经验，LLM 直接生成的参考文献作者列表 60% 到 80% 是错的，venue 也常常张冠李戴（FlexEdit 和 FlexiEdit 这种名字相近的特别容易混）。
4. **Questions 写成修改清单**——把"作者应该怎么改"塞进 Questions 里，挤占了真正的 clarifying question 空间。
5. **references 段堆一大坨**——把论文 bibliography 已经有的工作（StyleID、IP-Adapter 这种）又复列一遍，纯噪声。

这个 skill 把上面这些坑分别用四个阶段（Frontier Knowledge → Innovation Analysis → Write Review → Prune & Verify References）和一组明确的写作约束（plain prose、no bullet、引用 ≤4 条、不重复论文已引文献）固化下来。

---

## 安装

### Claude Code

```bash
# 安装到当前项目
mkdir -p .claude/skills
git clone https://github.com/jam-cc/paper-review-skill .claude/skills/paper-review

# 或安装到全局（所有项目都能用）
git clone https://github.com/jam-cc/paper-review-skill ~/.claude/skills/paper-review
```

### Cowork (Claude Desktop)

```bash
# 全局安装
mkdir -p ~/Library/Application\ Support/Claude/skills
git clone https://github.com/jam-cc/paper-review-skill \
  ~/Library/Application\ Support/Claude/skills/paper-review
```

### Anthropic Agent SDK / Claude API

把整个 `paper-review-skill/` 目录作为 skill 加载，或读取 `SKILL.md` 内容直接拼到 system prompt。

详细步骤、跨平台路径、常见问题见 [INSTALL.md](INSTALL.md)。

---

## 使用

把待审论文的 PDF 上传给 Claude，然后说：

```
帮我审一下这篇 NeurIPS 投稿
```

或

```
review this MM26 submission
```

Skill 会自动触发，按四个阶段产出：

1. **Phase 1 — Frontier Knowledge**：先 web search 同一子领域近 2-3 年的相关工作，写一份 `frontier_<subfield>.md` 作为对比基线。
2. **Phase 2 — Innovation Analysis**：拿前沿地图核对论文每个 contribution 的"是否真新、是否必要、证据是否支撑、ablation 是否充分"。
3. **Phase 3 — Write Review**：以 plain prose 写出 5 段：summary（2-4 句事实描述）/ strengths / weaknesses（每条以"论文宣称 → 数据显示 → 这意味着什么"组织）/ questions（真正问作者的疑问）/ suggestions（对全文的整体建议）。
4. **Phase 4 — Prune & Verify**：把已经在论文 bibliography 里的引用从 review 里删掉，剩下的 ≤4 条全部 web search 核对作者、venue、year、page。

最终输出 `review_<id>.txt` + `review_<id>.docx` + `frontier_<subfield>.md` 三个文件。

### 多篇连续审稿

```
帮我审一下 MM26 文件夹里面的两篇文章
```

Skill 会按顺序逐篇执行四阶段流程。

---

## 设计原则

### 1. 几条致命的 weakness 胜过一长串表面意见

每条 weakness 必须满足两个条件之一：要么威胁论文的核心贡献，要么揭示一个根本性方法缺陷。否则就该挪进 Questions 或 Suggestions。控制在 3-7 条，理想 4 条。

### 2. 引用只放论文没引、且对 weakness 起承重作用的工作

Review 不是一篇论文，它的 References 段不是论文 bibliography 的复述。在加任何引用之前先检查它是否已在论文里——如果在，连 inline 方括号都不要打，直接用名字提（"StyleSSP already manipulates...".）；只有论文没引的工作才进 Review 的 References 段，且总数 ≤4 条，理想 3 条。

### 3. Questions 是问作者，不是给作者下指令

Questions 是 reviewer 读论文时没看明白、想问作者澄清的事："这个值是多少？""这个图为什么这么算？""这条引文是不是搞错了？"  
Suggestions 是对论文整体的建议（按优先级），不是逐条 weakness 的修复清单。

### 4. 写作风格硬约束

纯 plain text，不用 bold、不用 bullet、不用 dash 或冒号做结构。每条 weakness 是一段连贯散文。"percent" 写出来，数字范围用 "to" 不用 en-dash。

### 5. 引用必须 web search 验证

LLM 生成的参考文献作者列表大部分都有错，所以最后一定要用 web search + DBLP 一条条核对作者、venue、year、page。

---

## 项目结构

本项目遵循 [AgentSkills](https://agentskills.io) 标准，整个 repo 就是一个 skill 目录：

```
paper-review-skill/
├── SKILL.md                     # skill 入口（带 frontmatter，描述触发条件 + 四阶段流程）
├── README.md                    # 本文件
├── README_EN.md                 # 英文 README
├── INSTALL.md                   # 跨平台安装详细说明
├── LICENSE                      # MIT
├── .gitignore
├── references/                  # SKILL.md 按需读取的参考文档
│   ├── review-examples.md       #   高质量审稿段落范例（calibrate 风格）
│   ├── hallucination-patterns.md #  常见引用幻觉模式（calibrate 警惕）
│   └── conference-formats.md    #   各会议/期刊的 review 格式与评分约定
├── examples/                    # （目前为空）仅接受完全合成的示例，不放任何真实审稿产物
└── docs/                        # 设计文档
    └── DESIGN.md                #   四阶段流程的设计动机
```

---

## 适用与限制

**适用场景**

- ML / AI / CV / NLP / DM 等领域的会议审稿（NeurIPS, ICML, ICLR, CVPR, ECCV, AAAI, ACM MM, ECML PKDD 等）
- 期刊审稿（如 IEEE TPAMI、IJCV、TKDE）
- 自审 / 内部 brain trust：在投稿前给自家论文做"红队 review"
- Rebuttal 准备：从 reviewer 视角预判 area chair 会问什么

**目前不太适用**

- 纯理论数学论文（缺乏实验数据，frontier 知识需要更深的领域 grounding）
- 非英文论文（web 验证基础设施都假设英文 paper title）
- 极小子领域（前沿知识 web search 命中率低）

---

## 演进与贡献

欢迎 PR，特别欢迎以下方向的扩展：

- 增加 `references/conference-norms.md`：不同会议（NeurIPS vs ICML vs CVPR）对 review 长度、scoring rubric、confidence 的差异
- 增加 `references/subfield-priors.md`：常见子领域的"高复杂度方法 vs 简单 baseline"对照表，帮 Phase 1 起步更快
- 增加 `examples/`：脱敏的真实审稿示例，便于校准风格
- 多语言：让 Skill 能输出中文 review（部分国内会议）

提交 PR 时请保留 SKILL.md 的核心约束（四阶段、≤4 引用、plain prose），扩展通过新增 `references/*.md` 完成。

---

## 致谢

设计灵感来自 Anthropic Skills 团队的 [skill-creator](https://github.com/anthropics/skills) 模板和 [titanwings/colleague-skill](https://github.com/titanwings/colleague-skill) 的"整个 repo 就是一个 skill"项目结构。引用幻觉模式部分基于 ECML PKDD 2026 真实审稿过程中观察到的案例。

---

MIT License © 2026
