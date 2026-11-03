# Vortexify agent skills

Give your coding agent guidance for building Vortexify projects, data connectors, syncs, and React applications.

The Vortexify skills plugin works with Codex and Claude Code. It helps your coding agent use the Vortexify CLI, follow SDK conventions, and work with your project's code and context.

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
