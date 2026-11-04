---
name: vortexify-apps
description: Create or edit Vortexify React applications using the React SDK, CLI-accessible design examples, backend resources, and frontend agent actions.
---

# Vortexify Applications

Use [the project workflow](../vortexify-project/SKILL.md) for shared project context and source commits. Resolve React/Python SDK contracts and CLI template access through [reference locations](../vortexify-project/references/locations.md).

Inspect existing app code, `package.json`, saved plans/designs, and the APIs, transforms, and agents it uses. Use `vortexify registry`, `vortexify table`, and existing API/resource source to inspect current schemas and backend contracts. Here, agents are the custom agents users build; coding agents work through the CLI and local files. Define user flows, backend calls, empty/error/loading states, and acceptance criteria before substantial implementation.

Read an attached design example with `vortexify blueprints` and `vortexify blueprints read <name> [path]`. Search the full catalog with `vortexify blueprints search "query"` and attach a selected blueprint with `vortexify blueprints attach <blueprint-name>`. Listing shows only project attachments. Follow the selected file inventory and read relevant HTML/assets through the CLI rather than assuming a local blueprints checkout. Treat it as a visual reference and adapt it to real data contracts. Saved design HTML may differ from implemented code; inspect both. Keep design changes distinct from functional implementation and honor any agreed design review.

Use the current React SDK's `VortexifyProvider` and configuration helpers. Apps call API functions, can start transforms and poll jobs, and retrieve resulting data through APIs. Integrate agent conversations and frontend action tools using the documented SDK registration and result contracts. Frontend action handlers run in the app and must return AgentToolResult ({success, message?, data?}, with message required on failure). The SDK submits results/errors to the waiting agent. Set timeoutMs per action when needed (default 60000, integer 1000–120000). Use useAgentView({id, description, getContext}) for one active route: data.current_view is sent initially and get_current_view_context reads the latest page during a run. Server API tools run in the backend. Neither grants additional backend access.

For agent-generated charts, consume the `generate_chart` tool result's `chart` object (`query_id`, `name`, `config`, `row_count`). Fetch its stored JSON rows with `client.getAnalysisData(chart.query_id)` and render using the app's chart library. Chart generation references a completed analysis; it does not execute SQL again or render the chart. Treat these backend tool results separately from frontend actions.

Keep secrets and long-lived credentials out of source and Vite environment variables. Local authentication comes from the CLI proxy; published apps receive runtime authentication from the app shell. The shared Clerk login and iframe wrapper belong to `vortexify-runtime/app-shell`, rather than each app's implementation.

Edit `package.json` to declare dependencies. The current starter targets `@vortexify/react@0.1.1`; update existing apps with `npm install --save-exact @vortexify/react@0.1.1` and commit the lockfile. Display the SDK error message: server failures such as an unprepared API runtime retain their explanation and HTTP status. For local preview, install dependencies in the app and run:

```sh
vortexify dev dashboard
```

Use the URL printed by the Vortexify CLI so SDK requests pass through its authenticated proxy. For platform builds, inspect returned job status, build logs and the preview_url. Run relevant local checks, then commit the intended app scope. Report build success separately from publication and functional verification.

Before local preview, use `vortexify build api <name> --wait` to prepare each API and `vortexify run transform <name> --wait` (or `run sync`) to publish required project tables. `vortexify build app <name> --wait` checks the committed app artifact; inspect logs using its job ID. Preview schedules do not run. Stale preview outputs remain usable until rebuilt; deployed apps keep their selected code versions and consume outputs of deployed producer versions. Tables retain one ID with separate output snapshots per job; checkpoints are isolated by producer version. Every successful table publication runs PK/relationship discovery; manual runs do not fire scheduled triggers. Use explicit `deploy --version` or `upgrade` when publication is authorized.

## Application specifications and designs

Follow the project workflow for the full Context/Knowledge Base conventions. Read
both collection indexes and relevant existing documents before planning changes.
As an application is planned, maintain a useful Markdown specification under
`.vortexify/context/applications/`: purpose, intended users, pages, navigation,
workflows, interactions, functionality and important visualizations as appropriate.
These are useful topics, not mandatory sections or a requirement to create a file
for every change.

Create actual HTML design references when useful for exploring or communicating
pages: rich layouts, UI components, charts and interactions, rather than prose alone.
Use self-contained HTML with inline CSS for portable design references. Store them
alongside app documents or in nested folders under `context/applications/`, with
recommended names such as `inventory-planner-overview.html`. Several designs per
app are supported. Link them from the relevant specification and keep both aligned
as design decisions and implementation evolve. Remove superseded artifacts.

Read attached blueprint examples with `vortexify blueprints` and
`vortexify blueprints read <name> [path]`. They are references separate from authored
project designs; do not assume every catalog example is attached or automatically
copy its source into the workspace.

If designing the workflow reveals a business rule or the user clarifies a concept
or entity, update the appropriate Knowledge Base document and reference it from
the specification. Routine component changes alone do not warrant a business
knowledge entry. Preserve unanswered business questions until they are relevant.
