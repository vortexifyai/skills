# CLI, SDK, and template references

## CLI

Start with `vortexify --help`, then the relevant subcommand's `--help`. The installed CLI is authoritative for command availability. Install/authenticate it separately from this skills plugin; a plugin installation does not log into Vortexify.

## Project templates and examples

Search and read blueprints through the CLI. Use `vortexify blueprints search "query"` to search the full backend catalog, and `vortexify blueprints attach NAME` to attach a specific result. Do not require a checkout of an internal blueprints repository.

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

Read [SDK contracts](sdk.md) for the core interfaces. Use the project's SDK version, installed Python source/TypeScript declarations, and available SDK documentation for exact signatures beyond this reference. The full package references are the [Python SDK](https://github.com/vortexifyai/python-sdk#readme) and [React SDK](https://github.com/vortexifyai/react-sdk#readme) READMEs.

The plugin contains guidance, not vendored source implementations, credentials, a Vortexify session, or an MCP server. Coding agents use the CLI/local project; Vortexify `Agent` resources are the custom agents built for apps.
