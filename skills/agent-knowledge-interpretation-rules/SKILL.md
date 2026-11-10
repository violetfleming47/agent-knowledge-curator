---
name: agent-knowledge-interpretation-rules
description: Define interpretation rules for confidence, freshness, absence-as-signal and conflicting data so agents weight evidence correctly. Use when agent output treats every data point as equally certain.
---

# Interpretation Rules

> Define the rules for confidence, freshness, absence-as-signal, and when to doubt — so agents interpret data the way your best operators do.

---

## What This Skill Does

A reasoning framework teaches agents how to think. A data reading guide teaches them how to query. Interpretation rules sit between the two — they govern how the agent translates raw data into meaning. What confidence level to assign. How much to trust data based on its age. When an empty field is a finding versus when it's just empty. When two sources disagree, which to believe.

Without interpretation rules, agents treat all data equally. A signal from a single CRM note written in thirty seconds gets the same weight as a trend confirmed across twenty interactions over three months. A field filled eighteen months ago carries the same authority as one updated yesterday. An empty field becomes a gap finding in a factual report that never asked for gap analysis.

These subtle miscalibrations are what make agent output feel "off" without anyone being able to name why. Each individual data point is handled reasonably. But the weighting, the emphasis, the confidence behind each conclusion — it's all slightly wrong. The agent doesn't know what to take seriously and what to hold lightly.

Interpretation rules fix this by making the weighting explicit. They're the bridge between "I have data" and "I understand what this data is worth."

---

## Confidence Labelling

The most common problem: everything in the output carries equal weight.

An agent produces an analysis. The first finding is based on twelve corroborating data points from the last two weeks. The second finding is based on a single CRM note from four months ago. Both appear in the output with the same confidence, the same formatting, the same authoritative tone. The reader treats them equally. One deserves trust. The other deserves investigation.

Without explicit confidence labelling, the agent has no mechanism for distinguishing between these. It presents everything with maximum helpfulness, which means maximum confidence. "Findings" all look the same, regardless of the evidence behind them.

### What confidence means in your domain

Confidence shouldn't be vague hedging — "it seems like," "there may be," "possibly." Vague hedging sounds uncertain but doesn't tell the reader how uncertain or why. It's the worst of both worlds: the reader knows something is uncertain but can't calibrate how uncertain or make an informed decision about what to do with it.

Instead, define what confidence means specifically:

**High confidence (80-100%).** What does this look like in your domain? Probably: multiple corroborating signals from recent, reliable sources. Direct evidence, not inference. The conclusion would surprise no experienced operator. State the finding without qualification.

**Medium confidence (50-79%).** Some evidence but incomplete. Signals pointing in the same direction without conclusive proof. Some inference involved. An experienced operator might agree or might want more data before acting. State the finding with explicit uncertainty — what was observed, what's missing, what would increase confidence.

**Low confidence (20-49%).** Single data points, aged data, indirect inference, or conflicting signals. A hypothesis, not a conclusion. Useful for flagging something worth investigating, not for decision-making. Describe what was observed and what would need to be true for the hypothesis to hold.

**Insufficient (<20%).** Not enough information to form even a hypothesis. State what was observed and what would be needed to assess further. This is a valid, useful output — much better than a forced low-confidence guess that gets treated as a finding.

### Express confidence as a percentage, not a tier

Binary confidence (yes/no) or three-tier confidence (high/medium/low) hides the distribution. There's a meaningful difference between 42% and 78%, but both might be "medium." There's a meaningful difference between 81% and 99%, but both are "high."

When confidence is a percentage, downstream agents can apply it mathematically. The scoring agent says 73% confidence on the audience. The format agent can factor that in — maybe choosing a broader format for medium-confidence targeting rather than a highly persona-specific format.

When nothing scores above a meaningful threshold, the agent says so. A scoring agent where no audience profile exceeds 40% confidence flags low relevance rather than forcing a fit. That honest low score is more useful than a forced high one, because every downstream agent can adjust.

---

## Freshness Rules

Data ages, and how much that matters depends entirely on what kind of data it is.

A competitive pricing signal might be stale in six weeks — markets move. A macro-economic trend might be relevant for two years. A client's stated priorities might hold for the duration of a project or shift overnight after a single leadership change. A regulatory constraint might be permanent until the law changes, which could be never or could be next month. A contact's email address is stable for years. A deal's stated next steps might be stale within a week.

Blanket freshness rules ("data older than 90 days is stale") mishandle all of these simultaneously. The competitive signal should have been flagged at six weeks. The macro-economic trend is fine at eighteen months. The regulatory constraint doesn't expire on a calendar.

### Per-data-type freshness windows

Define freshness thresholds for each type of data your agents work with. Not blanket rules — specific windows:

- Market signals: depends on urgency classification. Time-sensitive signals expire in days. Developing trends hold for months.
- Client priorities: valid until a change event (leadership change, reorg, strategy shift). Between events, could be months.
- Competitive positioning: weeks to months, depending on market velocity.
- Operational metrics: current quarter is live data. Last quarter is context. Older is historical.
- Contact information: stable for years in some roles, months in others.
- Regulatory data: permanent until an external event changes it.

### How freshness modifies interpretation

**Within the freshness window:** Use at full weight. The data is current enough to trust.

**Approaching the boundary:** Flag with a freshness note. "This was captured three months ago — approaching the end of its typical relevance window. Consider whether conditions have changed." The agent doesn't discard the data — it contextualises it.

**Past the window:** Treat as historical context, not current intelligence. Available for comparison and trend analysis, not for current-state assessment. "This competitive analysis is from Q1. The current competitive landscape may have shifted."

### Active use extends freshness

Data that's been recently used productively — cited as evidence in content, confirmed by a human, referenced in a successful output — gets extended relevance. Active use is a reinforcement signal. The system is saying: "someone used this recently and it was useful, so it's probably still current."

Data that sits unused decays faster. If nobody's referenced a signal in months, it's more likely to be outdated than a signal that was cited last week.

---

## Absence-as-Signal

This is the most dangerous interpretation pattern in agent systems, and it deserves detailed treatment because it's where the most confident-looking mistakes happen.

An empty CRM field becomes a "gap." A quiet period becomes "stalling." A missing relationship becomes "single-thread risk." A missing budget field becomes "budget unknown — risk." The agent sees nothing, interprets it as something, and presents the interpretation with the same confidence as findings based on actual data.

Sometimes this reasoning is correct. That's what makes it dangerous — it's not always wrong, so you can't just prohibit it.

### When absence IS a signal

In diagnostic or analytical contexts, absence can be genuinely meaningful:

- No executive engagement after three months of an enterprise deal → potential risk signal (in a context where executive engagement is expected)
- No competitive mention in any recent call transcript → either no competition (good) or the customer isn't sharing (worth investigating)
- No activity for 30 days on a previously active account → pattern change worth understanding

In these cases, the absence is meaningful because the *presence* would be expected. The agent has enough context to know what should be there, and its absence is information.

### When absence IS NOT a signal

In factual or reporting contexts, absence is just empty data:

- No value in "last_contacted" → nobody logged it, not "no contact occurred"
- No value in "budget" → field wasn't filled in, not "no budget exists"
- No competitor listed → nobody entered one, not "no competitive threat"
- No next steps documented → rep uses a different tracking method, not "no plan"

In these cases, the empty field tells you nothing about reality. It tells you something about data entry practices.

### The rules

Absence-as-signal reasoning should only be permitted when:

1. **The context is explicitly diagnostic or analytical.** Not factual reporting. Not status updates. Contexts where the agent has been asked to assess, evaluate, or diagnose.
2. **The specific field is one where absence has been defined as potentially meaningful.** Not every empty field. Only the ones where presence would be expected and absence is informative.
3. **The agent expresses the interpretation with appropriate uncertainty.** Not "there's a gap" but "this field is empty — in a diagnostic context, this could indicate X. However, it could also mean the data wasn't captured."
4. **The agent also states the alternative explanation.** Always present the non-signal interpretation alongside the signal interpretation. Let the downstream agent or human weigh both.

Default: absence means "we don't know." Not "doesn't apply." Not a negative finding. Not a risk signal. We don't know.

---

## Conflicting Data

When two sources disagree, the agent's natural instinct is to pick whichever supports its narrative — the one that makes the analysis most coherent. This is the same rationalisation mechanism that affects monolithic agents: the model finds a story that works and selects the data that fits.

### Present conflicts explicitly

"The structured deal stage field says 'Negotiation.' The most recent note from two days ago says 'they've gone back to evaluation after the leadership change.' These conflict. The note is more recent and provides context for why the stage may have changed — but the structured field hasn't been updated."

Don't quietly resolve the conflict by picking one. Surface it.

### Define source hierarchy

When you need a resolution (because the downstream agent can't work with a contradiction), define which source to trust:

- Most recently updated → more likely to reflect current reality
- Notes over structured fields → notes capture nuance that structured fields can't
- Structured fields over notes → for data types where structured fields are rigorously maintained
- Cross-reference both → if they agree, confidence is high. If they disagree, flag.

The hierarchy may differ by field type and by system. Document it per data source, not as a blanket rule.

### Flag for human attention

Material contradictions that affect the output should be surfaced to the human, not quietly resolved by the agent. The agent can present both data points and its assessment of which is more likely current — but the resolution should be visible, not hidden inside the analysis.

---

## References

This skill loads:
- `references/interpretation-and-confidence.md` — confidence calibration, freshness rules, and absence-as-signal guidance
- `references/quiet-hallucination-patterns.md` — how agents misinterpret data without explicit interpretation rules

---

## You Will Need Next

Move to **Depth Control** to determine when agents should apply frameworks lightly versus fully — the difference between a nurse taking vitals and a diagnostician running differentials, using the same textbook at different depths.
