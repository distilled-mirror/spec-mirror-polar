> ## Documentation Index
> Fetch the complete documentation index at: https://polar.sh/docs/llms.txt
> Use this file to discover all available pages before exploring further.

# polar orders

```bash theme={null}
polar orders <subcommand> [flags]
```

**Subcommands**

* [`polar orders generate_invoice`](#polar-orders-generate_invoice) Trigger generation of an order's invoice.
* [`polar orders get`](#polar-orders-get) Get an order by ID.
* [`polar orders invoice`](#polar-orders-invoice) Get an order's invoice data.
* [`polar orders list`](#polar-orders-list) List orders.
* [`polar orders receipt`](#polar-orders-receipt) Get a presigned URL to download an order's receipt PDF.
* [`polar orders update`](#polar-orders-update) Update an order.

## polar orders generate\_invoice

Trigger generation of an order's invoice.

```bash theme={null}
polar orders generate_invoice [flags] <id>
```

**Arguments**

| Argument | Type | Description |
| - | - | - |
| `id` | `string` | |

## polar orders get

Get an order by ID.

```bash theme={null}
polar orders get [flags] <id>
```

**Arguments**

| Argument | Type | Description |
| - | - | - |
| `id` | `string` | |

## polar orders invoice

Get an order's invoice data.

```bash theme={null}
polar orders invoice [flags] <id>
```

**Arguments**

| Argument | Type | Description |
| - | - | - |
| `id` | `string` | |

## polar orders list

List orders.

```bash theme={null}
polar orders list [flags]
```

**Flags**

| Flag | Type | Description |
| - | - | - |
| `--data`, `-d` | `string` | JSON object; explicitly supplied flags override its top-level keys |
| `--organization-id`, `--org` | `string` | Filter by organization ID. Defaults to the active organization. |
| `--product-id` | `string` | Filter by product ID. |
| `--product-billing-type` | `choice` | Filter by product billing type. `recurring` will filter data corresponding to subscriptions creations or renewals. `one_time` will filter data corresponding to one-time purchases. (choices: one\_time, recurring) |
| `--discount-id` | `string` | Filter by discount ID. |
| `--customer-id` | `string` | Filter by customer ID. |
| `--external-customer-id` | `string` | Filter by customer external ID. |
| `--checkout-id` | `string` | Filter by checkout ID. |
| `--subscription-id` | `string` | Filter by subscription ID. |
| `--status` | `choice` | Filter by order status. (choices: draft, pending, paid, refunded, partially\_refunded, void) |
| `--created-after` | `string` | Only include orders created after this date |
| `--created-before` | `string` | Only include orders created before this date |
| `--page` | `integer` | Page number, defaults to 1. |
| `--limit` | `integer` | Size of a page, defaults to 10. Maximum is 100. |
| `--sorting` | `choice` | Sorting criterion. Several criteria can be used simultaneously and will be applied in order. Add a minus sign `-` before the criteria name to sort by descending order. (choices: created\_at, -created\_at, status, -status, invoice\_number, -invoice\_number, amount, -amount, net\_amount, -net\_amount, customer, -customer, product, -product, discount, -discount, subscription, -subscription) |
| `--metadata` | `string` | Filter by metadata key-value pairs. JSON: \{"\<key>": string \| integer \| boolean \| array of string \| array of integer \| array of boolean} |

## polar orders receipt

Get a presigned URL to download an order's receipt PDF.

```bash theme={null}
polar orders receipt [flags] <id>
```

**Arguments**

| Argument | Type | Description |
| - | - | - |
| `id` | `string` | |

## polar orders update

Update an order.

```bash theme={null}
polar orders update [flags] <id>
```

**Arguments**

| Argument | Type | Description |
| - | - | - |
| `id` | `string` | |

**Flags**

| Flag | Type | Description |
| - | - | - |
| `--data`, `-d` | `string` | JSON object; explicitly supplied flags override its top-level keys |
| `--billing-name` | `string` | The name of the customer that should appear on the invoice. |
| `--billing-address` | `string` | The address of the customer that should appear on the invoice. Country and state fields cannot be updated. JSON: \{"country": "AD" \| "AE" \| "AF" \| "AG" \| "AI" \| ..., ...} |


This documentation is built and hosted on [Mintlify](https://mintlify.com), a developer documentation platform.
