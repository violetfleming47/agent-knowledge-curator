# Depth Control

> When to apply a framework lightly versus fully — because a nurse taking vitals and a diagnostician running differentials use the same textbook at different depths.

---

## What This Skill Does

Ask an agent "what stage is this deal at?" You'll get a stage label. You'll also get a risk assessment, a qualification audit, a health score, three action items, and a paragraph about strategic alignment. You asked a factual question. The agent delivered a full diagnostic.

This isn't the agent being overly helpful. It's the agent doing what it was designed to do — apply its analytical frameworks to the data in front of it. The problem is that it has one speed: full depth. Every query triggers the entire analytical toolkit. The nurse and the diagnostician are the same person, and they always run full differentials, even when you just want blood pressure.

The result is over-interpretation at scale. Users learn to distrust the output because the analysis feels disproportionate to the question. Or worse — they trust it, and start treating a casual factual query's risk assessment as a considered evaluation. The agent wasn't more careful about the risk assessment just because it was in a factual context. It applied the same framework at the same depth regardless of what was asked.

Depth control fixes this by making framework application variable. The same expertise — the same reasoning framework, the same data reading guide, the same interpretive lens — applied at different intensities for different purposes. The textbook doesn't change. How deeply you read it does.

---

## Why This Happens

Models are trained to be comprehensive. When a model has analytical tools loaded — a qualification framework, a risk assessment rubric, a scoring methodology — and data in front of it, it applies the tools. All of them. At full depth. Because that's what produces the most thorough response, and thoroughness is what the model was optimised for.

This is the same mechanism as scope drift, but in a different direction. Scope drift is the agent doing things it wasn't asked to do. Over-interpretation is the agent applying more depth than was asked for. Both come from the same root: the model filling every available space with capability.

And it's compounded by a design choice most teams make without thinking: loading the full framework for every query. The scoring agent always has the complete scoring methodology loaded. The account analyst always has the full qualification rubric. The content evaluator always has every assessment dimension active.

If the agent has a risk assessment framework loaded, it will find risks. If it has a scoring rubric loaded, it will produce scores. If it has a gap analysis toolkit, it will find gaps. The analytical lens creates the finding. The framework being present is what triggers the analysis, not the question being asked.

---

## The Solution: Application Modes

Define distinct modes for how deeply the agent applies its frameworks. The number of modes doesn't matter — what matters is that each mode has clear rules about what's active and what's not.

### Factual mode

The framework provides vocabulary and structure only. The agent knows what to call things and how to organise its response. It does NOT apply scoring, risk assessment, gap detection, or any analytical function.

"What stage is Deal X at?"

Factual mode response: "Deal X is in Negotiation stage. Last activity was 8 days ago. Key contact is Sarah Chen (VP Engineering). Next scheduled meeting is July 15."

That's it. No health score. No risk language. No "Key Gaps" section. No action items. The agent reported facts using the vocabulary from its framework (it knows what "Negotiation stage" means, what fields to check, how to identify the key contact) without applying the analytical components.

### Diagnostic mode

The full framework is active. Scoring logic, gap analysis, severity assessment, pattern detection, trend analysis, hypothesis formation. This is the mode for "what's wrong with Deal X?" or "should we be worried about this account?"

Diagnostic mode response: "Deal X shows three risk indicators. Contact frequency has declined 40% from the account baseline. The stakeholder composition has shifted from VP-level to Director-level in the last two meetings, which may indicate deprioritisation. No competitive intelligence has been captured in 60 days, which in an account this active is unusual. Combined confidence: 62%. Recommendation: schedule a strategic check-in at VP level."

Same data. Same framework. Completely different depth of application.

### Monitoring mode

The framework checks for deviations from established baselines. Not a full assessment — a targeted scan for changes that warrant attention.

"Has anything changed with Deal X since last review?"

Monitoring response: "Two changes since the last review. Stakeholder composition shifted (VP → Director level in last meeting). Contact frequency declined from weekly to biweekly. No other changes detected against baseline."

The agent isn't diagnosing. It's reporting deviations. If nothing has changed, the output is "no significant changes detected" — which is a valid, useful response, not a failure to produce analysis.

---

## How to Implement Depth Control

### Mode selection through routing

The cleanest approach: a routing layer classifies intent before the agent starts work. The classification determines which mode the agent operates in.

"Tell me about Deal X" → factual mode.
"What's wrong with Deal X?" → diagnostic mode.
"Has Deal X changed since last week?" → monitoring mode.
"Should we be worried about this account?" → diagnostic mode.
"What stage is Deal X at?" → factual mode.

The routing layer makes this call. The agent itself doesn't decide how deep to go — it operates in whatever mode it's been told to use. This prevents the agent from escalating to diagnostic depth because the data "looked interesting."

### Default conservative

When intent is ambiguous, default to the least analytical mode. Always assume the user wants facts, not analysis. Never default to the most analytical option.

This is counterintuitive because models are trained to be maximally helpful, and maximum helpfulness feels like maximum depth. But in practice, a factual response to an ambiguous question is almost never wrong — the user can ask for more depth. A diagnostic response to an ambiguous question frequently over-interprets, and the user may not notice the over-interpretation.

Default conservative. Escalate on request.

### Output structure as enforcement

This is the most reliable enforcement mechanism, and it's underused.

The output structure for factual mode simply doesn't have a "Risk Assessment" section. It doesn't have a "Key Gaps" section. It doesn't have a scoring table. The model can't produce a risk assessment if there's nowhere in the output structure for a risk assessment to go.

Compare this to instructions: "Don't produce risk assessments in factual mode." That instruction fights the model's drive to be comprehensive. The data is there. The framework is loaded. The model can see the risks. Telling it not to mention them is aspirational.

Removing the section where the risks would go is structural. The model produces what the structure accommodates. No section, no output. This is more reliable than any instruction.

### Framework loading by mode

The strongest form of depth control: different modes load different frameworks — or the same frameworks at different depths.

In factual mode: load the data reading guide (how to query correctly, what fields to check, what identifiers to resolve). Do NOT load the reasoning framework (how to interpret, assess, diagnose). If the agent can't access the risk assessment framework, it can't produce risk assessments. It doesn't have to resist the temptation — the analytical tools aren't in the room.

In diagnostic mode: load both the data reading guide and the full reasoning framework. The agent now has the analytical tools and should use them.

In monitoring mode: load baselines and the change-detection subset of the reasoning framework. Not the full analytical toolkit — just enough to identify deviations from established norms.

This is the same principle as framework loading in scope definition: an agent that can't access a framework can't misuse it. In depth control, you're applying this principle dynamically — loading different frameworks based on what the current mode requires.

---

## Risk Language: The Canary

The most reliable indicator that depth control is failing: risk language appearing in factual contexts.

"Last contact was 18 days ago" is a fact.
"Gone 18 days without engagement — risk signal" is an interpretation.

Same data. Very different framing. One belongs in a diagnostic assessment. The other belongs nowhere near a factual update.

Watch for risk language — words like "risk," "concern," "gap," "stalling," "decline" — appearing in outputs where the user asked a factual question. If they're showing up, the agent is applying diagnostic depth to factual requests. The framework is too active, the output structure isn't constraining enough, or the routing isn't distinguishing between modes.

Risk language in factual mode is the canary in the coal mine. Fix it, and most over-interpretation problems fix themselves.

---

## References

This skill loads:
- `references/framework-depth-control.md` — how to design variable-depth framework application
- `references/output-structure-by-mode.md` — how output structures enforce mode boundaries

---

## This Completes the Agent Knowledge Curator Package

You now have:
1. **Knowledge Mapping** — what institutional knowledge exists and which agents need it
2. **Reasoning Frameworks** — how to encode expert thinking into structured guides
3. **Data Reading Guides** — how to encode system interaction rules
4. **Interpretation Rules** — confidence, freshness, absence-as-signal, and conflict handling
5. **Depth Control** — when to apply frameworks lightly versus fully

To design your agents, use **Agent System Design**. To understand where your system is fragile, use **Agent Drift Prevention**. To challenge your assumptions, try **Agent Blind Spots**.
