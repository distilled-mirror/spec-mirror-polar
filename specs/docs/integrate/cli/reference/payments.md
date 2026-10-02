> ## Documentation Index
> Fetch the complete documentation index at: https://polar.sh/docs/llms.txt
> Use this file to discover all available pages before exploring further.

# polar payments

```bash theme={null}
polar payments <subcommand> [flags]
```

**Subcommands**

* [`polar payments get`](#polar-payments-get) Get a payment by ID.
* [`polar payments list`](#polar-payments-list) List payments.

## polar payments get

Get a payment by ID.

```bash theme={null}
polar payments get [flags] <id>
```

**Arguments**

| Argument | Type | Description |
| - | - | - |
| `id` | `string` | |

## polar payments list

List payments.

```bash theme={null}
polar payments list [flags]
```

**Flags**

| Flag | Type | Description |
| - | - | - |
| `--data`, `-d` | `string` | JSON object; explicitly supplied flags override its top-level keys |
| `--organization-id`, `--org` | `string` | Filter by organization ID. Defaults to the active organization. |
| `--checkout-id` | `string` | Filter by checkout ID. |
| `--order-id` | `string` | Filter by order ID. |
| `--customer-id` | `string` | Filter by customer ID. |
| `--status` | `choice` | Filter by payment status. (choices: pending, succeeded, failed) |
| `--method` | `string` | Filter by payment method. |
| `--customer-email` | `string` | Filter by customer email. |
| `--page` | `integer` | Page number, defaults to 1. |
| `--limit` | `integer` | Size of a page, defaults to 10. Maximum is 100. |
| `--sorting` | `choice` | Sorting criterion. Several criteria can be used simultaneously and will be applied in order. Add a minus sign `-` before the criteria name to sort by descending order. (choices: created\_at, -created\_at, status, -status, amount, -amount, method, -method) |


This documentation is built and hosted on [Mintlify](https://mintlify.com), a developer documentation platform.
