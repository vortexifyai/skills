# CLI, SDK, and template references

## CLI

Start with `vortexify --help`, then the relevant subcommand's `--help`. The installed CLI is authoritative for command availability. Install/authenticate it separately from this skills plugin; a plugin installation does not log into Vortexify.

## Application blueprints

A blueprint brings together an HTML application design, implementation guides, and information about the use case and its workflows. Use it when planning and building a particular app in your project. Search with `vortexify blueprints search "query"`, inspect the file inventory, and read the README, relevant implementation guides, use-case documentation, and HTML through `vortexify blueprints read NAME [PATH]`. Attach selected references with `vortexify blueprints attach NAME`. Attachments belong to the project; identify the intended app and relevant sections in `--usage-note` and that app’s specification under `.vortexify/context/applications/`.

Blueprint commands include:

```sh
vortexify blueprints list
vortexify blueprints search "operations dashboard"
vortexify blueprints attach NAME
vortexify blueprints read NAME
vortexify blueprints read NAME PATH
```

Listing shows only project attachments; the entire catalog is searchable, not listable. Follow returned file inventories and respect attachment/read scope. If a required template is not accessible, use existing project code plus the SDK and source documentation, or identify the missing reference when it blocks correct implementation.

## SDK contracts

Read [SDK contracts](sdk.md) for the core interfaces. Use the project's SDK version, installed Python source/TypeScript declarations, and available SDK documentation for exact signatures beyond this reference. Read the public [Python reference](https://vortexify.ai/docs/reference/python), [React setup](https://vortexify.ai/docs/reference/react/setup), and [CLI reference](https://vortexify.ai/docs/reference/cli/overview) for detailed contracts. For source integrations, start with the [connector guides](https://vortexify.ai/docs/connectors). Append `.md` to a documentation page URL for Markdown; https://vortexify.ai/llms.txt provides the documentation index. Read these pages directly without installing an SDK or checking out its repository.

The plugin contains guidance, not vendored source implementations, credentials, a Vortexify session, or an MCP server. Coding agents use the CLI/local project; Vortexify `Agent` resources are the custom agents built for apps.
