# Agent Knowledge Curator

> Extract and structure the institutional knowledge your agents need to reason the way your best people do.

---

## What This Package Does

Context engineering gets the right data into the agent's context window. That's necessary. It's not sufficient.

Even with the data, the agent doesn't know how to read it. It doesn't know what matters, what to ignore, what "empty" means versus what "missing" means. It doesn't know that the same CRM note written in thirty seconds between calls contains four compressed signals that an experienced operator would instantly decompose. It doesn't know that a quiet period in one context means "stable" and in another means "they're talking to your competitor."

This package helps you capture the knowledge that lives in your best people's heads — not what they decide, but how they think about decisions — and structure it so agents can apply it at scale.

Five skills covering the full knowledge capture process.

---

## Skills

### 01 — Knowledge Mapping
Identify what your organisation knows that isn't in the data. Tribal knowledge, implicit process knowledge, interpretive expertise, domain-specific semantics. Map each piece to the agents that need it and the agents that must not have it.

### 02 — Reasoning Frameworks
Encode how your best people think about data. Not rules ("if X, do Y") — reasoning structures. What factors they consider, what signals carry weight, how they read ambiguity, what good looks like. The interpretive lens that transforms data retrieval into genuine judgment.

### 03 — Data Reading Guides
Teach agents how to interact with your actual systems. Query mechanics, field trust hierarchies, empty vs missing distinction, pagination handling, internal identifier resolution, freshness rules. The operational knowledge that prevents the quiet hallucinations — not fabrication, but confident misreading of real data.

### 04 — Interpretation Rules
Define the rules for confidence, freshness, absence-as-signal, and conflicting data. Without these, everything in the output carries equal weight — a signal from one CRM note gets the same treatment as a trend confirmed across twenty data points.

### 05 — Depth Control
Design when agents apply frameworks lightly versus fully. A nurse taking vitals and a diagnostician running differentials use the same textbook at different depths. Without depth control, every query triggers the full analytical toolkit — ask a simple factual question and you get a risk assessment nobody asked for.

---

## Reference Documents

- **Institutional Knowledge Types** — the categories of knowledge that live outside structured data
- **Knowledge-to-Agent Mapping** — mapping knowledge to agents while controlling access
- **Reasoning Framework Structure** — anatomy of a well-structured reasoning framework
- **Data Reading Guide Structure** — anatomy of a well-structured data reading guide
- **Expert Knowledge Extraction** — how to extract the interpretive lens from domain experts
- **Quiet Hallucination Patterns** — how agents misread systems without operational guidance
- **Interpretation and Confidence** — confidence calibration, freshness rules, absence-as-signal
- **Framework Depth Control** — variable-depth framework application
- **Output Structure by Mode** — how structures enforce mode boundaries

---

## How to Use This

Start with Knowledge Mapping to identify what needs capturing. Then use Reasoning Frameworks and Data Reading Guides to structure it — they handle different types of knowledge (domain interpretation vs system interaction) that change for different reasons and at different rates. Layer on Interpretation Rules and Depth Control to refine how agents apply what they've been given.

---

## Install

### Claude Code (as a plugin)

```
/plugin marketplace add violetfleming47/agent-knowledge-curator
/plugin install agent-knowledge-curator@agent-knowledge-curator
```

### Claude Code (manual copy)

```bash
git clone https://github.com/violetfleming47/agent-knowledge-curator.git
cp -R agent-knowledge-curator/skills/* ~/.claude/skills/
```

### Claude apps (claude.ai, Desktop)

Download a skill's `.zip` from the [latest release](https://github.com/violetfleming47/agent-knowledge-curator/releases/latest) and upload it under Skills in Claude's settings.

### Other harnesses (Cursor, Windsurf, custom agents)

Each `SKILL.md` is plain markdown. Load it as a rule or system prompt, and keep its `references/` folder alongside it.

### What's included

Every skill is a self-contained folder under `skills/`, with a `SKILL.md` and the reference documents it draws on:

- `agent-knowledge-mapping`
- `agent-knowledge-reasoning-frameworks`
- `agent-knowledge-data-reading-guides`
- `agent-knowledge-interpretation-rules`
- `agent-knowledge-depth-control`

---

## About

Created by [Violet Fleming](https://github.com/violetfleming47). These tools are grounded in a design philosophy developed across production multi-agent systems.

Part of a suite of open-source agent design tools:
- [Agent System Design](https://github.com/violetfleming47/agent-system-design) — design and decompose agent systems
- **Agent Knowledge Curator** — capture institutional knowledge
- [Agent Drift Prevention](https://github.com/violetfleming47/agent-drift-prevention) — prevent silent degradation
- [Agent Edge Cases](https://github.com/violetfleming47/agent-edge-cases) — failure modes you haven't considered
- [Agent System Review](https://github.com/violetfleming47/agent-system-review) — architectural review of existing systems

Licensed under [CC BY-NC 4.0](LICENSE).
