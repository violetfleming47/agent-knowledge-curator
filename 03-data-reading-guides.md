# Data Reading Guides

> Teach agents how to interact with your actual systems — how to query, what to trust, what empty means versus missing.

---

## What This Skill Does

Reasoning frameworks teach agents how to think about what they find. Data reading guides teach them how to find it correctly in the first place — and how to avoid the quiet hallucinations that come from misreading your systems.

This distinction is important because these two kinds of knowledge are fundamentally different. Reasoning frameworks come from knowing the business — how experienced operators interpret signals, what patterns matter, how to assess confidence. Data reading guides come from knowing the system — its quirks, its limitations, the specific ways it will mislead an agent that doesn't understand its architecture.

Without data reading guides, agents hallucinate in a way that's particularly dangerous because it doesn't look like hallucination. The agent queries your CRM, gets real data, and misinterprets it. It assumes a single API call returned the full dataset when it only got the first page of twenty. It reports on a field without checking when it was last updated — and the field was filled in eighteen months ago, three deal stages ago, by someone who's since left the company. It displays an internal stage ID as if it were a human-readable label. It treats an empty field as a finding.

These aren't dramatic fabrications. They're compounding inaccuracies — each one small, each one plausible, and together they make the entire analysis unreliable. The output looks authoritative because it's derived from real data. It's still wrong. And nobody catches it because the mistakes are subtle enough to pass a quick review.

---

## What Quiet Hallucination Looks Like

Let me walk through some specific patterns, because understanding these at a gut level is what motivates the effort of writing data reading guides.

### Pagination blindness

The agent calls your API, receives the first page of results, and treats it as the complete dataset. "There are 25 contacts matching this criteria" — when there are actually 250. The agent read page 1 of 10 and never checked for more.

This compounds when the agent draws conclusions from the incomplete data. "The most common job title is VP Engineering" — true for the first 25 results, completely wrong for all 250. "We have no contacts in the healthcare vertical" — healthcare contacts are on page 6, which was never requested. Every conclusion is technically derived from real data and substantively wrong.

### Inference from training data

The agent recognises an entity and fills in details from its general knowledge instead of the actual record. It sees "Salesforce" as a company name and starts referencing Salesforce's general business model — not the specific account record with its specific deal history and its specific stakeholder map. It sees "Series B" and infers a company size from its training data, not from the actual data in your system.

This is insidious because the inferred details are usually plausible. Salesforce *is* a large CRM company. Series B companies *do* tend to be a certain size. The agent's general knowledge isn't wrong — it's just not the specific knowledge from this specific record. And the output blends the two seamlessly.

### Empty field interpretation

A null field in the CRM. The agent has to do something with it. Without guidance, it interprets:

- No value in "last_contacted" becomes "the customer hasn't been contacted" (maybe nobody logged it)
- No value in "competitor" becomes "no competitive threat" (maybe nobody asked)
- No value in "budget" becomes "budget unknown — risk signal" (maybe the field isn't used for this record type)
- No value in "next_steps" becomes "no clear next steps — stalling" (maybe the rep updates next steps in their own notes, not in the CRM)

Every empty field is an opportunity for the agent to invent meaning. Without a data reading guide that specifies what empty means for each field or category of fields, the agent defaults to treating absence as signal. And sometimes absence *is* signal — but the agent doesn't know when. So it treats every gap as a finding, because analytical writing trained it to treat noting gaps as thorough.

### Stale data confidence

A field filled eight months ago on a record that's been through three status changes. The agent reads it with the same confidence as yesterday's data. A competitive positioning statement from Q1 treated as current in Q3. A pricing benchmark from a market that's moved 20%. A client preference from a product evaluation that ended months ago.

There's no mechanism in the model to distinguish fresh from stale. The data looks exactly the same in context regardless of when it was entered. Without freshness rules, the agent produces high-confidence output based on historical data, and nothing in the output indicates the age of the underlying information.

---

## What a Data Reading Guide Contains

### Query mechanics

The practical rules for getting correct data out of your systems. Not "how to use the API" — but the specific operational knowledge that prevents misreads:

**Pagination.** Does this API return all results or paginated sets? What's the page size? How does the agent know whether it has the full dataset? Specify: always check for pagination indicators. Never assume a single call returned everything. If the first page is full, there are probably more pages.

**Filter limits.** What scoping is required before queries return meaningful results? In some systems, you need to scope to the right pipeline before stage labels are interpretable. In others, you need to filter by record type before field values make sense.

**Rate limiting.** How many calls before throttling? What does the agent do when throttled — wait, reduce scope, or flag for human attention?

**Standard query patterns.** Document the specific query shapes that are known to return correct results. The agent should use proven patterns rather than constructing queries from its understanding of the API. This is especially important for complex systems where the "obvious" query sometimes returns subtly wrong results due to system-specific behaviour.

### Field trust hierarchy

Not all fields in your systems are equally reliable. A data reading guide teaches the agent which to trust and which to verify:

**System-managed fields** — timestamps, auto-calculated values, workflow-generated statuses. Generally reliable, but check whether the inputs are current. An auto-calculated score based on stale inputs is precise but wrong.

**Structured human input** — dropdowns, categorised fields, standardised entries. Reliable within the constraints of the options available, but humans sometimes pick the closest option rather than the right one because the right option doesn't exist.

**Free-text human input** — notes, descriptions, custom fields. Entered by humans with varying discipline, varying terminology, and varying completeness. Must be interpreted, not quoted. Cross-reference against structured fields when possible, but don't assume the structured field is right and the note is wrong — sometimes the note is more current.

**Derived or calculated fields** — values computed from other fields. Only as good as the source fields and the calculation logic. A "deal health score" that's calculated from fields that haven't been updated in months is a precise calculation of a stale picture.

### Empty vs missing

The single biggest source of quiet hallucination. This deserves field-by-field or category-by-category specification:

**What empty means for this field.** Is it "nobody filled it in" (no information), "never relevant" (not applicable), "relevant but not captured" (gap in process), or "deliberately left blank" (conscious decision)?

**Whether absence should ever be treated as a signal.** And under what conditions. An empty "competitor" field in a diagnostic context might be worth noting. The same empty field in a factual report is just empty.

**The default interpretation.** When context is ambiguous: "we don't know." Not "doesn't apply." Not a negative finding. Not a gap to flag. We simply don't have this information.

### Data freshness

**Check timestamps.** When was this field last updated? When was this record last touched? Don't just check the record-level timestamp — check field-level modification dates if they're available.

**Define freshness thresholds.** Per data type. A contact's email is stable for years. A deal's next steps might be stale in a week. A competitive positioning statement might be outdated in a quarter. A market signal depends on its urgency classification.

**Flag staleness.** Don't silently present aged data as current. "This was captured four months ago and hasn't been updated since — treat with appropriate caution." That's more useful than a confident statement built on stale data, even when the stale data happens to still be accurate.

### Internal identifiers

Internal codes look plausible as labels but mean different things. Stage IDs, category codes, status integers. Specify which fields contain identifiers, how to resolve them to human-readable form, and what to do when resolution fails: say "I can't resolve this identifier" — never display raw codes as if they're meaningful.

Some identifiers are context-dependent — the same code means different things in different pipelines, different record types, or different business units. Scope before interpreting. Always.

---

## Data Reading Guides Are Not Reasoning Frameworks

This distinction matters enough to state explicitly: these are different kinds of knowledge that change for different reasons.

Data reading guides change when the system changes: new APIs, new fields, deprecated fields, schema migrations, new integrations. They're written by people who know the technology — engineers, system administrators, data architects.

Reasoning frameworks change when the business changes: new customer segments, new qualification criteria, new signal patterns, evolved market conditions. They're written by people who know the domain — senior operators, business leaders, subject matter experts.

An agent can load a data reading guide without a reasoning framework — when it just needs to pull accurate data. And a reasoning framework without a data reading guide — when it's interpreting data that's already been pulled and cleaned. Keeping them separate means you can update your query patterns without touching your interpretation rules, and vice versa.

---

## How to Use This

Describe your systems, data sources, and how agents will query them. I'll help you build guides that cover query mechanics, field trust, empty field interpretation, freshness rules, and identifier resolution — all specific to your actual systems, not generic best practices.

The output is a document (or set of documents) that each agent loads as part of its reference material. The data reading guide sits alongside the reasoning framework but serves a completely different purpose: the guide ensures the data is accurate. The framework ensures the interpretation is sound. Both are required. Neither substitutes for the other.

---

## References

This skill loads:
- `references/data-reading-guide-structure.md` — the anatomy of a well-structured data reading guide
- `references/quiet-hallucination-patterns.md` — the specific ways agents misread systems without operational guidance

---

## You Will Need Next

Move to **Interpretation Rules** for guidance on confidence, freshness, and absence-as-signal across your system, and **Depth Control** for when to apply frameworks lightly versus fully.
