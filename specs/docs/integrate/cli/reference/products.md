> ## Documentation Index
> Fetch the complete documentation index at: https://polar.sh/docs/llms.txt
> Use this file to discover all available pages before exploring further.

# polar products

```bash theme={null}
polar products <subcommand> [flags]
```

**Subcommands**

* [`polar products create`](#polar-products-create) Create a product.
* [`polar products delete`](#polar-products-delete) Delete a product.
* [`polar products get`](#polar-products-get) Get a product by ID.
* [`polar products list`](#polar-products-list) List products.
* [`polar products update`](#polar-products-update) Update a product.
* [`polar products update_benefits`](#polar-products-update_benefits) Update benefits granted by a product.

## polar products create

Create a product.

```bash theme={null}
polar products create [flags]
```

**Flags**

| Flag | Type | Description |
| - | - | - |
| `--data`, `-d` | `string` | JSON object; explicitly supplied flags override its top-level keys |
| `--metadata` | `string` | Key-value object allowing you to store additional information. JSON: \{"\<key>": string \| integer \| number \| boolean} |
| `--name` | `string` | Required. The name of the product. |
| `--description` | `string` | The description of the product. |
| `--visibility` | `choice` | The visibility of the product. (choices: draft, private, public) |
| `--prices` | `string` | Required. List of available prices for this product. It may combine at most one fixed price with one seat-based price (billed as `fixed + seat_charge`), or contain a single custom or free price, plus any number of metered prices. A free price cannot be combined with other prices, and a custom price cannot be combined with a fixed or seat-based price. Metered prices are not supported on one-time purchase products. JSON: array of (\{"amount\_type": "fixed", "price\_amount": integer, ...} \| \{"amount\_type": "custom", ...} \| \{"amount\_type": "seat\_based", "seat\_tiers": \{"tiers": array of \{...}, ...}, ...} \| \{"amount\_type": "unit\_based", "tiers": \{"type": "volume" \| "graduated", "tiers": array of \{...}}, ...} \| \{"amount\_type": "metered\_unit", "meter\_id": string, "unit\_amount": number \| string, ...} \| \{"amount\_type": "metered\_tiers", "meter\_id": string, "tiers": \{"type": "volume" \| "graduated", "tiers": array of \{...}}, ...}) |
| `--medias` | `string` | List of file IDs. Each one must be on the same organization as the product, of type `product_media` and correctly uploaded. |
| `--attached-custom-fields` | `string` | List of custom fields to attach. JSON: array of \{"custom\_field\_id": string, "required": boolean} |
| `--organization-id`, `--org` | `string` | The ID of the organization owning the product. Defaults to the active organization. |
| `--trial-interval` | `choice` | The interval unit for the trial period. (choices: day, week, month, year) |
| `--trial-interval-count` | `integer` | The number of interval units for the trial period. |
| `--recurring-interval` | `choice` | The recurring interval of the product. (choices: day, week, month, year) |
| `--recurring-interval-count` | `integer` | Number of interval units of the subscription. If this is set to 1 the charge will happen every interval (e.g. every month), if set to 2 it will be every other month, and so on. |
| `--meter-interval` | `choice` | Optional meter cycle, independent of the billing interval. When set, overage settlement, meter resets and meter-credit grants run on this cadence rather than the billing interval — e.g. yearly billing with monthly credits. It must evenly divide the billing interval. If `None`, metered concerns follow the billing interval. Once set, it can't be changed. (choices: day, week, month, year) |
| `--meter-interval-count` | `integer` | Number of meter interval units. Defaults to 1 when `meter_interval` is set. Ignored when `meter_interval` is `None`. |

## polar products delete

Delete a product.

```bash theme={null}
polar products delete [flags] <id>
```

**Arguments**

| Argument | Type | Description |
| - | - | - |
| `id` | `string` | |

**Flags**

| Flag | Type | Description |
| - | - | - |
| `--confirm`, `-c` | `boolean` | Skip the confirmation prompt for destructive requests |

## polar products get

Get a product by ID.

```bash theme={null}
polar products get [flags] <id>
```

**Arguments**

| Argument | Type | Description |
| - | - | - |
| `id` | `string` | |

## polar products list

List products.

```bash theme={null}
polar products list [flags]
```

**Flags**

| Flag | Type | Description |
| - | - | - |
| `--data`, `-d` | `string` | JSON object; explicitly supplied flags override its top-level keys |
| `--id` | `string` | Filter by product ID. |
| `--organization-id`, `--org` | `string` | Filter by organization ID. Defaults to the active organization. |
| `--query` | `string` | Filter by product name. |
| `--is-archived` | `boolean` | Filter on archived products. |
| `--is-recurring` | `boolean` | Filter on recurring products. If `true`, only subscriptions tiers are returned. If `false`, only one-time purchase products are returned. |
| `--benefit-id` | `string` | Filter products granting specific benefit. |
| `--visibility` | `choice` | Filter by visibility. (choices: draft, private, public) |
| `--page` | `integer` | Page number, defaults to 1. |
| `--limit` | `integer` | Size of a page, defaults to 10. Maximum is 100. |
| `--sorting` | `choice` | Sorting criterion. Several criteria can be used simultaneously and will be applied in order. Add a minus sign `-` before the criteria name to sort by descending order. (choices: created\_at, -created\_at, name, -name, price\_amount\_type, -price\_amount\_type, price\_amount, -price\_amount) |
| `--metadata` | `string` | Filter by metadata key-value pairs. JSON: \{"\<key>": string \| integer \| boolean \| array of string \| array of integer \| array of boolean} |

## polar products update

Update a product.

```bash theme={null}
polar products update [flags] <id>
```

**Arguments**

| Argument | Type | Description |
| - | - | - |
| `id` | `string` | |

**Flags**

| Flag | Type | Description |
| - | - | - |
| `--confirm`, `-c` | `boolean` | Skip the confirmation prompt for destructive requests |
| `--data`, `-d` | `string` | JSON object; explicitly supplied flags override its top-level keys |
| `--metadata` | `string` | Key-value object allowing you to store additional information. JSON: \{"\<key>": string \| integer \| number \| boolean} |
| `--trial-interval` | `choice` | The interval unit for the trial period. (choices: day, week, month, year) |
| `--trial-interval-count` | `integer` | The number of interval units for the trial period. |
| `--name` | `string` | name |
| `--description` | `string` | The description of the product. |
| `--recurring-interval` | `choice` | The recurring interval of the product. If `None`, the product is a one-time purchase. Can only be set on legacy recurring products. Once set, it can't be changed. (choices: day, week, month, year) |
| `--recurring-interval-count` | `integer` | Number of interval units of the subscription. If this is set to 1 the charge will happen every interval (e.g. every month), if set to 2 it will be every other month, and so on. Once set, it can't be changed. |
| `--is-archived` | `boolean` | Whether the product is archived. If `true`, the product won't be available for purchase anymore. Existing customers will still have access to their benefits, and subscriptions will continue normally. |
| `--visibility` | `choice` | The visibility of the product. (choices: draft, private, public) |
| `--prices` | `string` | List of available prices for this product. If you want to keep existing prices, include them in the list as an `ExistingProductPrice` object. JSON: array of (\{"id": string} \| \{"amount\_type": "fixed", "price\_amount": integer, ...} \| \{"amount\_type": "custom", ...} \| \{"amount\_type": "seat\_based", "seat\_tiers": \{"tiers": array of \{...}, ...}, ...} \| \{"amount\_type": "unit\_based", "tiers": \{"type": "volume" \| "graduated", "tiers": array of \{...}}, ...} \| \{"amount\_type": "metered\_unit", "meter\_id": string, "unit\_amount": number \| string, ...} \| \{"amount\_type": "metered\_tiers", "meter\_id": string, "tiers": \{"type": "volume" \| "graduated", "tiers": array of \{...}}, ...}) |
| `--medias` | `string` | List of file IDs. Each one must be on the same organization as the product, of type `product_media` and correctly uploaded. |
| `--attached-custom-fields` | `string` | attached\_custom\_fields JSON: array of \{"custom\_field\_id": string, "required": boolean} |

## polar products update\_benefits

Update benefits granted by a product.

```bash theme={null}
polar products update_benefits [flags] <id>
```

**Arguments**

| Argument | Type | Description |
| - | - | - |
| `id` | `string` | |

**Flags**

| Flag | Type | Description |
| - | - | - |
| `--data`, `-d` | `string` | JSON object; explicitly supplied flags override its top-level keys |
| `--benefits` | `string` | Required. List of benefit IDs. Each one must be on the same organization as the product. |


This documentation is built and hosted on [Mintlify](https://mintlify.com), a developer documentation platform.
