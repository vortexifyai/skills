---
name: vortexify-app-design-patterns
description: Design or refine Vortexify application layouts and interactions. Use when creating screens, adapting design examples, adding operational workflows, or reviewing filters, record details, scenario comparisons, approvals and contextual agents.
---

# Vortexify App Design Patterns

Apply patterns according to the user's workflow and the selected design.
Preserve established conventions during focused changes. Do not add every
pattern to every application.

Use vortexify-apps for SDK contracts, CLI operations, authentication,
implementation and builds. This skill defines the interaction behavior.

## Start with the workflow

Identify the user's question, the records they inspect, the decision they
make and the action that follows. Read the selected design and its companion
README when available. Adapt the example to actual project contracts.

Keep business-specific data requirements in the template README rather
than duplicating them in this skill.

## Scope and record context

Keep filters, selected records and relevant versions consistent across
charts, tables, details and agent context. Make the active scope visible.

Use record drawers when users need supporting details without losing their
place. Use dedicated pages for larger working tasks. Include relevant
relationships, source timestamps and activity history.

## Planning and analysis

Distinguish source facts, overrides and scenario assumptions. Preserve
the input snapshots and rule versions needed to explain saved results.

Compare alternatives on a consistent scope. Show meaningful tradeoffs,
constraints and missing inputs. Mark results stale when inputs change.

Distinguish observed outcomes from projections, and investigation findings
from confirmed causes.

## Review and actions

Use explicit draft, submitted, returned, approved and published states
where the workflow requires them. Preserve published versions and decisions.

Expose action prerequisites and validation feedback. Backend enforcement
must check permissions, quantities, current state and expected versions.

Preserve user input after failures. Distinguish submitted, processing,
failed and acknowledged actions. Make retries safe where duplicate
submission could repeat a business event.

## Contextual assistance

For apps with agent assistance, use a contextual, collapsible panel.
Tailor suggested questions, answers, evidence links and permitted actions
to the current page, filters and selected record.

Open supporting records without unexpectedly changing the user's scope.
Distinguish deterministic demo behavior from a connected agent, and
explanations from approved business actions.

## Interaction verification

Check loading, empty, stale-data, error and success states against the
actual workflow. Verify responsive layouts, keyboard navigation, focus
handling and readable tables and charts.

Verify that selection changes update dependent views, failed actions
retain drafts, and unpublished edits cannot appear as published results.