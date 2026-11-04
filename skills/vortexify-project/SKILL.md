---
name: vortexify-project
description: Work on a Vortexify project through its CLI and shared project files, including resource discovery, planning, upstream/downstream impact, and local source commits.
---

# Vortexify Project Workflow

Read [reference locations](references/locations.md) for CLI, SDK, and template access. Use this workflow when working on a Vortexify project.

## Establish context

Find the nearest `vortexify.project.yaml`; its paths map resource types to local folders. Run `vortexify status` before planning commits to inspect registration, local/server drift, upstream requirements, and downstream impact. Use `vortexify registry` for registered server resources. `VORTEXIFY_ORG_ID` supplies the default organization; `--org-id` overrides it. Read relevant local code and saved plans before creating replacements.

Connectors access external systems; syncs publish datasets; transforms process datasets; APIs expose request/response operations; databases store transactional state; agents use context and tools; React apps consume platform resources. Prefer extending an existing resource when its responsibility fits.

## Shared project files

Use these collections as a persistent, evolving understanding of the project and
business. Read .vortexify/context/index.md and .vortexify/knowledge-base/index.md
first, then follow relevant links before planning or changing resources.

### What belongs where

Context describes what we are building, why, how it should work, and how we plan
to implement it:

- context/requirements/: Markdown project objectives, user needs, constraints and
  acceptance criteria. One project-requirements.md is often enough; split only
  when useful, rather than creating a requirements file for every business topic.
- context/applications/: Markdown application specifications describing purpose,
  users, pages, workflows, interactions and expected functionality; actual HTML
  designs showing layouts, components, charts and interactions when useful.
- context/architecture/: Markdown implementation plans and decisions explaining
  pipelines, calculations, APIs, responsibilities and how resources work together.
  One plan can cover several resources.

Knowledge Base describes the underlying business:

- knowledge-base/concepts/: Markdown business concepts, definitions, processes,
  calculations, policies and rules, such as available inventory or waste classification.
- knowledge-base/entities/: Markdown business entities, such as production orders,
  machines or doctors: what they represent and how they relate. An entity may span
  multiple datasets; it is not a document per table.

These paths are inside .vortexify/. Use .md for documents and .html for designs;
HTML designs belong under context/applications/. Prefer app-name-prefixed names
such as inventory-planner-overview.html. Multiple designs per app are supported.
Designs are useful visual implementation references, not just written descriptions.
Attached blueprint examples are separate references, read through vortexify blueprints.

### When to maintain the collections

- Update Context as users explain requirements, applications are planned or
  designed, resources are implemented, and requirements or decisions change.
  Maintain useful specifications, HTML designs and architecture alongside the work,
  rather than waiting for a separate documentation step at the end.
- Update Knowledge Base when users provide business explanations, answer business
  questions, or data exploration reveals meaningful business understanding.
  Preserve that learning even when the user did not explicitly ask to document it.
  A code change alone does not require a Knowledge Base update.
- Use primary-key and relationship findings to understand business entities and
  their connections. Synthesize useful meaning; do not copy graph reports, schema
  dumps or connector table catalogs. Inspect source, CLI tables/previews and the
  knowledge graph directly for current technical information.
- Distinguish confirmed facts from observations and unresolved interpretations.
  Preserve useful open questions and revisit them when relevant without interrupting
  for every uncertainty. Update existing knowledge when clarified; preserve business
  explanations when discovery findings change. Neither collection is a transcript
  or activity log.

### Organization and maintenance

Both root index.md files and all five category folders are required; folders may
be empty. Keep indexes concise, with overviews and links to useful documents.
Create files and nested folders only when they help, for example
knowledge-base/entities/manufacturing/production-order.md or
context/applications/designs/inventory-planner-overview.html. Additional indexes
are optional. No prescribed sections, document templates or document per resource
are required.

Prefer updating existing documents. Split large or distinct topics, merge overlap,
and remove obsolete plans and superseded designs while preserving unrelated knowledge.
Keep the smallest useful collection of accurate knowledge and development artifacts.
Maintain ordinary relative Markdown links when files are added, moved or deleted.
Consult vortexify.project.yaml for resource paths and resolve links from the actual
document location, including .vortexify/, rather than from the collection root.
Links do not automatically synchronize their targets.

Plans express intent; committed code and registered resources establish implemented
behavior. Reconcile differences and distinguish planned from completed work. Do not
start unrelated planned work merely because a document mentions it.

Use vortexify context status, pull and push for the collections; full vortexify
status, pull and commit also include them. Collections are versioned separately
from resource source and runtime-agent attachments. Reconcile concurrent changes
before saving again. Inspect command help for the installed CLI version.

For example, if a user explains that reserved inventory cannot be promised to a
new order, preserve that definition in concepts/available-inventory.md. Describe
the requested replenishment screen in applications/inventory-planner.md, create
an HTML design when useful, and explain the allocation pipeline in architecture/.
Link these documents rather than repeating the business rule in each one.

If discovered order-to-roll connections have unclear meaning, note the unresolved
interpretation in the relevant entity document. Ask whether they mean planned
usage or actual consumption when that distinction matters to the current work;
incorporate the answer without replacing existing business explanations.

`vortexify init` creates these files. For an existing project, use `vortexify context init`. Full `pull`, `commit`, and `status` include both collections; resource-filtered operations affect source only. `context pull`, `context push`, and `context status` operate on collections independently. Concurrent edits are rejected. If both local and server files changed, use `vortexify context pull --backup-local` to archive the conflicting local collection in `.vortexify/backups/*.tar.gz` and pull the server version. Extract the archive elsewhere, reconcile your edits into the pulled files, then run `vortexify context push`. Ordinary pull preserves conflicting local files and refuses to overwrite them. `context versions <collection>` lists snapshots; `context read <collection> <path>` reads server content.

Use `vortexify tables`, `vortexify table <name-or-id>`, `registry`, and the local source to inspect resources directly. There is no CLI manager conversation. For read-only SQL on published outputs:

```sh
vortexify analyze --query "SELECT * FROM orders LIMIT 20" --table orders=mes.orders --limit 20 --wait
vortexify analyze --job <job-id>
vortexify logs <job-id>
```

SQL references the supplied aliases, such as `SELECT * FROM orders`. Multiple `--table` arguments support joins. Analysis returns a tracked job; no resource commit is required. Use `--deployed` for deployed output inventories.

Use CLI-accessible examples rather than requiring a templates/blueprints repository checkout. Currently, `vortexify blueprints` lists attached project blueprints, `vortexify blueprints search "query"` searches the full catalog, and `vortexify blueprints read <name> [path]` reads a selected attachment. Catalog visibility and attachment are different; attach only within the requested scope. Follow the returned inventory for supporting files.

Use `blueprints list` only for attachments; search the full catalog with `blueprints search` and attach a selected name with `blueprints attach`. The CLI does not list the whole catalog. Inspect `vortexify --help` and the relevant command help for the installed version. Templates are examples to adapt, not guaranteed implementations of every requested dataset or UI.

## Plan and implement

Review upstream inputs and affected downstream resources, including server-only resources. For a new workflow, agree on the resource flow before registration; existing approval covers the agreed scope. Keep relevant requirements, intended interfaces, decisions, assumptions and acceptance criteria in Context as the work evolves. Inspect source and platform metadata for current schemas instead of copying them into plans. Capture business learning in Knowledge Base when appropriate.

Implement against current SDK references. Source-declared inputs are authoritative and are validated at commit; manual graph edits cannot override conflicting declarations. Transforms can return one table or a dictionary of named tables. Keep consumer references consistent with the selected output names. Use `Table("_upload.customers")` for a published uploaded table named `customers`; `_upload` is reserved and cannot name a connector. Use `Table("connector.sync")` for synced data. Do not create a mock connector just to consume an upload. Do not infer output table availability from resource registration alone.

## Register, commit, and verify

```sh
vortexify create transform daily_revenue
vortexify status --type transform --resource daily_revenue
vortexify commit --type transform --resource daily_revenue
```

Use `create <type> <name>` to register a resource and obtain its starter; it preserves an existing local implementation. Create a sync with its qualified name, such as `vortexify create sync shop/orders`; `--connector` is also supported for an unqualified sync name. Commit selected resources upstream-first; `vortexify commit` orders changed resources in its selection and validates their references. A broad commit selects all managed resources, so choose the scope intentionally. Committing sync/database input references requires committed producers, not materialized output: consumers can commit while upstream runs or migrations are pending, running, or failed. TransformOutput also requires committed producer source. Sync-based trigger schedules can commit before the sync publishes a table. Execution still requires the referenced published data or database schema. Uploaded-table references require an existing uploaded table because there is no separately committed producer resource.

If registration succeeds but starter download or verification fails, use `vortexify create <type> <name> --resume` to recover its source without registering another resource. Resume requires an existing registration and a missing local resource folder; it preserves existing local implementations.

Database commits report required migration approval. Approval and successful migration are separate: inspect the job result, and after a failed migration, `vortexify database NAME --approve VERSION` submits a new job.

Downstream impact is informational, not permission to edit or deploy those resources. Pull only what is needed and inspect conflicts rather than overwrite local changes. Registration, committed source, deployed version, and completed execution are distinct states. Validate relevant behavior and report which states actually changed; a successful commit is not a deployment or a successful run.

## Preview execution and deployment

Use native CLI commands for the full loop:

```sh
vortexify test transform daily_revenue --wait
vortexify run transform daily_revenue --rebuild-downstream --wait
vortexify build api revenue_summary --wait
vortexify build app dashboard --wait
vortexify job <job-id> --json
vortexify logs <job-id>
vortexify deploy app dashboard --version 3 --wait
vortexify upgrade app dashboard --wait
```

`run` accepts API, transform, or sync; API/transform tests use `test`. Supply `--inputs-file` for runtime parameters and `--version` for a specific committed preview version. `build transform` only prepares dependencies, whereas `run transform` produces tables. Preview app builds compile and publish a preview URL. Deployed app publication remains a separate action. Use `--json` for machine-readable results; `--wait` polls existing work and never resubmits it. Inspect results, validation findings, output inventories, and logs; stopping a wait does not cancel execution.

Preview schedules and production triggers do not apply. Explicitly run/build each required resource. New commits make preview outputs and downstream previews stale, while last successful outputs remain usable. `status` reports freshness against committed source and consumed execution snapshots; local edits are separate. `--rebuild-downstream` runs a persisted plan upstream-first and waits for schema publication before consumers. Downstream APIs prepare runtimes; they are not invoked automatically. Downstream runtime parameters must have defaults.

Deployed resources are pinned and do not become stale on commit. Newer versions are available to upgrade. `deploy --version` selects a version; `upgrade` pins latest committed at request time. Rebuilding previews does not deploy or upgrade anything. Tables keep stable IDs across versions. Project inspection reads the latest successful output; deployed consumers read the latest successful output of the producer’s deployed version. Sync inventories and checkpoints are separated by producer version. All successful publications trigger PK/relationship discovery. Discovery findings identify their exact input snapshots; manual runs never fire scheduled downstream triggers. Perform deployment only within the user's authorized scope. Never label a deployment stale merely because a newer commit exists.

For connector setup, commit its `config.json` schema, then use
`vortexify connector <name> --configure` to supply actual server values interactively.
Enter keeps/defaults/skips; `:clear` removes optional values and `:cancel` abandons edits.
Secret prompts are hidden and require a terminal. Have the user enter credentials
there; do not put secret values in CLI arguments, source files or chat. Saving
configuration creates a connector version without deploying it or running syncs.

Syncs belong in `<connectors-path>/<connector>/syncs/<sync>/`; use `connector/sync`
for CLI selection and `sync:connector/sync` in graph/state identities. Short sync
names are accepted only when unambiguous. New YAML has no separate syncs path.
Connector code and sync code commit independently; nested syncs do not affect the
connector's source hash. Keep SDK `Table("connector.sync")` references unchanged.
