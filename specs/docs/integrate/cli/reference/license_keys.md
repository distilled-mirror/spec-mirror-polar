> ## Documentation Index
> Fetch the complete documentation index at: https://polar.sh/docs/llms.txt
> Use this file to discover all available pages before exploring further.

# polar license_keys

```bash theme={null}
polar license_keys <subcommand> [flags]
```

**Subcommands**

* [`polar license_keys get`](#polar-license_keys-get) Get a license key.
* [`polar license_keys get_activation`](#polar-license_keys-get_activation) Get a license key activation.
* [`polar license_keys list`](#polar-license_keys-list) Get license keys connected to the given organization & filters.
* [`polar license_keys rotate`](#polar-license_keys-rotate) Rotate a license key.
* [`polar license_keys update`](#polar-license_keys-update) Update a license key.

## polar license\_keys get

Get a license key.

```bash theme={null}
polar license_keys get [flags] <id>
```

**Arguments**

| Argument | Type | Description |
| - | - | - |
| `id` | `string` | |

**Flags**

| Flag | Type | Description |
| - | - | - |
| `--fields` | `string` | Only print these fields, comma-separated; use dots for nested ones, e.g. id,name,prices.price\_amount |

## polar license\_keys get\_activation

Get a license key activation.

```bash theme={null}
polar license_keys get_activation [flags] <id> <activation_id>
```

**Arguments**

| Argument | Type | Description |
| - | - | - |
| `id` | `string` | |
| `activation_id` | `string` | |

**Flags**

| Flag | Type | Description |
| - | - | - |
| `--fields` | `string` | Only print these fields, comma-separated; use dots for nested ones, e.g. id,name,prices.price\_amount |

## polar license\_keys list

Get license keys connected to the given organization & filters.

```bash theme={null}
polar license_keys list [flags]
```

**Flags**

| Flag | Type | Description |
| - | - | - |
| `--fields` | `string` | Only print these fields, comma-separated; use dots for nested ones, e.g. id,name,prices.price\_amount |
| `--data`, `-d` | `string` | JSON object; explicitly supplied flags override its top-level keys |
| `--organization-id`, `--org` | `string` | Filter by organization ID. Defaults to the active organization. |
| `--benefit-id` | `string` | Filter by benefit ID. |
| `--status` | `choice` | Filter by license key status. (choices: granted, revoked, disabled) |
| `--page` | `integer` | Page number, defaults to 1. |
| `--limit` | `integer` | Size of a page, defaults to 10. Maximum is 100. |

## polar license\_keys rotate

Rotate a license key.

```bash theme={null}
polar license_keys rotate [flags] <id>
```

**Arguments**

| Argument | Type | Description |
| - | - | - |
| `id` | `string` | |

**Flags**

| Flag | Type | Description |
| - | - | - |
| `--confirm`, `-c` | `boolean` | Skip the confirmation prompt for destructive requests |
| `--fields` | `string` | Only print these fields, comma-separated; use dots for nested ones, e.g. id,name,prices.price\_amount |

## polar license\_keys update

Update a license key.

```bash theme={null}
polar license_keys update [flags] <id>
```

**Arguments**

| Argument | Type | Description |
| - | - | - |
| `id` | `string` | |

**Flags**

| Flag | Type | Description |
| - | - | - |
| `--confirm`, `-c` | `boolean` | Skip the confirmation prompt for destructive requests |
| `--fields` | `string` | Only print these fields, comma-separated; use dots for nested ones, e.g. id,name,prices.price\_amount |
| `--data`, `-d` | `string` | JSON object; explicitly supplied flags override its top-level keys |
| `--status` | `choice` | status (choices: granted, revoked, disabled) |
| `--usage` | `integer` | usage |
| `--limit-activations` | `integer` | limit\_activations |
| `--limit-usage` | `integer` | limit\_usage |
| `--expires-at` | `string` | expires\_at |


This documentation is built and hosted on [Mintlify](https://mintlify.com), a developer documentation platform.
