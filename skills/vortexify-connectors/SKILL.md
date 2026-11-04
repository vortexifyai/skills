---
name: vortexify-connectors
description: Create or edit Vortexify connectors and their syncs using CLI-accessible examples, configuration schemas, and the Python SDK.
---

# Vortexify Connectors and Syncs

Use [the project workflow](../vortexify-project/SKILL.md) for shared project context and source commits. Resolve SDK contracts and CLI template access through [reference locations](../vortexify-project/references/locations.md).

Inspect the connector, its existing syncs, configuration schemas, and relevant saved plans. Read the context/knowledge-base indexes and use `vortexify table` for schemas when necessary. Keep inspection focused on the connector and its consumers rather than listing the entire project by default.

Inspect source tables, schemas and previews through the CLI; do not create a
connector table catalog or schema dump in Context or Knowledge Base. Maintain a
useful ingestion/pipeline plan in `context/architecture/` when the work needs one;
it can cover multiple connectors and syncs. As source exploration or user answers
clarify business concepts and entities, enrich the relevant Knowledge Base files.
For example, explain how an order is represented across systems rather than
copying a relationship report. Preserve uncertain interpretations for clarification
when relevant and retain existing user-provided business explanations. Follow the
project workflow for organization, relative links and continuous maintenance.

Use `vortexify --help` to discover the installed CLI's template capabilities. When available, search/list source templates through the CLI, inspect the selected description and file inventory, and read only the relevant example files. Template search is planned and is not present in the current CLI; do not invent its command names or assume every example is attached to this project. Until it is available, use existing project source, accessible source documentation, and the SDK contract reference to implement the requested sync without requiring a blueprints repository checkout.

Examples are starting points for adaptation, including similar sources, rather than complete vendor integrations. Confirm authentication, endpoints, fields, pagination, filters, and limits from the selected source documentation. Choose table row meanings and supported resources for the requested use case. Use a custom API pattern when no source-specific example fits. Identify the required `requirements.in` packages and any native-driver setup.

Keep access/authentication in the connector and dataset-specific extraction in syncs. Implement the current SDK's `client()`, `check()`, and `discover()` contracts. `config.json` is a JSON Schema, not a file containing actual credentials. The platform resolves configuration and secrets into `self.config`; checkpoint state belongs in `self.state`.

Syncs yield Pandas DataFrames. Implement preview independently from extraction using the current SDK contract; preview must not consume a production checkpoint. Incremental output is appended by the runtime, so design cursors deliberately and use downstream transforms for deduplication when required. The platform persists checkpoints after durable data writes. Sync schedules are cron-only.

Pin `vortexify==0.1.1` in `requirements.in`. `_upload` is reserved and cannot name a connector. Uploaded tables can be consumed directly through `Table("_upload.table_name")`; do not create a connector for already uploaded data.

Register a connector before creating its syncs:

```sh
vortexify create connector shop
vortexify create sync shop/orders
```

Store each sync in `<connectors-path>/<connector>/syncs/<sync>/`, independently from
connector source. Sync names may repeat across connectors. Use qualified CLI names
such as `shop/orders` (or resource IDs); SDK table references remain `Table("shop.orders")`.
Project YAML has no standalone syncs path. Connector commits exclude nested syncs,
and connector pulls preserve them. Commit each changed sync explicitly.

Validate configuration handling and extraction logic with sample data or mocked source responses. Report separately whether live connectivity, actual ingestion, and output publication were verified. A template's sample resource does not establish complete vendor coverage.

After configuration, use `vortexify connector NAME --check --wait`, then
`vortexify connector NAME --discover --wait`. These execute saved connector code
and configuration without deploying or extracting data. Discovery reports source
table names and columns. Use `--version N` to select a saved version, and `--json`
for structured results. Checks/discovery and configuration are separate actions.
