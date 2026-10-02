> ## Documentation Index
> Fetch the complete documentation index at: https://polar.sh/docs/llms.txt
> Use this file to discover all available pages before exploring further.

# polar event_types

```bash theme={null}
polar event_types <subcommand> [flags]
```

**Subcommands**

* [`polar event_types list`](#polar-event_types-list) List event types with aggregated statistics.
* [`polar event_types update`](#polar-event_types-update) Update an event type's label.

## polar event\_types list

List event types with aggregated statistics.

```bash theme={null}
polar event_types list [flags]
```

**Flags**

| Flag | Type | Description |
| - | - | - |
| `--data`, `-d` | `string` | JSON object; explicitly supplied flags override its top-level keys |
| `--organization-id`, `--org` | `string` | Filter by organization ID. Defaults to the active organization. |
| `--customer-id` | `string` | Filter by customer ID. |
| `--external-customer-id` | `string` | Filter by external customer ID. |
| `--query` | `string` | Query to filter event types by name or label. |
| `--root-events` | `boolean` | When true, only return event types with root events (parent\_id IS NULL). |
| `--parent-id` | `string` | Filter by specific parent event ID. |
| `--source` | `choice` | Filter by event source (system or user). (choices: system, user) |
| `--page` | `integer` | Page number, defaults to 1. |
| `--limit` | `integer` | Size of a page, defaults to 10. Maximum is 100. |
| `--sorting` | `choice` | Sorting criterion. Several criteria can be used simultaneously and will be applied in order. Add a minus sign `-` before the criteria name to sort by descending order. (choices: name, -name, label, -label, occurrences, -occurrences, first\_seen, -first\_seen, last\_seen, -last\_seen) |

## polar event\_types update

Update an event type's label.

```bash theme={null}
polar event_types update [flags] <id>
```

**Arguments**

| Argument | Type | Description |
| - | - | - |
| `id` | `string` | |

**Flags**

| Flag | Type | Description |
| - | - | - |
| `--data`, `-d` | `string` | JSON object; explicitly supplied flags override its top-level keys |
| `--label` | `string` | Required. The label for the event type. |
| `--label-property-selector` | `string` | Property path to extract dynamic label from event metadata (e.g., 'subject' or 'metadata.subject'). |


This documentation is built and hosted on [Mintlify](https://mintlify.com), a developer documentation platform.
