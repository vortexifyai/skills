# Core SDK contracts

Read only the section relevant to the current resource. Inspect the project's installed SDK version/source for additional interfaces rather than guessing a signature.

Use `vortexify==0.1.1` in Python `requirements.in`, matching the SDK installed in platform base images. Add only the resource's additional third-party dependencies. The current React app starter targets `@vortexify/react@0.1.2`; update existing apps and their lockfiles to that release for HTTP error messages.

## Python connectors and syncs

A `Connector(config)` owns a plain configuration dictionary and lazily creates/reuses its source client in `client()`. Implement `check()` with a lightweight access read and return `True` or raise. `discover()` returns `DiscoveredTable(name=..., schema=[Column(name=..., type=...)])`. Column types are source-defined strings. `close()` releases owned clients, is repeat-safe, and does not initialize a client just to close it. The connector context manager calls cleanup.

`Sync(connector, config=None, state=None)` exposes `self.connector`, `self.client`, `self.config`, and `self.state`. Implement `extract(self, *, avg_row_size_bytes=None)` as an iterator yielding Pandas DataFrames. `preview(self)` returns a small DataFrame from an independent read with fresh configuration/client and temporary state; it must not call extraction or advance production checkpoints. `SyncPreview.from_dataframe(frame)` derives an optional sample-size estimate. Handle an absent estimate. Config files contain JSON Schemas; actual secrets are platform configuration.

Full is the default sync mode. Incremental output is appended and checkpoints are persisted by the platform after durable writes; choosing incremental mode does not implement change capture. Declare update/deletion and deduplication behavior deliberately. Sync schedules are cron-only. SDK table references use `Table("connector.sync")` while CLI sync selectors use `connector/sync`.

## Python transforms, APIs, and custom agents

Use `@vxfy_transform` for tabular work and `@vxfy_api` for request/response operations. Declare inputs in source with table/output/database/secret references or scalar `Input` values. Transforms can return one DataFrame or named DataFrames; consumers select named outputs with `TransformOutput("transform.output")`. API names used by the CLI/React SDK are registered resource names, not the decorator route. Inspect actual source declarations to establish data contracts.

`Agent` declares a custom app agent with a prompt, table/transform context, `ApiRunner` backend tools, and optional `AgentCapabilities`. Uploaded documentation is a platform attachment rather than a Python context declaration. Use `vortexify upload file PATH --resource-type agent --resource NAME --wait` to attach it; custom agents can search their own saved attachments. CLI `files` commands show bounded previews. The coding agent building the project is separate from these app agents.

`Table("_upload.customers")` references an uploaded table named `customers` in function inputs or agent context. `_upload` is reserved and cannot be a connector name. These use the same DataFrame/DuckDB input behavior as synced tables. Source commits require committed sync/database/transform producers, not completed executions; data must be published before a consumer runs. Sync trigger schedules can also commit before materialization. Uploaded references need an existing table.

Use `vortexify test api NAME --wait` or `vortexify test transform NAME --wait` after committing changes. These are server-side SDK mock tests: the platform runs the selected immutable committed source, including its tests and mock factories, with its selected dependency image and records the job/results against that resource version. This keeps platform verification tied to saved code and its runtime rather than uncommitted edits or a developer's Python environment. `--version N` selects a specific committed version. Local `vortexify.testing.run_tests(...)` is useful for quick feedback, but does not submit a platform test job or establish that the committed version passes on the server.

Supply explicit mocks for resource inputs; missing required mocks are errors, never a fallback to live tables, secrets or preview databases. The runner checks input names, required values, scalar types and supported mock objects before invoking the resource. It does not validate table columns or business correctness automatically: tests must assert those contracts. Mock factories provide repeatable scenarios without depending on changing live data. No CSV fixtures or preview database seeds are generated automatically. Use `vortexify run` separately for integration checks against real preview inputs; a passing mock test does not prove database permissions, data availability or deployed behavior.

New resources start as uncommitted starter baselines; commit before building or executing them. The first explicit commit creates version 1, even when the starter is unchanged. For `PROJECT_GUARDIAN_NOT_READY`, use `vortexify guardian --wait`; inspect failed activation before an editor retries with `--retry --wait`.

Automatic `@vxfy_validate` output validation applies to transforms only. Neither preview nor deployed APIs invoke it automatically; put required checks in the API body or explicit tests. API validator declarations produce a non-blocking commit warning.

`Database` inputs resolve to scoped psycopg2 connections. Use cursors with `%s` value placeholders and separate parameters. Autocommit is off; the runtime commits read/write connections after successful function/output processing and closes them on exit. Roll back after a caught SQL error before reusing the connection. Manual commits cannot be undone by later runtime failures. Preview uses preview databases without falling back to origin; mocks own their supplied connections.

## React

Wrap an app in `VortexifyProvider` with a stable `createVortexifyConfig(import.meta.env)` configuration. `useVortexify()` exposes the shared `client`, `ready`, and a conversation `agentManager`. Inspect the installed package's types for detailed options.

- `client.runApi(name, inputs?, options?)` invokes a registered API and returns its function result. HTTP failures preserve the server message, HTTP status, and decoded response when available. A message that the API runtime is not ready means prepare it with `vortexify build api NAME --wait`; a committed API alone is not ready to execute.
- `startTransform`, `getTransformJob`, and `pollTransform` submit/observe background transforms. Polling does not retrieve their table data or cancel server work on timeout.
- `client.runAgent(name, options)` creates a lazy single-use stream request; consume it with `agentManager.consume(...)`.
- Frontend actions execute in the browser and must return AgentToolResult ({success, message?, data?}; message required on failure). The SDK submits results/errors and the model waits for them. Action timeoutMs defaults to 60000 (1000–120000). Backend tools execute on the server. Use useAgentView({id, description, getContext}) inside the provider for initial data.current_view and on-demand get_current_view_context; callbacks read the latest active page.
- Chart tools return a saved query/chart description. Fetch rows using `client.getAnalysisData(queryId)` and render with the app's chart library.

Use `vortexify dev APP` and its printed URL for authenticated local preview. Hosted apps use the platform shell. Keep credentials out of browser source and public environment variables. Prepare the necessary committed API runtimes and table outputs before testing app behavior. Preview builds, deployment, and actual functional verification are distinct steps.

Python SDK 0.1.1 removes `Agent.name`. Declare `Agent(prompt=..., context=..., tools=...)`;
use the registered resource name or ID in CLI/React calls. When upgrading existing
source, remove the `name=` argument and update `requirements.in` to `vortexify==0.1.1`.
