# Core SDK contracts

Read only the section relevant to the current resource. Inspect the project's installed SDK version/source for additional interfaces rather than guessing a signature.

## Python connectors and syncs

A `Connector(config)` owns a plain configuration dictionary and lazily creates/reuses its source client in `client()`. Implement `check()` with a lightweight access read and return `True` or raise. `discover()` returns `DiscoveredTable(name=..., schema=[Column(name=..., type=...)])`. Column types are source-defined strings. `close()` releases owned clients, is repeat-safe, and does not initialize a client just to close it. The connector context manager calls cleanup.

`Sync(connector, config=None, state=None)` exposes `self.connector`, `self.client`, `self.config`, and `self.state`. Implement `extract(self, *, avg_row_size_bytes=None)` as an iterator yielding Pandas DataFrames. `preview(self)` returns a small DataFrame from an independent read with fresh configuration/client and temporary state; it must not call extraction or advance production checkpoints. `SyncPreview.from_dataframe(frame)` derives an optional sample-size estimate. Handle an absent estimate. Config files contain JSON Schemas; actual secrets are platform configuration.

Full is the default sync mode. Incremental output is appended and checkpoints are persisted by the platform after durable writes; choosing incremental mode does not implement change capture. Declare update/deletion and deduplication behavior deliberately. Sync schedules are cron-only. SDK table references use `Table("connector.sync")` while CLI sync selectors use `connector/sync`.

## Python transforms, APIs, and custom agents

Use `@vxfy_transform` for tabular work and `@vxfy_api` for request/response operations. Declare inputs in source with table/output/database/secret references or scalar `Input` values. Transforms can return one DataFrame or named DataFrames; consumers select named outputs with `TransformOutput("transform.output")`. API names used by the CLI/React SDK are registered resource names, not the decorator route. Inspect actual source declarations to establish data contracts.

`Agent` declares a custom app agent with a prompt, table/transform context, `ApiRunner` backend tools, and optional `AgentCapabilities`. Uploaded documentation is a platform attachment rather than a Python context declaration. Use `vortexify upload file PATH --resource-type agent --resource NAME --wait` to attach it; custom agents can search their own saved attachments. CLI `files` commands show bounded previews. The coding agent building the project is separate from these app agents.

## React

Wrap an app in `VortexifyProvider` with a stable `createVortexifyConfig(import.meta.env)` configuration. `useVortexify()` exposes the shared `client`, `ready`, and a conversation `agentManager`. Inspect the installed package's types for detailed options.

- `client.runApi(name, inputs?, options?)` invokes a registered API and returns its function result.
- `startTransform`, `getTransformJob`, and `pollTransform` submit/observe background transforms. Polling does not retrieve their table data or cancel server work on timeout.
- `client.runAgent(name, options)` creates a lazy single-use stream request; consume it with `agentManager.consume(...)`.
- Frontend actions execute in the browser and must return AgentToolResult ({success, message?, data?}; message required on failure). The SDK submits results/errors and the model waits for them. Action timeoutMs defaults to 60000 (1000–120000). Backend tools execute on the server. Use useAgentView({id, description, getContext}) inside the provider for initial data.current_view and on-demand get_current_view_context; callbacks read the latest active page.
- Chart tools return a saved query/chart description. Fetch rows using `client.getAnalysisData(queryId)` and render with the app's chart library.

Use `vortexify dev APP` and its printed URL for authenticated local preview. Hosted apps use the platform shell. Keep credentials out of browser source and public environment variables. Prepare the necessary committed API runtimes and table outputs before testing app behavior. Preview builds, deployment, and actual functional verification are distinct steps.
