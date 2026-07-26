---
name: craft-cloud-competitive-check
description: Run focused, current competitive research to inform product-shaping Craft Cloud planning, architecture, admin UI, documentation, and PR review. Use when evaluating a new or changed user-facing feature, workflow, platform convention, price, quota, or plan boundary; when reviewing product-shaping Craft Cloud work; or when explicitly asked to compare Craft Cloud with Servd, fortrabbit, Laravel Cloud, Heroku, or another PaaS. Skip narrow bug fixes whose intended contract is already known.
---

# Craft Cloud Competitive Check

Use competitor evidence to sharpen a specific Craft Cloud decision, not to produce a generic market report.

## Scope the decision

1. State the user-facing capability or workflow under consideration.
2. Inspect the current Craft Cloud behavior or contract before assuming a gap.
3. Identify what the comparison could change: behavior, naming, defaults, UI, docs, limits, or acceptance criteria.
4. Skip an unsolicited competitive check when it cannot affect the decision. Always honor an explicit comparison request.

## Choose useful comparators

Select the closest two to four platforms by product and service layer; do not mechanically survey every one.

- Start with direct Craft CMS hosts such as Servd and fortrabbit.
- Use Laravel Cloud for framework-native PHP workflows.
- Use Heroku or another general PaaS when the feature is standard app-hosting behavior.
- Add a specialized platform only when it is a closer match for the decision.

Treat the list as a starting point, not a permanent market map. Compare equivalent managed services and plan tiers; do not equate a self-managed feature, paid add-on, or enterprise exception with an included platform capability.

## Research live evidence

Browse current primary sources whenever a claim depends on present capabilities, pricing, limits, or terminology:

1. Official documentation
2. Official product, pricing, changelog, launch, or status pages
3. Public API or CLI documentation
4. First-party repositories when documentation is insufficient

Use secondary sources only to discover leads, then verify them against primary sources. Link each claim to the source that supports it. Record the access date for volatile facts such as pricing and quotas. Separate confirmed facts from inference; never treat missing public documentation as proof that a feature does not exist.

Compare the dimensions that matter to the decision:

- user outcome and mental model
- defaults and onboarding
- dashboard, CLI, or API workflow
- environment and deployment model
- operational limits and failure behavior
- security and tenant boundaries
- observability and support affordances
- pricing or plan boundaries

For pricing and quotas, normalize the currency, billing unit, region, plan tier, included usage, and required add-ons before drawing a conclusion.

## Recommend for Craft Cloud

Translate the evidence into one of four outcomes:

- **Adopt** the established pattern.
- **Adapt** it to Craft CMS-native terminology or constraints.
- **Differ** intentionally and explain why.
- **Defer** because the evidence does not justify product complexity.

Do not copy a competitor merely because a feature exists. Preserve Craft CMS-native defaults, exact behavior contracts, secure server-side boundaries, and clear incident behavior.

## Report briefly

Default to:

```markdown
Decision:
...

Comparable behavior:
- Platform: confirmed behavior and relevant limit ([source](...)).
- Platform: confirmed behavior and relevant limit ([source](...)).

Craft Cloud implication:
Adopt/adapt/differ/defer because ...

Confidence or gaps:
... (only when material)
```

For implementation or review work, include the competitive check only when it changes the design, naming, documentation, or acceptance criteria. “No design change” and “no useful public comparison found” are valid conclusions.
