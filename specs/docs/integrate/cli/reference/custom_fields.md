> ## Documentation Index
> Fetch the complete documentation index at: https://polar.sh/docs/llms.txt
> Use this file to discover all available pages before exploring further.

# polar custom_fields

```bash theme={null}
polar custom_fields <subcommand> [flags]
```

**Subcommands**

* [`polar custom_fields create`](#polar-custom_fields-create) Create a custom field.
* [`polar custom_fields delete`](#polar-custom_fields-delete) Delete a custom field.
* [`polar custom_fields get`](#polar-custom_fields-get) Get a custom field by ID.
* [`polar custom_fields list`](#polar-custom_fields-list) List custom fields.
* [`polar custom_fields update`](#polar-custom_fields-update) Update a custom field.

## polar custom\_fields create

Create a custom field.

```bash theme={null}
polar custom_fields create [flags]
```

**Flags**

| Flag | Type | Description |
| - | - | - |
| `--data`, `-d` | `string` | JSON object; explicitly supplied flags override its top-level keys |
| `--metadata` | `string` | Key-value object allowing you to store additional information. JSON: \{"\<key>": string \| integer \| number \| boolean} |
| `--type` | `choice` | Required. type (choices: text, number, date, checkbox, select) |
| `--slug` | `string` | Required. Identifier of the custom field. It'll be used as key when storing the value. Must be unique across the organization.It can only contain ASCII letters, numbers and hyphens. |
| `--name` | `string` | Required. Name of the custom field. |
| `--organization-id`, `--org` | `string` | The ID of the organization owning the custom field. Defaults to the active organization. |
| `--properties` | `string` | Required. properties JSON: \{...} \| \{"options": array of \{"value": string, "label": string}, ...} |

## polar custom\_fields delete

Delete a custom field.

```bash theme={null}
polar custom_fields delete [flags] <id>
```

**Arguments**

| Argument | Type | Description |
| - | - | - |
| `id` | `string` | |

**Flags**

| Flag | Type | Description |
| - | - | - |
| `--confirm`, `-c` | `boolean` | Skip the confirmation prompt for destructive requests |

## polar custom\_fields get

Get a custom field by ID.

```bash theme={null}
polar custom_fields get [flags] <id>
```

**Arguments**

| Argument | Type | Description |
| - | - | - |
| `id` | `string` | |

## polar custom\_fields list

List custom fields.

```bash theme={null}
polar custom_fields list [flags]
```

**Flags**

| Flag | Type | Description |
| - | - | - |
| `--data`, `-d` | `string` | JSON object; explicitly supplied flags override its top-level keys |
| `--organization-id`, `--org` | `string` | Filter by organization ID. Defaults to the active organization. |
| `--query` | `string` | Filter by custom field name or slug. |
| `--type` | `choice` | Filter by custom field type. (choices: text, number, date, checkbox, select) |
| `--page` | `integer` | Page number, defaults to 1. |
| `--limit` | `integer` | Size of a page, defaults to 10. Maximum is 100. |
| `--sorting` | `choice` | Sorting criterion. Several criteria can be used simultaneously and will be applied in order. Add a minus sign `-` before the criteria name to sort by descending order. (choices: created\_at, -created\_at, slug, -slug, name, -name, type, -type) |

## polar custom\_fields update

Update a custom field.

```bash theme={null}
polar custom_fields update [flags] <id>
```

**Arguments**

| Argument | Type | Description |
| - | - | - |
| `id` | `string` | |

**Flags**

| Flag | Type | Description |
| - | - | - |
| `--data`, `-d` | `string` | JSON object; explicitly supplied flags override its top-level keys |
| `--metadata` | `string` | Key-value object allowing you to store additional information. JSON: \{"\<key>": string \| integer \| number \| boolean} |
| `--name` | `string` | name |
| `--slug` | `string` | slug |
| `--type` | `choice` | Required. type (choices: text, number, date, checkbox, select) |
| `--properties` | `string` | properties JSON: \{...} \| \{"options": array of \{"value": string, "label": string}, ...} |


This documentation is built and hosted on [Mintlify](https://mintlify.com), a developer documentation platform.
