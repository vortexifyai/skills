# Links in project documents

Use relative Markdown links between documents. For example, from
`.vortexify/context/architecture/pipeline.md`, link to business knowledge with
`[Order](../../knowledge-base/entities/order.md)`.

For registered resources, copy the exact `resource_reference` from
`vortexify registry --json` or `vortexify tables --json`. These links are relative
to the current project identity, independent of local folders or YAML mappings.
The viewer opens the resource or table, not a local source file. No execution,
deployment, data publication, or version pinning is implied.

| Target | Example |
| --- | --- |
| API | `vortexify://api/orders` |
| Transform | `vortexify://transform/forecast` |
| App | `vortexify://app/planner` |
| Agent | `vortexify://agent/assistant` |
| Connector | `vortexify://connector/mes` |
| Sync | `vortexify://sync/mes/plants` |
| Database | `vortexify://database/planning` |
| Sync table | `vortexify://table/sync/mes/plants` |
| Named sync output | `vortexify://table/sync/mes/plants/locations` |
| Transform table | `vortexify://table/transform/forecast` |
| Named transform output | `vortexify://table/transform/forecast/monthly` |
| Uploaded table | `vortexify://table/upload/customers` |
| Database table | `vortexify://table/database/planning/origin/public.orders` |

Write `[Plants](vortexify://table/sync/mes/plants)`, not a local connector path.
Producer types distinguish outputs even when a connector and transform share a
name. The default sync/transform output has no `/default` suffix. Each name is a
percent-encoded path segment; copy the supplied URI instead of constructing it.
A transform resource and its output table are distinct targets.

If a tool does not return a reference, leave the resource name as prose and report
the missing reference. Do not guess links for planned or unregistered resources.
Renames/deletions may break name-based links; update affected documents. Missing
or ambiguous targets are errors. Older producer-qualified table links are accepted
only when they identify exactly one active project table. Existing local source
links are legacy and depend on the last saved workspace path mapping; replace
them with canonical links when the intended resource is known.
