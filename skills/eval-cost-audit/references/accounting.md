# Accounting and evidence

## Field discovery

Inspect actual exported attributes, including whether keys are flattened or nested. Possible OpenInference-style fields include `llm.cost.total`, `llm.cost.prompt`, `llm.cost.completion`, their detail fields, and `llm.token_count.*`. These are discovery hints, not a guaranteed schema or a currency specification.

Build a mapping from each source field to its meaning, unit, missing-value behavior, and inclusion relationship. Verify whether prompt tokens include cached tokens and whether cost details are components of or additional to parent totals. Preserve raw source values in a sanitized ledger.

Never silently equate an absent counter with zero, a negative value with a refund, or a non-finite value with valid spend. Flag invalid records and disclose their treatment. A valid zero must remain zero. Keep provider/model/configuration cohorts separate where accounting differs.

## Cost reconciliation

- Prefer recorded per-call totals when present, but label them as recorded telemetry rather than a provider invoice.
- Sum costs once per chosen judge LLM execution span. Do not add ancestor trace totals or overlapping rollups to child LLM costs.
- Sum component costs only when their semantics establish that they are disjoint. Compare with the total using a tolerance justified by source precision; disclose the tolerance and any unexplained residual. Never invent a component to force a match.
- If cost is absent, a calculated cost requires known model/provider, token categories, units, and an applicable price source/date. Identify published-price calculations separately from recorded costs and explain any unknown contract discounts.
- Never blend currencies. State currency, and mark it unknown if it cannot be established.
- Missing cost records contribute to missingness counts, not a zero-cost claim. State cost coverage over identified judge calls.

For one aligned cohort and one token category:

```text
effective cost per million tokens = category cost / category token count × 1,000,000
mean recorded cost per call = sum(valid recorded call costs) / calls with valid costs
observed cost per evaluated object = attributed cost / distinct identified objects
```

The first formula gives an observed effective rate, not necessarily list pricing. Do not divide total call cost by prompt tokens and label the result an input tariff. Zero denominators yield unavailable, not zero. Unresolved evaluator attribution limits evaluator-specific comparisons.

For example, 100,000 input tokens at $2 per million imply $0.20 input cost. If telemetry reports $0.002, reconcile the source units and counters before using either figure. Do not silently adjust a factor of 1,000 to make a report plausible.

## Cache measures

Report each measure with its eligible-record count and definition:

| Measure | Formula / requirement |
|---|---|
| Cached-input token share | Cache-read input tokens / total input tokens, only if total includes those tokens |
| Request-level hit fraction | Requests with cache-read tokens > 0 / requests with known cache-read status |
| Cache-read cost share | Recorded cache-read cost / compatible total prompt cost |

If token categories overlap or cache status is missing, the corresponding measure is unavailable. Report the measured subset. Cost share cannot establish token or request hit rate because unit prices differ. Do not infer cache retention, cross-session reuse, or cache-write premiums from these ratios alone.

## Attribution and counterfactuals

Record evaluator identity/version and evaluated object identity separately from execution trace/span/session identity. Several judges may validly evaluate one object. Several LLM spans may belong to one evaluation. Do not call these duplicates without configuration and execution evidence.

Measure component token overhead directly or compare otherwise equivalent inputs. Identical message counts do not control message lengths, system prompts, tool results, or histories. Tool count reduction is not proportional token reduction unless measured.

A savings scenario must identify affected calls, baseline accounting, proposed change, assumed token/model/cache behavior, additional execution costs, and overlap with other scenarios. Observed cost associated with a candidate is not automatically avoidable cost. Do not call sampled savings monthly savings merely because the query spans a month.
