# Vortexify agent skills

Give your coding agent guidance for building Vortexify projects, data connectors, syncs, and React applications.

The Vortexify skills plugin works with Codex and Claude Code. It helps your coding agent use the Vortexify CLI, follow SDK conventions, and work with your project's code and context.

## Changelog

### v0.1.4

- Clarify validator inputs for transforms with named outputs.
- Use Python SDK 0.1.2 in resource requirements.

### v0.1.3

- Clarify React SDK dependency setup and API error handling.
- Explain saved-version mock tests, explicit mock inputs and preview integration checks.
- Document guardian readiness, first commits, database transaction ownership and automatic output validation for transforms only.

### v0.1.2

- Use relative Markdown links between project documents and canonical resource links for registered tables, transforms, APIs, apps, agents, connectors, syncs and databases.
- Copy `resource_reference` from CLI registry/table output instead of constructing links from local workspace paths, UUIDs or output names such as `default`.
- Add reference examples for default and named table outputs, and explain missing targets, ambiguous links and resource renames.
- Align both plugin manifests with the release version. The v0.1.1 Git tag shipped with manifests still declaring 0.1.0.

### v0.1.1

- Document uploaded-table references, commits with pending producers, and collection conflict-recovery flows.
- Update Python and React SDK pins to 0.1.1 and remove `Agent.name` guidance.

## Install

With the Vortexify CLI and your coding tool installed, run the command for your tool:

```sh
vortexify skills install --agent codex
```

Or, for Claude Code:

```sh
vortexify skills install --agent claude
```

The coding tool's plugin manager owns installation, updates, and removal. Sign in with `vortexify login` before asking your agent to work with a Vortexify project.

The default source is [vortexifyai/skills](https://github.com/vortexifyai/skills). To select a different Git repository, add `--source GIT_URL` to either command.

## Use the skills

Open your Vortexify project in your coding tool and describe what you want to build. For example:

- "Create a transform that summarizes weekly revenue by customer."
- "Add a connector and an incremental sync for our orders API."
- "Build an orders dashboard using our project APIs."
- "Improve the dashboard's filters and record details."

The skills guide the agent through resource inspection, implementation, testing, and CLI workflows. Your project permissions and the coding tool's normal approval controls still apply.

## Included skills

| Skill | What it helps with |
| --- | --- |
| `vortexify-project` | Project context, resource inspection, dependencies, and source synchronization |
| `vortexify-connectors` | Connector configuration, source discovery, extraction, and syncs |
| `vortexify-apps` | React applications, project APIs, and agent conversations |
| `vortexify-app-design-patterns` | Page layouts, filters, record details, and contextual agent interactions |

## Install directly with your coding tool

You can also install the plugin directly from its GitHub repository through your coding tool:

For Codex:

```sh
codex plugin marketplace add https://github.com/vortexifyai/skills.git
codex plugin add vortexify@vortexify-tools
```

For Claude Code:

```sh
claude plugin marketplace add https://github.com/vortexifyai/skills.git
claude plugin install vortexify@vortexify-tools
```

The Vortexify CLI is still needed for project commands. The React and Python SDKs are installed separately in the applications and resource environments that use them.
