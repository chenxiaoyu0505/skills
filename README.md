# Taya Skills

**English** · [中文](README.zh-CN.md)

A collection of reusable **Agent Skills** for Claude (Claude Code / Claude.ai / Claude API) and
other AI tools that can load Markdown instructions, such as Codex. Each skill is a self-contained
folder with a `SKILL.md` instruction file, plus the templates and references it loads on demand.

This repository currently ships one skill: **`technical-pm`**.

## `technical-pm` - Technical Product Manager

`technical-pm` makes the assistant operate like an experienced **technical product manager**: someone
who owns *what to build and why*, not just how. Give it a fuzzy idea, raw feature request, meeting
notes, or an existing requirements document, and it turns that material into clear, sharply scoped,
buildable product artifacts.

The skill is intentionally product-first. It should clarify the problem, scope, success criteria, and
risks before implementation begins.

> **AI/ML product lens built in.** When the feature is AI-driven, such as extraction, classification,
> generation, OCR, or matching, `technical-pm` applies a dedicated lens: eval sets, thresholds,
> precision/recall tradeoffs, error-cost asymmetry, human-in-the-loop review, `needs_review` handling,
> and data dependencies.

### What It Produces

| Need | Output |
|---|---|
| **Discovery and framing** | Real problem surfaced before accepting a proposed solution; target user, goal, constraints, and explicit non-goals |
| **PRD / spec** | Problem, scope, user stories with acceptance criteria, success metrics, risks, and open questions |
| **Existing-doc reconstruction** | A structured PRD from meeting notes, old docs, or competitor material, with conflicts and assumptions surfaced |
| **Slicing** | Epics, user stories, tasks, and `Given / When / Then` acceptance criteria covering happy path, boundaries, and failure |
| **Prioritization** | Ranked backlog via RICE, MoSCoW, Kano, Value/Effort, or WSJF, with scores and reasoning shown |
| **Estimation and planning** | Story points, velocity, PERT three-point estimates, sprint planning, release slices, and dependencies |
| **Metrics** | North Star, input, and guardrail metrics, each with a baseline and target |
| **Risk** | Probability x impact risk register with triggers, owners, and responses |
| **Communication** | Stakeholder updates and decision docs tuned to the audience |

### When To Use It

Use `technical-pm` whenever you are deciding **what to build or why**:

- writing a PRD or product spec;
- scoping an MVP;
- breaking a feature into epics, stories, and tasks;
- prioritizing a backlog;
- estimating a sprint or release;
- defining success metrics;
- assessing product, delivery, or technical risk;
- preparing a stakeholder update or decision doc.

It should also activate when someone asks the assistant to "act as a PM", "wear the product hat", or
starts building without a clear problem statement and scope.

### Output Language

The skill follows the user's language. For Chinese users, product artifacts default to Chinese while
keeping established framework terms such as `MVP`, `PRD`, `RICE`, and `MoSCoW` in English.

## Installing Into Claude Code

Copy the skill folder into Claude Code's skills directory:

```bash
mkdir -p "$HOME/.claude/skills" && cp -R technical-pm "$HOME/.claude/skills/technical-pm"
```

Then start a new conversation and either let the skill trigger automatically on product/PM-shaped
requests, or invoke it explicitly:

```text
$technical-pm
```

Example prompt:

```text
Act as a technical PM and turn this feature idea into a scoped PRD.
```

## Other AI Tools

Load `technical-pm/SKILL.md` as the system instruction. The skill will pull relevant files from
`references/` and `assets/` on demand. For Codex-style UIs, `technical-pm/agents/openai.yaml`
provides display metadata and a default prompt.

## Repository Structure

```text
skills/
├── .gitignore
├── README.md
├── README.zh-CN.md
└── technical-pm/
    ├── SKILL.md                       # instructions and workflow router
    ├── agents/
    │   └── openai.yaml                # display name and default prompt for Codex-style UIs
    ├── assets/                        # fill-in templates
    │   ├── prd-template.md
    │   ├── risk-register-template.md
    │   └── user-story-template.md
    └── references/                    # deep-dive references loaded on demand
        ├── ai-ml-products.md
        ├── delivery-and-process.md
        ├── discovery-and-requirements.md
        ├── edge-cases-and-exceptions.md
        ├── metrics.md
        ├── prioritization-and-estimation.md
        ├── technical-artifacts.md
        └── worked-example.md
```

## Conventions

- `SKILL.md` is the sole entry point for each skill.
- `agents/openai.yaml` holds UI metadata; execution logic lives in `SKILL.md`.
- `references/` holds deep-dive material loaded progressively on demand.
- `assets/` holds output templates and reusable files.
- Avoid extra READMEs or changelogs inside skill folders so the assistant has less loading noise.

## Adding More Skills

To add another skill, create a new top-level folder with at least a `SKILL.md`. Keep the skill
self-contained: templates in `assets/`, detailed guidance in `references/`, and UI metadata in
`agents/` only when a target assistant surface needs it.
