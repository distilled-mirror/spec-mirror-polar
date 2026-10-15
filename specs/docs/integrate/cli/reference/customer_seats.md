> ## Documentation Index
> Fetch the complete documentation index at: https://polar.sh/docs/llms.txt
> Use this file to discover all available pages before exploring further.

# polar customer_seats

```bash theme={null}
polar customer_seats <subcommand> [flags]
```

**Subcommands**

* [`polar customer_seats assign_seat`](#polar-customer_seats-assign_seat) Assign a seat for a subscription or order.
* [`polar customer_seats claim_seat`](#polar-customer_seats-claim_seat) Claim a customer seat with an invitation token.
* [`polar customer_seats get_claim_info`](#polar-customer_seats-get_claim_info) Get claim information for a customer seat invitation.
* [`polar customer_seats list_seats`](#polar-customer_seats-list_seats) List seats for a subscription or order.
* [`polar customer_seats resend_invitation`](#polar-customer_seats-resend_invitation) Resend an invitation for a pending customer seat.
* [`polar customer_seats revoke_seat`](#polar-customer_seats-revoke_seat) Revoke a customer seat.

## polar customer\_seats assign\_seat

Assign a seat for a subscription or order.

```bash theme={null}
polar customer_seats assign_seat [flags]
```

**Flags**

| Flag | Type | Description |
| - | - | - |
| `--fields` | `string` | Only print these fields, comma-separated; use dots for nested ones, e.g. id,name,prices.price\_amount |
| `--data`, `-d` | `string` | JSON object; explicitly supplied flags override its top-level keys |
| `--subscription-id` | `string` | Subscription ID. Required if neither order\_id nor checkout\_id is provided. |
| `--order-id` | `string` | Order ID for one-time purchases. Required if subscription\_id is not provided. |
| `--email` | `string` | Email of the customer to assign the seat to |
| `--external-customer-id` | `string` | External customer ID for the seat assignment |
| `--customer-id` | `string` | Customer ID for the seat assignment |
| `--external-member-id` | `string` | External member ID for the seat assignment. Can be used alone (lookup existing member) or with email (create/validate member). |
| `--member-id` | `string` | Member ID for the seat assignment. |
| `--metadata` | `string` | Additional metadata for the seat (max 10 keys, 1KB total) JSON: \{"\<key>": any} |
| `--immediate-claim` | `boolean` | If true, the seat will be immediately claimed without sending an invitation email. API-only feature. |

## polar customer\_seats claim\_seat

Claim a customer seat with an invitation token.

```bash theme={null}
polar customer_seats claim_seat [flags]
```

**Flags**

| Flag | Type | Description |
| - | - | - |
| `--environment` | `choice` | Environment for this unauthenticated request (choices: production, sandbox) |
| `--fields` | `string` | Only print these fields, comma-separated; use dots for nested ones, e.g. id,name,prices.price\_amount |
| `--data`, `-d` | `string` | JSON object; explicitly supplied flags override its top-level keys |
| `--invitation-token` | `string` | Required. Invitation token to claim the seat |

## polar customer\_seats get\_claim\_info

Get claim information for a customer seat invitation.

```bash theme={null}
polar customer_seats get_claim_info [flags] <invitation_token>
```

**Arguments**

| Argument | Type | Description |
| - | - | - |
| `invitation_token` | `string` | |

**Flags**

| Flag | Type | Description |
| - | - | - |
| `--environment` | `choice` | Environment for this unauthenticated request (choices: production, sandbox) |
| `--fields` | `string` | Only print these fields, comma-separated; use dots for nested ones, e.g. id,name,prices.price\_amount |

## polar customer\_seats list\_seats

List seats for a subscription or order.

```bash theme={null}
polar customer_seats list_seats [flags]
```

**Flags**

| Flag | Type | Description |
| - | - | - |
| `--fields` | `string` | Only print these fields, comma-separated; use dots for nested ones, e.g. id,name,prices.price\_amount |
| `--data`, `-d` | `string` | JSON object; explicitly supplied flags override its top-level keys |
| `--subscription-id` | `string` | subscription\_id |
| `--order-id` | `string` | order\_id |

## polar customer\_seats resend\_invitation

Resend an invitation for a pending customer seat.

```bash theme={null}
polar customer_seats resend_invitation [flags] <seat_id>
```

**Arguments**

| Argument | Type | Description |
| - | - | - |
| `seat_id` | `string` | |

**Flags**

| Flag | Type | Description |
| - | - | - |
| `--fields` | `string` | Only print these fields, comma-separated; use dots for nested ones, e.g. id,name,prices.price\_amount |

## polar customer\_seats revoke\_seat

Revoke a customer seat.

```bash theme={null}
polar customer_seats revoke_seat [flags] <seat_id>
```

**Arguments**

| Argument | Type | Description |
| - | - | - |
| `seat_id` | `string` | |

**Flags**

| Flag | Type | Description |
| - | - | - |
| `--confirm`, `-c` | `boolean` | Skip the confirmation prompt for destructive requests |
| `--fields` | `string` | Only print these fields, comma-separated; use dots for nested ones, e.g. id,name,prices.price\_amount |


This documentation is built and hosted on [Mintlify](https://mintlify.com), a developer documentation platform.
