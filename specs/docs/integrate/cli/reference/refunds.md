> ## Documentation Index
> Fetch the complete documentation index at: https://polar.sh/docs/llms.txt
> Use this file to discover all available pages before exploring further.

# polar refunds

```bash theme={null}
polar refunds <subcommand> [flags]
```

**Subcommands**

* [`polar refunds create`](#polar-refunds-create) Create a refund.
* [`polar refunds list`](#polar-refunds-list) List refunds.

## polar refunds create

Create a refund.

```bash theme={null}
polar refunds create [flags]
```

**Flags**

| Flag | Type | Description |
| - | - | - |
| `--confirm`, `-c` | `boolean` | Skip the confirmation prompt for destructive requests |
| `--fields` | `string` | Only print these fields, comma-separated; use dots for nested ones, e.g. id,name,prices.price\_amount |
| `--data`, `-d` | `string` | JSON object; explicitly supplied flags override its top-level keys |
| `--metadata` | `string` | Key-value object allowing you to store additional information. JSON: \{"\<key>": string \| integer \| number \| boolean} |
| `--order-id` | `string` | Required. order\_id |
| `--reason` | `choice` | Required. Reason for the refund. (choices: duplicate, fraudulent, customer\_request, service\_disruption, satisfaction\_guarantee, other) |
| `--amount` | `integer` | Required. Amount to refund in cents. Minimum is 1. |
| `--comment` | `string` | An internal comment about the refund. |
| `--revoke-benefits` | `boolean` | Should this refund trigger the associated customer benefits to be revoked? |

## polar refunds list

List refunds.

```bash theme={null}
polar refunds list [flags]
```

**Flags**

| Flag | Type | Description |
| - | - | - |
| `--fields` | `string` | Only print these fields, comma-separated; use dots for nested ones, e.g. id,name,prices.price\_amount |
| `--data`, `-d` | `string` | JSON object; explicitly supplied flags override its top-level keys |
| `--id` | `string` | Filter by refund ID. |
| `--organization-id`, `--org` | `string` | Filter by organization ID. Defaults to the active organization. |
| `--order-id` | `string` | Filter by order ID. |
| `--subscription-id` | `string` | Filter by subscription ID. |
| `--customer-id` | `string` | Filter by customer ID. |
| `--external-customer-id` | `string` | Filter by customer external ID. |
| `--succeeded` | `boolean` | Filter by `succeeded`. |
| `--page` | `integer` | Page number, defaults to 1. |
| `--limit` | `integer` | Size of a page, defaults to 10. Maximum is 100. |
| `--sorting` | `choice` | Sorting criterion. Several criteria can be used simultaneously and will be applied in order. Add a minus sign `-` before the criteria name to sort by descending order. (choices: created\_at, -created\_at, amount, -amount) |


This documentation is built and hosted on [Mintlify](https://mintlify.com), a developer documentation platform.
