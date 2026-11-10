---
name: agent-knowledge-mapping
description: Identify the institutional knowledge your agents need that isn't in the data — tribal, process, interpretive and semantic knowledge — and map which agents must have it and which must not. Use when starting to encode expert judgment into an agent system.
---

# Knowledge Mapping

> Identify what your organisation knows that isn't in the data — the tribal knowledge, implicit processes, and accumulated expertise your agents need to reason properly.

---

## What This Skill Does

There's a moment in every agent project where the team says: "We've got the data connected. The agent can read the CRM, pull market signals, access the knowledge base. Why is the output still... off?"

It's off because context engineering — getting the right data into the agent's context window — solves only half the problem. The agent has the data. It can read every field, pull every record, access every table. What it can't do is *understand* the data the way your experienced people do.

Your senior account manager reads a CRM note and sees alarm bells. Your junior reads the same note and sees a routine update. Same data. Completely different understanding. The difference isn't the data — it's the interpretive lens the senior brings to it. The knowledge that "when a customer says 'we're evaluating options,' it means they already have a shortlist." The awareness that "this field is unreliable because the sales team fills it in differently depending on who's entering it." The instinct that "a quiet period in this account context means they're talking to your competitor, not that everything's fine."

That interpretive lens is institutional knowledge. It lives in your best people's heads. It was never written down because nobody needed to write it down — experienced operators just *know* it. But your agent doesn't know it, and without it, the agent is processing data, not understanding it.

This skill helps you find that knowledge and map it — before you try to structure it into frameworks and guides (that's what the other skills in this package are for).

---

## Why This Matters More Than You Think

Most agent projects fail not because the technology doesn't work, but because the agents don't know what they don't know. The agent queries the CRM, gets results, and produces analysis. The analysis looks confident and well-structured. But it's based on a naive reading of the data — treating every field at face value, every record as complete, every label as accurate.

Your experienced operators would never do that. They know which fields to trust and which to cross-reference. They know what an empty field means in one context versus another. They know that the same deal stage label can mean completely different things depending on which pipeline the record is in. They know that a certain customer's stated priorities are aspirational, not actual. They know that one data source is authoritative for pricing but unreliable for timeline.

None of this knowledge is in the data. It's *about* the data. And until you capture it and give it to your agents, your agents are doing the equivalent of handing a junior analyst a spreadsheet and saying "tell me what's happening" — technically correct queries producing subtly wrong conclusions.

---

## The Categories of Knowledge That Live Outside Your Data

### Tribal knowledge

The things everyone on the team knows but nobody documented. This is the most dangerous category because it's invisible — it only becomes apparent when someone who doesn't have it makes a mistake.

"Acme Corp is in a sensitive renegotiation — handle carefully." "We never discount below 15% except for multi-year contracts." "When a customer says 'we're evaluating options,' it means they already have a shortlist." "That CRM field is unreliable because sales reps fill it in differently." "We tried that approach with their industry last year and it backfired."

Tribal knowledge is the first thing that walks out the door when someone leaves. And it's the first thing an agent needs when it starts interacting with your data. Without it, the agent treats every customer the same, queries every field with the same trust, and interprets every signal at face value. It's the most technically capable junior analyst you've ever hired — brilliant at processing data, clueless about context.

### Implicit process knowledge

The gap between the documented process and what people actually do. Every organisation has this gap, and it's usually wider than anyone admits.

The documented pricing methodology says one thing. What actually gets approved is different. The org chart says decisions flow one way. They actually flow a different way. The qualification methodology has seven steps. Experienced reps assess three key signals and know the answer before step four.

This matters because an agent following the documented process will do technically correct things that experienced operators would never do. It'll apply the full pricing methodology when a senior rep would just check three fields and make the call. It'll route a decision through the documented escalation path when everyone knows that Sarah in operations is who actually resolves things quickly.

If your agents are following the process as documented and the output feels bureaucratic or slow, this is why. They're following the official process. Your people follow the real one.

### Interpretive expertise

How experienced operators read the same data as juniors and see completely different things. This is the highest-value knowledge to capture because it's what transforms an agent from data retrieval into genuine reasoning.

The senior account manager reads a CRM note and hears three distinct signals compressed into one sentence: a timeline shift, a competitive threat, and an unstated budget concern. The junior reads the same note and sees one update: "meeting went well, follow-up scheduled."

The senior reads a quiet period in one account and knows it's fine — they're in a stable phase, nothing unusual. The senior reads a quiet period in another account and gets worried — this customer goes quiet when they're talking to competitors.

The senior cross-references instinctively. They check the structured field, then the latest note, then the activity log. If they disagree, they know which to trust and why. The junior trusts whatever they query first.

That interpretive expertise is the raw material for reasoning frameworks. Capturing it is the hardest and most valuable part of this process.

### Domain-specific semantics

What words and concepts mean in your specific context, which is often different from their general or industry meaning.

"Churn" in telecom (cancellation) versus SaaS (non-renewal) versus insurance (policy lapse). "Qualified lead" between sales teams at the same company. "Stage 3" in your pipeline versus the industry standard definition. "Enterprise" as a customer segment versus "Enterprise" as a product tier.

Agents trained on general language will apply general meanings. They'll confidently use the wrong definition of "qualified" because their training data has a different definition than your business does. Unless you make the domain-specific meaning explicit, the agent will process data correctly using the wrong vocabulary.

---

## How to Use This

Describe your domain, your systems, and the decisions your agents need to make. I'll help you map:

### What knowledge exists and where it lives

For each piece of institutional knowledge, we'll identify where it currently lives:

- **In people's heads** — the most common and most dangerous location. One departure and the knowledge walks out the door. One agent project and nobody thinks to transfer it.
- **In scattered documents** — wikis, policy docs, runbooks, Slack threads, email chains. The knowledge exists but it's not in a form agents can use, and half of it is outdated.
- **In process behaviour** — encoded in how people actually work, never written down. The shortcuts, the cross-references, the instinctive data quality checks.
- **In historical decisions** — "we did it this way last time because..." with the reasoning lost to time. The decision is documented. The judgment behind it isn't.

### Which agents need which knowledge

Not every agent needs all the knowledge. This is critical. Map each piece of knowledge to the agents that require it — and equally important, to the agents that must NOT have it.

A scoring agent needs ICP definitions and signal interpretation rules. It does not need pricing rules or contract terms — if it can see those, it will factor them into scoring, which is not its job. A validation agent needs quality criteria and brand rules. It does not need audience frameworks — the scoring agent already made that decision.

The mapping produces a matrix: knowledge × agents. Every cell is either "loads this" or "structurally cannot access this." The empty cells matter as much as the filled ones. A model with a framework it doesn't need will use it. The analytical lens creates the finding, not the other way around.

### What form each piece of knowledge needs to take

Not all knowledge becomes the same thing:

- Some becomes a **reasoning framework** — how to think about what you find. The interpretive expertise, the judgment, the "how my best person reads this data." Move to the Reasoning Frameworks skill for this.
- Some becomes a **data reading guide** — how to interact with your systems correctly. The query rules, the field trust hierarchy, the pagination handling. Move to the Data Reading Guides skill for this.
- Some becomes **reference data** — loaded into a database, queried by agents when needed. ICP profiles, content pillars, brand rules.
- Some becomes **skill rules** — embedded directly in the agent's scope definition as procedural steps, noise-naming, or prohibitions.

The distinction between reasoning frameworks and data reading guides deserves special attention. They're both "knowledge that agents need" but they come from different sources, they're written by different people, and they change for different reasons. Your query pagination rules don't change when your sales methodology evolves. Your signal taxonomy doesn't change when you add a new API endpoint. Keeping them separate means you can update one without touching the other.

---

## What a Complete Knowledge Map Looks Like

A finished knowledge map specifies:
- Every piece of institutional knowledge your agents need to reason properly
- Where it currently lives (people, documents, process, decisions)
- Which agents need it and which must not have access to it
- What form it should take (reasoning framework, data reading guide, reference data, skill rule)
- Who owns it and how it stays current
- What would trigger a review (system change, process change, data source change, learning proposal)

The map itself is the deliverable. It tells you what needs to be authored before your agents can reason the way your best people do. Without it, you're guessing at what knowledge matters — and guessing is how you end up with agents that query confidently and conclude wrongly.

---

## References

This skill loads:
- `references/institutional-knowledge-types.md` — the categories of knowledge that live outside structured data
- `references/knowledge-to-agent-mapping.md` — how to map knowledge to agents while controlling access

---

## You Will Need Next

With your knowledge mapped, move to **Reasoning Frameworks** to encode how your best people think, and **Data Reading Guides** to encode how to interact with your systems correctly. These are the two most important outputs of the knowledge mapping process, and they serve fundamentally different purposes.
