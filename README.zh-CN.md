# Taya Skills

[English](README.md) · **中文**

一组可复用的 **Agent Skills（技能包）**，适用于 Claude（Claude Code / Claude.ai / Claude API）以及其他可加载 Markdown 指令的 AI 工具，例如 Codex。每个技能都是一个自包含的文件夹，包含 `SKILL.md` 指令文件，以及它按需加载的模板和参考资料。

本仓库目前包含一个技能：**`technical-pm`**。

## `technical-pm` - 技术产品经理

`technical-pm` 会让助手以资深**技术产品经理**的方式工作：负责*做什么、为什么做*，而不只是怎么做。你可以给它一个模糊想法、原始功能需求、会议纪要或已有需求文档，它会把这些材料整理成清晰、收敛、可落地的产品产物。

这个技能会先处理产品问题。它应该先澄清问题、范围、成功标准和风险，再进入实现。

> **内置 AI/ML 产品视角。** 当功能由 AI 驱动，例如提取、分类、生成、OCR 或匹配，`technical-pm` 会应用专门视角：评测集、阈值、精确率 / 召回率取舍、错误代价不对称、人工复核、`needs_review` 处理和数据依赖。

### 能产出什么

| 需求 | 产出 |
|---|---|
| **需求发现与界定** | 先找出真实问题，而不是直接接受某个方案；明确目标用户、目标、约束和非目标 |
| **PRD / 需求规格** | 问题、范围、带验收标准的用户故事、成功指标、风险和未决问题 |
| **已有文档重构** | 从会议纪要、旧 PRD 或竞品资料中整理结构化 PRD，并标出冲突和假设 |
| **需求拆分** | 史诗、用户故事、任务，以及覆盖正常路径、边界和异常的 `Given / When / Then` 验收标准 |
| **优先级排序** | 使用 RICE、MoSCoW、Kano、价值 / 成本或 WSJF 排序，并展示打分和理由 |
| **估算与计划** | 故事点、速率、PERT 三点估算、迭代计划、版本切片和依赖项 |
| **指标设计** | 北极星指标、输入指标、护栏指标，每个都带基线和目标 |
| **风险管理** | 概率 x 影响的风险登记册，包含触发条件、负责人和应对策略 |
| **干系人沟通** | 面向不同受众的状态更新和决策文档 |

### 什么时候使用

只要你在决定**做什么、为什么做**，就可以使用 `technical-pm`：

- 写 PRD 或产品规格；
- 界定 MVP；
- 把功能拆成史诗、用户故事和任务；
- 给待办列表排优先级；
- 估算迭代或版本计划；
- 定义成功指标；
- 评估产品、交付或技术风险；
- 准备干系人汇报或决策文档。

当你说“当个 PM”“戴上产品帽子”，或在问题和范围还不清晰时就开始实现，这个技能也应该被触发。

### 产出语言

技能会跟随用户语言。中文用户的产品产物默认使用中文，只保留团队常用的框架术语，例如 `MVP`、`PRD`、`RICE`、`MoSCoW`。

## 安装到 Claude Code

把技能目录复制到 Claude Code 的 skills 目录：

```bash
mkdir -p "$HOME/.claude/skills" && cp -R technical-pm "$HOME/.claude/skills/technical-pm"
```

然后开启新对话，让它在产品 / PM 类请求中自动触发，或显式调用：

```text
$technical-pm
```

示例：

```text
请作为技术产品经理，把这个功能想法整理成一份范围清晰的 PRD。
```

## 其他 AI 工具

将 `technical-pm/SKILL.md` 作为系统指令加载。技能会在需要时按需拉取 `references/` 和 `assets/` 中的文件。如果使用 Codex 风格的 UI，`technical-pm/agents/openai.yaml` 提供展示名称和默认提示词。

## 仓库结构

```text
skills/
├── .gitignore
├── README.md
├── README.zh-CN.md
└── technical-pm/
    ├── SKILL.md                       # 指令与工作流路由
    ├── agents/
    │   └── openai.yaml                # Codex 风格 UI 的展示名称和默认提示词
    ├── assets/                        # 填写模板
    │   ├── prd-template.md
    │   ├── risk-register-template.md
    │   └── user-story-template.md
    └── references/                    # 按需加载的深度参考资料
        ├── ai-ml-products.md
        ├── delivery-and-process.md
        ├── discovery-and-requirements.md
        ├── edge-cases-and-exceptions.md
        ├── metrics.md
        ├── prioritization-and-estimation.md
        ├── technical-artifacts.md
        └── worked-example.md
```

## 约定

- `SKILL.md` 是每个技能的唯一入口。
- `agents/openai.yaml` 存放 UI 元数据；执行逻辑在 `SKILL.md` 中。
- `references/` 存放按需渐进加载的深度参考资料。
- `assets/` 存放产出模板和可复用文件。
- 技能文件夹内尽量不放额外 README 或 changelog，减少助手加载噪音。

## 新增更多技能

如需新增技能，在仓库顶层创建一个新目录，并至少包含 `SKILL.md`。建议保持技能自包含：模板放在 `assets/`，详细说明放在 `references/`，只有目标助手界面需要时才添加 `agents/` 元数据。
