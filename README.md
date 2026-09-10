# paper-review.skill

> 这个 skill 能输出**专业、有理有据的顶会审稿意见**，对论文的创新点、实验结果和关键结论给出具体评价。

适用于 **Codex、Claude Code、Gemini CLI**，也可以把指令和材料交给其他 AI 助手使用。

你可以用它在投稿前自审，或辅助撰写审稿意见，重点找出：**哪项主张证据不足，哪个对比不公平，哪些问题值得优先解决。**

[![MIT License](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Agent Skills](https://img.shields.io/badge/Agent_Skills-portable-16803c.svg)](https://agentskills.io)
[![Codex](https://img.shields.io/badge/Codex-Skill-111827.svg)](#codex)
[![Claude Code](https://img.shields.io/badge/Claude_Code-Skill-D97757.svg)](#claude-code)
[![Gemini CLI](https://img.shields.io/badge/Gemini_CLI-Skill-4285F4.svg)](#gemini-cli)

面向 **NeurIPS / ICML / ICLR / CVPR / ECCV / AAAI / ACM MM / ECML PKDD** 等 ML/AI 会议的审稿与投稿前自审。

[为什么需要它](#为什么需要这个-skill) · [快速开始](#快速开始) · [审稿示例](#你会得到什么样的意见) · [工作流程](#从论文到审稿意见) · [安装指南](INSTALL.md) · [**English**](README_EN.md)

---

## 为什么需要这个 skill

直接把论文丢给 AI，说一句“帮我审稿”，容易遇到这些问题：

1. **没看清论文，就下结论。** 写着“实验不充分”“对比不全面”，却说不出缺哪个实验、影响哪项结论。读起来很严厉，作者却不知道该改什么。

2. **判断新意，没有具体参照。** 说“方法缺乏创新”，却没找到最接近的前作，也没说明两者在方法、假设和实验条件上的差别。

3. **引用看着像真的，细节却对不上。** 论文标题可能存在，但作者、年份、会议被拼错；也可能把名字相近的两篇工作混为一谈。格式完整不代表查证过。

4. **Questions 写成了修改任务单。** 一连串“请补实验”“请增加对比”，挤掉了真正值得作者澄清的问题，例如一个未说明的设置，或一处可能改变判断的歧义。

5. **参考文献堆得多，却没支撑关键意见。** 重复列出论文已经引用的工作，或加入无关背景文献，让最重要的证据淹没在长列表里。

这个 skill 把应对这些问题的做法写进审稿流程：**先查相关工作，再核对贡献与证据，写成完整意见，最后精简并核验引用。** 每一条关键批评都应能追溯到论文内容或外部来源。

---

## 你会得到什么样的意见

“实验不充分”很难指导修改。这个 skill 要求助手说清楚：缺的证据会影响哪项结论。

下面是一个**完全虚构**的例子，展示目标写法，并非模型实测结果：

> 论文将准确率提升归因于新模块，但表 2 同时改变了模块和训练数据量。基线使用 10,000 个样本，完整方法使用 20,000 个样本。现有比较无法区分结构改进与额外数据的作用，因此尚不足以支持“新模块带来提升”这一主张。

作者接下来需要核查什么，读到这里就清楚了。你可以查看[示例材料与完整分析](examples/synthetic-ablation.md)。

---

## 适合什么时候用

- **投稿前自审**：检查贡献表述、baseline 公平性和消融实验，决定下一轮修改的重点。
- **组内讨论**：带着具体证据讨论论文，把“感觉有问题”落实到表格、公式和实验设置。
- **准备 rebuttal**：区分需要澄清的误解、需要补充的证据，以及应该收窄的主张。
- **辅助同行评审**：在适用的审稿与保密要求允许时，整理有依据的评审草稿，由你核查后使用。

主要面向 ML / AI / CV / NLP 等研究论文。支持中英文请求与输出；理论论文也可检查论证，但本项目的范例更侧重实证研究。

---

## 快速开始

选择你的助手，运行对应的一组命令。需要 Git；已有同名目录时，请先检查已有安装。

### Codex

```bash
mkdir -p ~/.agents/skills
git clone https://github.com/jam-cc/paper-review.skill.git ~/.agents/skills/paper-review
```

附上论文或给出可访问的文件路径，然后输入：

```text
用 $paper-review 帮我做投稿前自审。重点检查贡献是否被实验支撑，
按重要程度写出问题，并用中文解释。论文在 papers/draft.pdf。
```

### Claude Code

```bash
mkdir -p ~/.claude/skills
git clone https://github.com/jam-cc/paper-review.skill.git ~/.claude/skills/paper-review
```

```text
/paper-review 帮我审一下 papers/draft.pdf，重点检查 baseline 和消融实验。
```

### Gemini CLI

```bash
mkdir -p ~/.gemini/skills
git clone https://github.com/jam-cc/paper-review.skill.git ~/.gemini/skills/paper-review
```

```text
使用 paper-review skill 审阅 papers/draft.pdf，用中文输出。
```

安装后开启新会话或刷新 skill 列表。路径依据各平台的官方文档；项目内安装、Windows、Cowork 与手动接入见[安装指南](INSTALL.md)。

### 其他助手 / API

把 [SKILL.md](SKILL.md)、其中需要的 `references/` 文件和论文提供给助手，要求按此流程审阅。能读 Markdown 的助手可以使用这些指令；自动发现 skill、联网核验和文件导出取决于宿主能力。**本项目无需专用 SDK，也不绑定模型。**

---

## 从论文到审稿意见

| 阶段 | 助手需要做什么 | 对你有什么用 |
|---|---|---|
| 1. 查相关工作 | 找到直接前作、竞争方法和简单基线，记录来源与比较条件 | 判断新意时有具体参照 |
| 2. 核对贡献 | 检查新颖性、复杂度收益、主张与证据、消融是否隔离变量 | 把问题定位到证据 |
| 3. 写审稿意见 | 组织 Summary、Strengths、Weaknesses、Questions and Suggestions | 得到可读、可讨论的草稿 |
| 4. 精简与核验引用 | 去重，核查外部引用及其是否支撑批评 | 让关键判断可追溯 |

默认用连贯段落写审稿，最多保留 4 条必要的外部引用，不要求凑数。你提供的格式、语言和审稿表优先。只想检查引用或某一项贡献，也可以直接提出。

---

## 输出

支持文件写入时，默认保存在当前工作项目的 `outputs/`，也可以指定其他目录：

```text
outputs/
├── review_<id>.txt             # 审稿正文
├── frontier_<subfield>.md      # 相关工作、证据链接与核验记录
└── review_<id>.docx            # 按需导出，需要宿主具备 Word 生成能力
```

没有文件工具时在对话中返回正文与来源笔记。没有联网或可用外部文献时，仍可检查论文内部证据，但会标明新颖性与文献覆盖的限制。

---

## 设计原则

### 1. 批评要有出处

数字对应到表格，概念问题对应到具体假设或论证。缺少说明和证明错误是两回事。

### 2. 不为了显得严格而凑缺点

有几个值得讨论的问题就写几个，也给扎实的实验和清楚的贡献应有的肯定。

### 3. 引文核验不能靠记忆

查不到的引用不进入最终参考文献；依赖它的判断也要删除或降低确定性。这个流程有助于减少错误，不保证模型永不出错。

### 4. 换助手也能沿用

工作流只要求所需能力，不写死工具名称。各平台安装说明按官方文档整理，尚未完成所有宿主的端到端实测。

---

## 项目结构

```text
paper-review/
├── SKILL.md                         # 审稿入口与四阶段流程
├── agents/openai.yaml               # Codex 展示信息
├── references/
│   ├── runtime-compatibility.md     # 跨助手适配与工具缺失处理
│   ├── review-examples.md           # 有依据的审稿段落范例
│   ├── hallucination-patterns.md    # 引用核验方法
│   └── conference-formats.md        # 审稿格式适配
├── examples/synthetic-ablation.md   # 完全虚构的完整示例
├── docs/DESIGN.md                   # 设计说明
└── INSTALL.md                       # 各平台安装指南
```

---

## 参与贡献

入口是 [SKILL.md](SKILL.md)，配套有[运行时适配](references/runtime-compatibility.md)、[写作范例](references/review-examples.md)、[引用核验](references/hallucination-patterns.md)和[格式指南](references/conference-formats.md)。[设计说明](docs/DESIGN.md)解释这些选择。

欢迎提交有具体复现步骤的兼容性反馈，以及**完全合成**的审稿示例。请勿提交真实投稿、审稿产物或其脱敏版本；示例要求见 [examples/README.md](examples/README.md)。

如果它帮你发现了投稿前值得修正的问题，欢迎给项目一个 Star，也欢迎告诉我们哪条规则有用、哪条还需要改进。

---

## 致谢与许可

灵感来自 [Anthropic Skills](https://github.com/anthropics/skills) 和 [colleague-skill](https://github.com/titanwings/colleague-skill) 的 skill 组织方式。MIT License，见 [LICENSE](LICENSE)。
