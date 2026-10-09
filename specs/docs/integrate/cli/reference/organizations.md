> ## Documentation Index
> Fetch the complete documentation index at: https://polar.sh/docs/llms.txt
> Use this file to discover all available pages before exploring further.

# polar organizations

```bash theme={null}
polar organizations <subcommand> [flags]
```

**Subcommands**

* [`polar organizations get`](#polar-organizations-get) Get an organization by ID.
* [`polar organizations list`](#polar-organizations-list) List organizations.

## polar organizations get

Get an organization by ID.

```bash theme={null}
polar organizations get [flags] <id>
```

**Arguments**

| Argument | Type | Description |
| - | - | - |
| `id` | `string` | |

**Flags**

| Flag | Type | Description |
| - | - | - |
| `--fields` | `string` | Only print these fields, comma-separated; use dots for nested ones, e.g. id,name,prices.price\_amount |

## polar organizations list

List organizations.

```bash theme={null}
polar organizations list [flags]
```

**Flags**

| Flag | Type | Description |
| - | - | - |
| `--fields` | `string` | Only print these fields, comma-separated; use dots for nested ones, e.g. id,name,prices.price\_amount |
| `--data`, `-d` | `string` | JSON object; explicitly supplied flags override its top-level keys |
| `--slug` | `string` | Filter by slug. |
| `--page` | `integer` | Page number, defaults to 1. |
| `--limit` | `integer` | Size of a page, defaults to 10. Maximum is 100. |
| `--sorting` | `choice` | Sorting criterion. Several criteria can be used simultaneously and will be applied in order. Add a minus sign `-` before the criteria name to sort by descending order. (choices: created\_at, -created\_at, slug, -slug, name, -name, next\_review\_threshold, -next\_review\_threshold, days\_in\_status, -days\_in\_status) |


This documentation is built and hosted on [Mintlify](https://mintlify.com), a developer documentation platform.
