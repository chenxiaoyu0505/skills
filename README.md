# Taya Skills

A collection of **Agent Skills** for Claude — reusable capability packs that teach the
assistant a specialized way of working. Each skill is a self-contained folder with a
`SKILL.md` (instructions), plus the templates and references it pulls from on demand.

This repo ships two skills: **`technical-pm`** (write a PRD) and **`prd-review`** (audit a PRD someone already wrote).

> 📖 English below · [中文说明见下半部分 ↓](#中文说明)

---

## `technical-pm` — Technical Product Manager

Makes the assistant operate as an experienced **technical product manager**: someone who
owns *what to build and why*, not just how. Point it at a fuzzy idea or a raw feature
request and it turns it into clear, sharply-scoped, buildable product artifacts — and it
won't write code until the product thinking is solid.

### What it produces

| Need | What you get |
|---|---|
| **Discovery & framing** | The real problem surfaced (5 Whys, Jobs-To-Be-Done) before any solution is accepted, plus a one-sentence problem statement + goal |
| **PRD / spec** | Problem, scope (in/out), user stories with acceptance criteria, success metrics, risks — scales from a one-pager to a multi-module program doc; can also **reconstruct** a PRD from existing material (meeting notes, an old doc, a competitor's) |
| **Slicing** | An idea broken into epics → user stories → tasks, INVEST-checked, with `Given/When/Then` acceptance criteria covering happy path **+ boundaries + failure** |
| **Prioritization** | A ranked backlog via RICE / MoSCoW / Kano / Value-Effort / WSJF — with the scores and the reasoning shown, not just a verdict |
| **Estimation & planning** | Story points, velocity, PERT three-point, sprint and release planning |
| **Metrics** | A North Star + input + guardrail metrics, each with a baseline and target |
| **Risk** | A probability × impact register with triggers, owners, and responses |
| **Communication** | Stakeholder updates and decision docs tuned to the audience |

### When it activates

Whenever you're deciding **what to build or why** — writing a PRD, scoping an MVP, breaking
down a feature, prioritizing a backlog, planning a sprint, defining metrics, or assessing
risk. Also when you say *"act as a PM"* / *"put on the product hat"*, or start building
something with no clear problem statement. It carries a dedicated lens for **AI/ML-driven
features** (eval sets, precision/recall, error-cost asymmetry, human-in-the-loop).

### Output language

Adapts to your language; **defaults to 中文** for the product artifacts, keeping only
established framework terms (MoSCoW, RICE, MVP, PRD) in English.

### Using it

1. Copy the `technical-pm/` folder into your skills directory (e.g. `~/.claude/skills/` for
   Claude Code, or your platform's skills location).
2. Start a conversation and either let it trigger automatically (it activates on
   product/PM-shaped requests) or invoke it explicitly — e.g. `$technical-pm`, or
   *"act as a technical PM and scope this feature."*

The skill reads `SKILL.md` first, then pulls the relevant files from `references/` and
`assets/` on demand.

---

## `prd-review` — PRD Completeness Review

Inverts `technical-pm`: instead of *writing* a PRD, it **audits one someone already wrote**. Point it
at an existing PRD / spec and it answers a single question — *can an engineer build the right thing
from this without guessing, and if not, what exactly is missing?* It diagnoses; it does not rewrite.

### What it produces

| Need | What you get |
|---|---|
| **Completeness rating** | How complete the doc is, judged against the same technical-PM rubric (problem framing, scope in/out, user stories + acceptance criteria covering edges, success metrics, technical contracts, AI/ML eval bar, risks & dependencies) |
| **Go / no-go verdict** | A **ready / conditional / not-ready** call on whether the PRD can go to development |
| **Gap list** | A prioritized, specific list of exactly *what is missing and must be added* — not vague notes |
| **HTML report** | The whole assessment delivered as a clear, self-contained **HTML report** |

### When it activates

On *reviewing / grading an existing PRD* — e.g. *"审一下这份 PRD"*, *"这份需求能交开发吗"*,
*"PRD 还缺什么"*, *"is this spec ready for dev"* — not on writing one from scratch. It reuses the
`technical-pm` body of knowledge, but inverts it: those files define *how a good PRD is written*, so
here they become the **standard it judges against**.

### Using it

Copy the `prd-review/` folder into your skills directory (same as above), then point it at a PRD —
e.g. `$prd-review`, or *"审一下这份需求文档,能不能交开发。"*

---

## Repo structure · 目录结构

```
skills/
├── README.md
├── technical-pm/
│   ├── SKILL.md                       # the skill's instructions + workflow router
│   ├── agents/
│   │   └── openai.yaml                # interface manifest (display name, default prompt)
│   ├── assets/                        # fill-in templates
│   │   ├── prd-template.md
│   │   ├── user-story-template.md
│   │   └── risk-register-template.md
│   └── references/                    # deep-dive references the skill loads on demand
│       ├── discovery-and-requirements.md
│       ├── technical-artifacts.md
│       ├── ai-ml-products.md
│       ├── prioritization-and-estimation.md
│       ├── delivery-and-process.md
│       ├── metrics.md
│       ├── edge-cases-and-exceptions.md
│       └── worked-example.md
└── prd-review/
    ├── SKILL.md                       # PRD-review instructions + rubric router
    ├── agents/
    │   └── openai.yaml                # interface manifest (display name, default prompt)
    ├── assets/                        # templates + the HTML report shell
    │   ├── prd-template.md
    │   ├── user-story-template.md
    │   ├── risk-register-template.md
    │   └── report-template.html
    └── references/                    # the rubric + standards it judges against
        ├── review-rubric.md
        ├── discovery-and-requirements.md
        ├── technical-artifacts.md
        ├── ai-ml-products.md
        ├── prioritization-and-estimation.md
        ├── delivery-and-process.md
        ├── metrics.md
        ├── edge-cases-and-exceptions.md
        ├── worked-example.md
        └── worked-review-example.md
```

---

## 中文说明

一组面向 Claude 的 **Agent Skills(技能包)**—— 可复用的能力模块,教模型用某种专业方式工作。
每个技能都是一个自包含的文件夹,内含 `SKILL.md`(指令),以及它按需调用的模板和参考资料。

本仓库包含两个技能:**`technical-pm`**(写 PRD)和 **`prd-review`**(审别人已经写好的 PRD)。

### `technical-pm` —— 技术产品经理

让模型以资深**技术产品经理**的身份工作:它负责*做什么、为什么做*,而不只是怎么做。给它一个
模糊的想法或原始需求,它会产出清晰、收敛、可落地的产品文档 —— 并且在产品思路理顺之前不会
去写代码。

**它能产出**

| 你的需求 | 你会得到 |
|---|---|
| **需求发现与界定** | 先用 5 Whys / JTBD 挖出真正的问题,而不是直接接受某个"解决方案",并给出一句话问题陈述 + 目标 |
| **PRD / 需求文档** | 问题、范围(做 / 不做)、带验收标准的用户故事、成功指标、风险;小到一页纸,大到多模块项目文档;也能把已有材料(会议纪要、旧文档、竞品)**重构**成 PRD |
| **需求拆分** | 把想法拆成 史诗 → 用户故事 → 任务,过 INVEST 检查,验收标准用 `Given/When/Then` 覆盖正常 **+ 边界 + 异常** |
| **优先级** | 用 RICE / MoSCoW / Kano / 价值-成本 / WSJF 排序,并给出打分和理由,而不只是结论 |
| **估算与排期** | 故事点、速率、PERT 三点估算、迭代与版本规划 |
| **指标** | 北极星 + 输入 + 护栏指标,每个都带基线与目标 |
| **风险** | 概率 × 影响的风险登记册,带触发条件、负责人、应对策略 |
| **沟通** | 按受众定制的干系人汇报与决策文档 |

**什么时候触发**

只要你在决定**做什么、为什么做**:写 PRD、界定 MVP、拆功能、排优先级、做迭代计划、定指标、
评风险。或者你说"当个 PM""戴上产品的帽子",又或者还没想清问题就开始动手时。它对
**AI / 模型驱动的功能** 有专门视角(评测集、精确率 / 召回率、错误代价不对称、人工兜底)。

**产出语言**

跟随你的语言,产品文档**默认中文**,只保留约定俗成的框架术语(MoSCoW、RICE、MVP、PRD)为英文。

**如何使用**

1. 把 `technical-pm/` 文件夹复制到你的技能目录(例如 Claude Code 的 `~/.claude/skills/`,
   或你所用平台的技能位置)。
2. 开始对话,让它自动触发(遇到产品 / PM 类请求时),或显式调用 —— 例如 `$technical-pm`,
   或"用 technical-pm 把这个想法写成 PRD"。

模型会先读 `SKILL.md`,再按需拉取相关的 `references/` 和 `assets/`。

### `prd-review` —— PRD 完整性审核

与 `technical-pm` 相反:它不*写* PRD,而是**审别人已经写好的** PRD。给它一份已有的
PRD / 需求文档,它只回答一个问题 —— *工程师能不能照着它把正确的东西造出来而不用猜?
如果不能,到底还缺什么?* 它只诊断、不改写。

**它能产出**

| 你的需求 | 你会得到 |
|---|---|
| **完整度评分** | 用与 technical-pm 相同的评判标准(问题界定、范围做 / 不做、覆盖边界与异常的用户故事 + 验收标准、成功指标、技术契约、AI/ML 评测门槛、风险与依赖)衡量文档有多完整 |
| **交付裁决** | 给出**可开工 / 有条件 / 不可开工**的判断,回答这份 PRD 能不能交开发 |
| **缺失项清单** | 一份按优先级排列、具体的"还缺什么、必须补齐什么"清单,而不是含糊的意见 |
| **HTML 报告** | 整份评估以清晰、自包含的 **HTML 报告** 交付 |

**什么时候触发**

在*审核 / 打分一份已有的 PRD* 时 —— 例如"审一下这份 PRD""这份需求能交开发吗"
"PRD 还缺什么""is this spec ready for dev" —— 而不是从零写一份。它复用 `technical-pm`
的知识体系,但反过来用:那些文件定义了*一份好 PRD 该怎么写*,在这里就成了**它据以评判的标准**。

**如何使用**

把 `prd-review/` 文件夹复制到你的技能目录(同上),然后把一份 PRD 交给它 —— 例如
`$prd-review`,或"审一下这份需求文档,能不能交开发"。
