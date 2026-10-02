> ## Documentation Index
> Fetch the complete documentation index at: https://polar.sh/docs/llms.txt
> Use this file to discover all available pages before exploring further.

# polar benefits

```bash theme={null}
polar benefits <subcommand> [flags]
```

**Subcommands**

* [`polar benefits create`](#polar-benefits-create) Create a benefit.
* [`polar benefits delete`](#polar-benefits-delete) Delete a benefit.
* [`polar benefits files`](#polar-benefits-files) List the downloadable files for a benefit with their download statistics.
* [`polar benefits get`](#polar-benefits-get) Get a benefit by ID.
* [`polar benefits grants`](#polar-benefits-grants) List the individual grants for a benefit.
* [`polar benefits list`](#polar-benefits-list) List benefits.
* [`polar benefits update`](#polar-benefits-update) Update a benefit.

## polar benefits create

Create a benefit.

```bash theme={null}
polar benefits create [flags]
```

**Flags**

| Flag | Type | Description |
| - | - | - |
| `--data`, `-d` | `string` | JSON object; explicitly supplied flags override its top-level keys |
| `--metadata` | `string` | Key-value object allowing you to store additional information. JSON: \{"\<key>": string \| integer \| number \| boolean} |
| `--type` | `choice` | Required. type (choices: custom, discord, github\_repository, downloadables, license\_keys, meter\_credit, feature\_flag, slack\_shared\_channel) |
| `--description` | `string` | Required. The description of the benefit. Will be displayed on products having this benefit. |
| `--organization-id`, `--org` | `string` | The ID of the organization owning the benefit. Defaults to the active organization. |
| `--visibility` | `choice` | The visibility of the benefit in the customer portal. (choices: draft, private, public) |
| `--properties` | `string` | Required. properties JSON: \{...} \| \{"guild\_id": string, "role\_id": string, "kick\_member": boolean} \| \{"repository\_owner": string, "repository\_name": string, "permission": "pull" \| "triage" \| "push" \| "maintain" \| "admin"} \| \{"files": array of string, ...} \| \{"units": integer, "rollover": boolean, "meter\_id": string} \| \{} \| \{"slack\_integration\_id": string, "channel\_name\_template": string, ...} |

## polar benefits delete

Delete a benefit.

```bash theme={null}
polar benefits delete [flags] <id>
```

**Arguments**

| Argument | Type | Description |
| - | - | - |
| `id` | `string` | |

**Flags**

| Flag | Type | Description |
| - | - | - |
| `--confirm`, `-c` | `boolean` | Skip the confirmation prompt for destructive requests |

## polar benefits files

List the downloadable files for a benefit with their download statistics.

```bash theme={null}
polar benefits files [flags] <id>
```

**Arguments**

| Argument | Type | Description |
| - | - | - |
| `id` | `string` | |

**Flags**

| Flag | Type | Description |
| - | - | - |
| `--data`, `-d` | `string` | JSON object; explicitly supplied flags override its top-level keys |
| `--page` | `integer` | Page number, defaults to 1. |
| `--limit` | `integer` | Size of a page, defaults to 10. Maximum is 100. |

## polar benefits get

Get a benefit by ID.

```bash theme={null}
polar benefits get [flags] <id>
```

**Arguments**

| Argument | Type | Description |
| - | - | - |
| `id` | `string` | |

## polar benefits grants

List the individual grants for a benefit.

```bash theme={null}
polar benefits grants [flags] <id>
```

**Arguments**

| Argument | Type | Description |
| - | - | - |
| `id` | `string` | |

**Flags**

| Flag | Type | Description |
| - | - | - |
| `--data`, `-d` | `string` | JSON object; explicitly supplied flags override its top-level keys |
| `--is-granted` | `boolean` | Filter by granted status. If `true`, only granted benefits will be returned. If `false`, only revoked benefits will be returned. |
| `--customer-id` | `string` | Filter by customer. |
| `--member-id` | `string` | Filter by member. |
| `--page` | `integer` | Page number, defaults to 1. |
| `--limit` | `integer` | Size of a page, defaults to 10. Maximum is 100. |

## polar benefits list

List benefits.

```bash theme={null}
polar benefits list [flags]
```

**Flags**

| Flag | Type | Description |
| - | - | - |
| `--data`, `-d` | `string` | JSON object; explicitly supplied flags override its top-level keys |
| `--organization-id`, `--org` | `string` | Filter by organization ID. Defaults to the active organization. |
| `--type` | `choice` | Filter by benefit type. (choices: custom, discord, github\_repository, downloadables, license\_keys, meter\_credit, feature\_flag, slack\_shared\_channel) |
| `--id` | `string` | Filter by benefit IDs. |
| `--exclude-id` | `string` | Exclude benefits with these IDs. |
| `--query` | `string` | Filter by description. |
| `--page` | `integer` | Page number, defaults to 1. |
| `--limit` | `integer` | Size of a page, defaults to 10. Maximum is 100. |
| `--sorting` | `choice` | Sorting criterion. Several criteria can be used simultaneously and will be applied in order. Add a minus sign `-` before the criteria name to sort by descending order. (choices: created\_at, -created\_at, description, -description, type, -type, user\_order, -user\_order) |
| `--metadata` | `string` | Filter by metadata key-value pairs. JSON: \{"\<key>": string \| integer \| boolean \| array of string \| array of integer \| array of boolean} |

## polar benefits update

Update a benefit.

```bash theme={null}
polar benefits update [flags] <id>
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
| `--description` | `string` | The description of the benefit. Will be displayed on products having this benefit. |
| `--visibility` | `choice` | The visibility of the benefit in the customer portal. (choices: draft, private, public) |
| `--type` | `choice` | Required. type (choices: custom, discord, github\_repository, downloadables, license\_keys, meter\_credit, feature\_flag, slack\_shared\_channel) |
| `--properties` | `string` | properties JSON: \{"note": string \| null} \| \{"guild\_id": string, "role\_id": string, "kick\_member": boolean} \| \{"repository\_owner": string, "repository\_name": string, "permission": "pull" \| "triage" \| "push" \| "maintain" \| "admin"} \| \{"files": array of string, ...} \| \{...} \| \{"units": integer, "rollover": boolean, "meter\_id": string} \| \{} \| \{"slack\_integration\_id": string, "channel\_name\_template": string, ...} |


This documentation is built and hosted on [Mintlify](https://mintlify.com), a developer documentation platform.
