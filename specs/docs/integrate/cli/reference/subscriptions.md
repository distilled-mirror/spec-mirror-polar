> ## Documentation Index
> Fetch the complete documentation index at: https://polar.sh/docs/llms.txt
> Use this file to discover all available pages before exploring further.

# polar subscriptions

```bash theme={null}
polar subscriptions <subcommand> [flags]
```

**Subcommands**

* [`polar subscriptions get`](#polar-subscriptions-get) Get a subscription by ID.
* [`polar subscriptions list`](#polar-subscriptions-list) List subscriptions.
* [`polar subscriptions revoke`](#polar-subscriptions-revoke) Revoke a subscription, i.e cancel immediately.
* [`polar subscriptions update`](#polar-subscriptions-update) Update a subscription.

## polar subscriptions get

Get a subscription by ID.

```bash theme={null}
polar subscriptions get [flags] <id>
```

**Arguments**

| Argument | Type | Description |
| - | - | - |
| `id` | `string` | |

## polar subscriptions list

List subscriptions.

```bash theme={null}
polar subscriptions list [flags]
```

**Flags**

| Flag | Type | Description |
| - | - | - |
| `--data`, `-d` | `string` | JSON object; explicitly supplied flags override its top-level keys |
| `--organization-id`, `--org` | `string` | Filter by organization ID. Defaults to the active organization. |
| `--product-id` | `string` | Filter by product ID. |
| `--customer-id` | `string` | Filter by customer ID. |
| `--external-customer-id` | `string` | Filter by customer external ID. |
| `--discount-id` | `string` | Filter by discount ID. |
| `--active` | `boolean` | Filter by active or inactive subscription. |
| `--status` | `choice` | Filter by subscription status. (choices: incomplete, incomplete\_expired, trialing, active, past\_due, canceled, unpaid, paused) |
| `--cancel-at-period-end` | `boolean` | Filter by subscriptions that are set to cancel at period end. |
| `--customer-cancellation-reason` | `choice` | Filter by customer cancellation reason. (choices: customer\_service, low\_quality, missing\_features, switched\_service, too\_complex, too\_expensive, unused, other) |
| `--canceled-at-after` | `string` | Filter by cancellation date (after or equal to). |
| `--canceled-at-before` | `string` | Filter by cancellation date (before or equal to). |
| `--started-after` | `string` | Only include subscriptions started after this date. |
| `--started-before` | `string` | Only include subscriptions started before this date. |
| `--page` | `integer` | Page number, defaults to 1. |
| `--limit` | `integer` | Size of a page, defaults to 10. Maximum is 100. |
| `--sorting` | `choice` | Sorting criterion. Several criteria can be used simultaneously and will be applied in order. Add a minus sign `-` before the criteria name to sort by descending order. (choices: customer, -customer, status, -status, started\_at, -started\_at, current\_period\_end, -current\_period\_end, ended\_at, -ended\_at, ends\_at, -ends\_at, amount, -amount, product, -product, discount, -discount) |
| `--metadata` | `string` | Filter by metadata key-value pairs. JSON: \{"\<key>": string \| integer \| boolean \| array of string \| array of integer \| array of boolean} |

## polar subscriptions revoke

Revoke a subscription, i.e cancel immediately.

```bash theme={null}
polar subscriptions revoke [flags] <id>
```

**Arguments**

| Argument | Type | Description |
| - | - | - |
| `id` | `string` | |

**Flags**

| Flag | Type | Description |
| - | - | - |
| `--confirm`, `-c` | `boolean` | Skip the confirmation prompt for destructive requests |

## polar subscriptions update

Update a subscription.

```bash theme={null}
polar subscriptions update [flags] <id>
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
| `--product-id` | `string` | Update subscription to another product. |
| `--proration-behavior` | `choice` | Determine how to handle the proration billing. If not provided, will use the default organization setting. (choices: invoice, prorate, next\_period, reset) |
| `--discount-id` | `string` | Update the subscription to apply a new discount. If set to `null`, the discount will be removed. The change will be applied on the next billing cycle. |
| `--trial-end` | `string` | Set or extend the trial period of the subscription. If set to `now`, the trial will end immediately and the first billing cycle will be charged synchronously. The subscription remains trialing if the payment fails. |
| `--seats` | `integer` | Update the number of seats for this subscription. |
| `--units` | `integer` | Update the number of units for this subscription. |
| `--current-billing-period-end` | `string` | Set a new date for the end of the current billing period. The subscription will renew on this date. The new date can be earlier or later than the current period end, as long as it's in the future. |
| `--customer-cancellation-reason` | `choice` | Customer reason for cancellation. (choices: customer\_service, low\_quality, missing\_features, switched\_service, too\_complex, too\_expensive, unused, other) |
| `--customer-cancellation-comment` | `string` | Customer feedback and why they decided to cancel. |
| `--cancel-at-period-end` | `boolean` | Cancel an active subscription once the current period ends. |
| `--revoke` | `string` | Cancel and revoke an active subscription immediately JSON: true |
| `--pause-at-period-end` | `boolean` | Pause an active subscription at the end of the current period. |
| `--resumes-at` | `string` | Date at which the paused subscription should automatically resume. |
| `--resume` | `string` | Resume a paused subscription immediately, starting a new billing period and charging the customer. JSON: true |
| `--pending-update` | `string` | Clear the pending subscription update. Set to null to remove scheduled changes. JSON: null |


This documentation is built and hosted on [Mintlify](https://mintlify.com), a developer documentation platform.
