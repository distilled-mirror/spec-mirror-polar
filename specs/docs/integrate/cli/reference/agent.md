> ## Documentation Index
> Fetch the complete documentation index at: https://polar.sh/docs/llms.txt
> Use this file to discover all available pages before exploring further.

# polar agent

> Set up coding agents to build with Polar

```bash theme={null}
polar agent <subcommand> [flags]
```

**Subcommands**

* [`polar agent setup`](#polar-agent-setup) Install the Polar plugin, MCP server and skills for your coding agents

## polar agent setup

Install the Polar plugin, MCP server and skills for your coding agents

```bash theme={null}
polar agent setup [flags]
```

**Flags**

| Flag | Type | Description |
| - | - | - |
| `--status` | `boolean` | Show what is set up without changing anything |
| `--confirm`, `-c` | `boolean` | Set up every detected agent without asking |
| `--force` | `boolean` | Reinstall even when an agent is already set up |
| `--client` | `choice` | Only set up this agent (choices: claude-code, codex, cursor) |
| `--skills` | `choice` | Also install the Polar skills into .agents/skills of this directory (project) or your home directory (global), for agents without a Polar plugin (choices: project, global) |

**Examples**

Detect Claude Code, Codex and Cursor, then choose what to set up

```bash theme={null}
polar agent setup
```

Show what is already set up

```bash theme={null}
polar agent setup --status
```

Only install the Polar plugin for Claude Code

```bash theme={null}
polar agent setup --client claude-code
```

Install the Polar skills into .agents/skills for any other agent

```bash theme={null}
polar agent setup --skills project
```

Set up every detected agent without asking, for scripts

```bash theme={null}
polar agent setup --confirm --json
```


This documentation is built and hosted on [Mintlify](https://mintlify.com), a developer documentation platform.
