> ## Documentation Index
> Fetch the complete documentation index at: https://polar.sh/docs/llms.txt
> Use this file to discover all available pages before exploring further.

# polar files

```bash theme={null}
polar files <subcommand> [flags]
```

**Subcommands**

* [`polar files delete`](#polar-files-delete) Delete a file.
* [`polar files list`](#polar-files-list) List files.

## polar files delete

Delete a file.

```bash theme={null}
polar files delete [flags] <id>
```

**Arguments**

| Argument | Type | Description |
| - | - | - |
| `id` | `string` | |

**Flags**

| Flag | Type | Description |
| - | - | - |
| `--confirm`, `-c` | `boolean` | Skip the confirmation prompt for destructive requests |

## polar files list

List files.

```bash theme={null}
polar files list [flags]
```

**Flags**

| Flag | Type | Description |
| - | - | - |
| `--fields` | `string` | Only print these fields, comma-separated; use dots for nested ones, e.g. id,name,prices.price\_amount |
| `--data`, `-d` | `string` | JSON object; explicitly supplied flags override its top-level keys |
| `--organization-id`, `--org` | `string` | Filter by organization ID. Defaults to the active organization. |
| `--ids` | `string` | Filter by file ID. |
| `--page` | `integer` | Page number, defaults to 1. |
| `--limit` | `integer` | Size of a page, defaults to 10. Maximum is 100. |


This documentation is built and hosted on [Mintlify](https://mintlify.com), a developer documentation platform.
