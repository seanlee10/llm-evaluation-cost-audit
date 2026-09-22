# LLM Evaluation Cost Audit

A reusable skill for auditing LLM-as-judge costs in Arize. It focuses on evaluator attribution, reproducible accounting, and quality-preserving optimization experiments rather than duplicating general application cost analysis.

The repository contains instructions, not an agent runtime or a standalone API client. It neither provisions credentials nor schedules jobs itself.

## Skill

- [eval-cost-audit](skills/eval-cost-audit/SKILL.md)
- [Accounting rules](skills/eval-cost-audit/references/accounting.md)

## Use with Arize Managed Agents

1. Publish this repository to GitHub.
2. Register a Custom skill using the repository URL as its install source. Describe it as an audit of judge-call costs, token accounting, and quality-preserving savings experiments.
3. Verify that the runtime discovers `skills/eval-cost-audit/SKILL.md` and its relative reference. This folder layout follows the Agent Skills convention; Managed Agents' nested-repository discovery behavior has not been tested here.
4. Select the project containing judge execution traces. Verify that an `Eval Traces` project contains the intended workload before analyzing it.
5. Run a one-off session, inspect its evidence and arithmetic, then configure a cron automation with an agent config if recurring audits are desired.

Managed Agents provide short-lived read credentials for the selected project's traces. Additional evaluator-resource access can require an Arize integration with an appropriately scoped `ARIZE_API_KEY`. A model-provider key authenticates model calls, not trace access. Keep secrets in integrations, never in this public repository.

The skill uses authenticated tools available in the runtime. CLI availability, fields, project access, and artifact retention must be verified in the actual environment. No live Managed Agent integration test has been performed for this repository.

## Example task

```text
Use eval-cost-audit to audit the previous complete UTC day's LLM-as-judge
execution costs in the selected project. Start by verifying that these are
judge calls. Report evaluator/model breakdowns where attribution is available,
token distributions, and separately defined cache measures. Show accounting
checks and data coverage. Propose up to three evidence-backed savings
experiments with evaluation-quality checks. Do not change any configuration.
Return an English Markdown report with trace/span references.
```

If the project contains application calls instead, the expected result is a scope/data-gap report, not relabeled application cost findings.

## Public repository hygiene

Use synthetic examples for published fixtures. Keep exported spans, customer/project identifiers, generated reports, and credentials outside this repository. Sandbox-local artifacts may not survive the run; retain reports only through an explicitly configured destination.

References: [Managed Agents](https://arize.com/docs/ax/agents/build-your-own-agent), [authentication and skills](https://arize.com/docs/ax/agents/connect-your-harness), [built-in Cost Agent](https://arize.com/docs/ax/agents/cost-agent).
