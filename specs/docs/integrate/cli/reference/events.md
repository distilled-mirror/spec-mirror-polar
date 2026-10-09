> ## Documentation Index
> Fetch the complete documentation index at: https://polar.sh/docs/llms.txt
> Use this file to discover all available pages before exploring further.

# polar events

```bash theme={null}
polar events <subcommand> [flags]
```

**Subcommands**

* [`polar events get`](#polar-events-get) Get an event by ID.
* [`polar events list_names`](#polar-events-list_names) List event names.

## polar events get

Get an event by ID.

```bash theme={null}
polar events get [flags] <id>
```

**Arguments**

| Argument | Type | Description |
| - | - | - |
| `id` | `string` | |

**Flags**

| Flag | Type | Description |
| - | - | - |
| `--fields` | `string` | Only print these fields, comma-separated; use dots for nested ones, e.g. id,name,prices.price\_amount |

## polar events list\_names

List event names.

```bash theme={null}
polar events list_names [flags]
```

**Flags**

| Flag | Type | Description |
| - | - | - |
| `--fields` | `string` | Only print these fields, comma-separated; use dots for nested ones, e.g. id,name,prices.price\_amount |
| `--data`, `-d` | `string` | JSON object; explicitly supplied flags override its top-level keys |
| `--organization-id`, `--org` | `string` | Filter by organization ID. Defaults to the active organization. |
| `--customer-id` | `string` | Filter by customer ID. |
| `--external-customer-id` | `string` | Filter by external customer ID. |
| `--source` | `choice` | Filter by event source. (choices: system, user) |
| `--query` | `string` | Query to filter event names. |
| `--page` | `integer` | Page number, defaults to 1. |
| `--limit` | `integer` | Size of a page, defaults to 10. Maximum is 100. |
| `--sorting` | `choice` | Sorting criterion. Several criteria can be used simultaneously and will be applied in order. Add a minus sign `-` before the criteria name to sort by descending order. (choices: name, -name, occurrences, -occurrences, first\_seen, -first\_seen, last\_seen, -last\_seen) |


This documentation is built and hosted on [Mintlify](https://mintlify.com), a developer documentation platform.
