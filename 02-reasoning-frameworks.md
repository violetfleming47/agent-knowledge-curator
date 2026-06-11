# Reasoning Frameworks

> Encode how your best people think about data — not what they decide, but how they arrive at decisions.

---

## What This Skill Does

Rules encode decisions: "if X, do Y." They're the most common way people try to capture expertise for agents. And they're the most common reason the agents produce output that's technically correct and practically useless.

Rules are brittle. They work for the cases the rule-writer thought of and break on everything else. "If the deal is over $100K, escalate to a senior rep." What about a $95K deal with a Fortune 500 company? The rule didn't account for it. A junior analyst follows the rule and misses the opportunity. A senior rep would never follow that rule — they'd look at the company, the potential, the relationship, and make a judgment call.

Frameworks encode reasoning. Not "if X, do Y" but "here are the factors that matter, here's how to weigh them, here's what signals carry weight, here's what a confident conclusion looks like versus an uncertain one." When an agent operates against a framework, it doesn't follow a script. It reasons against a standard. And when it encounters the $95K deal from the Fortune 500 company, it has the interpretive structure to recognise that deal size isn't the only factor.

This skill helps you extract the reasoning from your best people's heads and structure it so agents can apply it at scale. This is the hardest work in the entire knowledge capture process. It's also the most valuable.

---

## Why Rules Fail and Frameworks Hold

The difference becomes stark when the data is messy — and in production, the data is always messy.

A CRM note written by a busy salesperson in thirty seconds between calls: "Met with Sarah and new CFO, budget review pushed to Q3, they're also talking to CompetitorX about integration." That's one note. It contains at least four distinct signals: a stakeholder change (new CFO), a timeline shift (budget pushed to Q3), a competitive threat (CompetitorX), and an implied process change (integration evaluation). A senior account manager reading this note sees all four immediately and knows which ones matter.

A rule-based approach would say: "If note contains competitor mention, flag as competitive threat." That catches one signal and misses three. You could write more rules — rules for stakeholder changes, rules for timeline shifts, rules for process changes. But the number of rules explodes, and every new rule interacts with every other rule in ways you can't predict. Rule systems become fragile labyrinths that nobody wants to maintain.

A reasoning framework says: "Notes are shorthand from busy people. A single entry might contain multiple distinct signals. Decompose each one. Classify it — urgency, risk, opportunity, competitive, stakeholder, timeline. Assess confidence based on the source, the directness of the information, and the recency. Note what you're sure about and what you're inferring. If you're not sure, say what you observed and what you couldn't determine."

The framework doesn't specify every possible situation. It teaches the agent *how to think about* any situation. It gives the agent the interpretive structure to handle the messy, varied, unpredictable data that real systems produce — including situations the framework designer didn't anticipate.

---

## How to Extract the Reasoning

The knowledge you need lives in your best people's heads. Not as a document they can hand over — as an interpretive lens they apply automatically, often without being aware they're doing it. The senior account manager doesn't think "I'm now applying my stakeholder composition analysis framework." They just see the data and notice things.

Making this implicit expertise explicit is genuinely difficult. Here's what works:

### Work through real examples

Don't ask "how do you evaluate accounts?" in the abstract. Put real data in front of the expert and ask them to think out loud.

"Here's a CRM record. What do you notice? What concerns you? What seems fine? What would you look at next? Why?"

Watch for what they look at first (reveals their priority hierarchy), what they skip (reveals what they've learned is noise), what they cross-reference (reveals which data layers they trust and which they verify), where they pause (reveals where interpretation is required), and what they infer that isn't explicitly stated (reveals the patterns they've learned to recognise).

This produces richer material than any interview question. You're observing the interpretive lens in action, not asking someone to describe it after the fact.

### Compare junior and senior readings

Show the same data to a junior and a senior operator. The differences in their observations are the framework:

- The junior reports what the data says. The senior reports what it means.
- The junior sees one signal. The senior sees four compressed into the same note.
- The junior takes the data at face value. The senior checks freshness, cross-references, and adjusts for context.
- The junior treats all data equally. The senior weights by source reliability and recency.
- The junior produces a factual summary. The senior produces a contextualised assessment with confidence levels.

Those differences are exactly what needs to be encoded. Every gap between junior and senior interpretation is a framework rule waiting to be written.

### Capture corrections

When a senior operator corrects an agent's output or a junior's analysis, the correction is a direct learning signal:

- What was wrong?
- Why is the correction right?
- What interpretive rule was missing?

Each correction is one observation. Patterns of corrections across multiple instances reveal the framework rules that need to be encoded. This is actually the same mechanism the learning system uses at runtime — but during the knowledge capture phase, you're bootstrapping the framework from historical corrections rather than waiting for the learning system to accumulate them over time.

### Ask about edge cases

Standard cases don't reveal much — the junior gets them right too. The framework lives in the hard cases:

- "When do you override the standard methodology?"
- "What makes you doubt the data?"
- "When do you escalate instead of proceeding?"
- "What signals make you worried even when the numbers look fine?"
- "When is absence of information actually informative?"

These edge cases are where the expert's judgment framework lives — the rules that handle the 20% of situations that account for 80% of the value.

---

## What a Reasoning Framework Contains

Not a document of rules. A structured guide to thinking:

### Domain context

What this agent is reasoning about and why it matters. Not instructions — orientation. The agent should understand the landscape before it starts interpreting data points.

"You are interpreting signals from client accounts. These signals help the business understand whether relationships are healthy, shifting, or at risk. Your interpretations feed into content targeting, outreach decisions, and strategic planning. Accuracy matters because downstream agents build on your assessments — a misread here compounds through the entire pipeline."

### Signal taxonomy

What kinds of things the agent might find. Each category has a definition, significance indicators, interpretation guidance, and common misreadings.

For client intelligence: urgency signals, risk signals, opportunity signals, sentiment shifts, stakeholder changes, competitive threats, buying signals, expansion signals. For each one: what does it look like in the data? What makes it significant? What context changes the interpretation? How do agents typically misread it?

The taxonomy gives the agent a structured way of seeing. Without it, the agent interprets through whatever analytical lens it finds most interesting — usually the one you didn't want. With it, the agent has categories that guide its attention to the things that matter in this domain.

### Confidence calibration

What confidence means in this specific domain, with explicit criteria. Not vague labels — defined, measurable standards:

**High confidence (80-100%):** Multiple corroborating signals from recent, reliable sources. Direct evidence, not inference. The conclusion would surprise no experienced operator.

**Medium confidence (50-79%):** Some evidence but gaps. Signals pointing in the same direction without conclusive proof. Some inference involved. An experienced operator might agree or might want more data.

**Low confidence (20-49%):** Single data points, aged data, indirect inference, or conflicting signals. A hypothesis, not a conclusion. Useful as a starting point for investigation, not for decision-making.

**Insufficient (<20%):** Not enough to form even a hypothesis. State what was observed and what would be needed to assess further. This is a valid output — better than a forced low-confidence guess.

### Decomposition rules

How to break unstructured data into classified signals. This is where the framework handles the messiness of real data:

"A single note entry from a busy operator might contain four or five distinct signals compressed into a few sentences. Decompose each one. Classify it. Interpret it in context. A note that says 'Met with Sarah and new CFO, budget review pushed to Q3, they're also talking to CompetitorX about integration' contains: a stakeholder change (new CFO), a timeline shift (budget pushed), a competitive signal (CompetitorX), and an implied process change (integration evaluation). Each should be classified and assessed separately."

"Write what the signal means, not what was written. Notes are shorthand. The agent's job is to interpret, not to parrot."

### Attribution guidance

How to link signals to the right people and assess their significance based on role:

Who said it? What's their role — decision-maker, influencer, champion, detractor, observer? Is this their pain or someone else's they're reporting? How reliable is their perspective? A champion reports favourably. A detractor reports unfavourably. Neither is lying — they're seeing through their own lens.

### Uncertainty guidance

How to express partial knowledge usefully. Not "say you don't know" — a structured way to communicate what was observed, what couldn't be determined, and what conditions would need to hold for the hypothesis to be correct.

This is the part that distinguishes a framework from a set of rules. Rules produce clean outputs — they force data into categories even when the data doesn't fit. Frameworks produce honest outputs — they guide the agent's thinking and allow it to express uncertainty as structured, useful information rather than vague hedging.

---

## Validation

Once you've structured a framework, validate it:

With the expert: "Does this capture how you think about it? What's missing? Where would this framework get it wrong? What situations wouldn't this handle?"

With data: run the framework against historical examples where the expert's judgment is known. Does the framework produce assessments that align with what the expert concluded? Where they diverge, the framework needs refinement.

The framework will never be perfect on day one. That's what learning cycles are for — the framework evolves through accumulated observations and human corrections over time. But it needs to be good enough that the agent's reasoning is recognisably similar to the expert's, even if it's not yet as nuanced.

---

## References

This skill loads:
- `references/reasoning-framework-structure.md` — the anatomy of a well-structured reasoning framework
- `references/expert-knowledge-extraction.md` — how to extract the interpretive lens from domain experts

---

## You Will Need Next

Move to **Data Reading Guides** to encode the operational rules for interacting with your systems — a fundamentally different kind of knowledge that changes for different reasons and at different rates. Then to **Interpretation Rules** for the specific guidance around confidence, freshness, and absence-as-signal that sits between the frameworks and the data.
