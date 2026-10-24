> ## Documentation Index
> Fetch the complete documentation index at: https://polar.sh/docs/llms.txt
> Use this file to discover all available pages before exploring further.

# Webhook Events

> Our webhook events and in which context they are useful

## Billing Events

### Checkout

> **[checkout.created](https://polar.sh/docs/api-reference/current/checkout_created.md)**

> **[checkout.updated](https://polar.sh/docs/api-reference/current/checkout_updated.md)**

> **[checkout.expired](https://polar.sh/docs/api-reference/current/checkout_expired.md)**
>
> Fired when a checkout link has expired without being completed.

### Customers

> **[customer.created](https://polar.sh/docs/api-reference/current/customer_created.md)**
>
> Fired when a new customer has been created.

> **[customer.updated](https://polar.sh/docs/api-reference/current/customer_updated.md)**
>
> Fired when a customer has been updated.

> **[customer.deleted](https://polar.sh/docs/api-reference/current/customer_deleted.md)**
>
> Fired when a customer has been deleted.

> **[customer.state\_changed](https://polar.sh/docs/api-reference/current/customer_state_changed.md)**
>
> Fired when a customer's state has changed. Includes active subscriptions and
> granted benefits.

### Subscriptions

In order to properly implement logic for handling subscriptions, you should look into the following events.

> **[subscription.created](https://polar.sh/docs/api-reference/current/subscription_created.md)**
>
> Fired when a new subscription has been created.

> **[subscription.active](https://polar.sh/docs/api-reference/current/subscription_active.md)**

> **[subscription.uncanceled](https://polar.sh/docs/api-reference/current/subscription_uncanceled.md)**

> **[subscription.cycled](https://polar.sh/docs/api-reference/current/subscription_cycled.md)**
>
> Fired when a subscription enters a new billing period, before the renewal order exists and whether or not the payment succeeds.

> **[subscription.canceled](https://polar.sh/docs/api-reference/current/subscription_canceled.md)**

> **[subscription.past\_due](https://polar.sh/docs/api-reference/current/subscription_past_due.md)**
>
> Fired when a subscription payment has failed. The customer can recover by updating their payment method.

> **[subscription.updated](https://polar.sh/docs/api-reference/current/subscription_updated.md)**
>
> Use this event if you want to handle cancellations, un-cancellations, etc. The
> updated event is a catch-all event for `subscription.active`,
> `subscription.canceled`, `subscription.uncanceled`, `subscription.cycled`,
> `subscription.past_due`, `subscription.revoked`, `subscription.paused`,
> `subscription.resumed` and `subscription.migrated`.

> **[order.created](https://polar.sh/docs/api-reference/current/order_created.md)**
>
> Carries a `billing_reason` field, which can be `purchase`,
> `subscription_create`, `subscription_cycle`, `subscription_update` and
> `subscription_meter_cycle`. To act
> on a renewal, listen to `subscription.cycled` instead: it fires whether or not
> the renewal payment succeeds.

> **[subscription.revoked](https://polar.sh/docs/api-reference/current/subscription_revoked.md)**

> **[subscription.paused](https://polar.sh/docs/api-reference/current/subscription_paused.md)**
>
> Fired when a scheduled pause takes effect at the end of the period. Billing stops and benefits are revoked until the subscription resumes.

> **[subscription.resumed](https://polar.sh/docs/api-reference/current/subscription_resumed.md)**
>
> Fired when a paused subscription resumes. A new billing period starts and the customer is charged immediately.

> **[subscription.migrated](https://polar.sh/docs/api-reference/current/subscription_migrated.md)**
>
> Fired when Polar takes over billing of a subscription migrated from another provider. Includes `provider` and `provider_subscription_id` so you can correlate the two. Does not send `subscription.created` or `subscription.active`.

#### Cancellation Sequences

When a subscription is canceled, the events triggered depend on whether the cancellation is immediate or scheduled for the end of the billing period.

**End-of-Period Cancellation (default)**

When a subscription is **canceled** (by customer action from the portal or by the merchant from the dashboard/API), the following events are sent immediately:

1. `subscription.updated`
2. `subscription.canceled`

Both events contain the same subscription data. The subscription will still have `active` status, but the `cancel_at_period_end` flag will be set to `true`.

When the end of the current billing period arrives, the subscription is definitively revoked: billing cycles stop and benefits are revoked. The following events are then sent:

3. `subscription.updated`
4. `subscription.revoked`

Both events contain the same subscription data. The subscription will have the `canceled` status.

**Immediate Revocation**

When a merchant cancels a subscription with **immediate revocation**, those events are sent at once:

1. `subscription.updated`
2. `subscription.canceled`
3. `subscription.revoked`

All three events contain the same subscription data. The subscription will have the `canceled` status immediately.

#### Renewal Sequences

When a subscription is renewed for a new cycle, the webhook events are triggered in a specific sequence to help you track the renewal process and handle billing logic appropriately.

**Initial Renewal Events**

When a subscription reaches its renewal date, the following events are sent immediately (if enabled on the webhook):

1. `subscription.cycled`
2. `subscription.updated`
3. `order.created`

`subscription.cycled` fires only on a new billing period, so you can act on a renewal without inspecting `billing_reason` on the order. It also fires when a trial converts to a paid subscription, since that starts a period too: read `status` to tell the two apart.

The subscription data will reflect the new billing period through the `current_period_start` and `current_period_end` properties, showing the updated cycle dates.

The order data represents the new invoice for the upcoming cycle, with a total representing what the customer will pay for this new period. If usage-based billing is involved, their consumption for the past period will be included in the total. The status of this order is `pending` at this stage.

**Payment Processing Events**

Shortly after the initial renewal events, the platform will trigger a payment for the new order. Once the payment is successfully processed, the following events are sent:

4. `order.updated`
5. `order.paid`

Both events will contain the same order data, with the order status changed to `paid`.

#### Pause Sequences

Pausing is scheduled for the end of the current billing period, much like an end-of-period cancellation.

When a subscription is **paused** (by customer action from the portal or by the merchant from the dashboard/API), only one event is sent immediately, because the subscription stays `active` until the period ends:

1. `subscription.updated`

The `pause_at_period_end` flag is set to `true`, and `resumes_at` holds the scheduled resume date if you set one.

When the end of the current billing period arrives, the pause takes effect: billing stops and benefits are revoked. The following events are then sent:

2. `subscription.updated`
3. `subscription.paused`

Both events contain the same subscription data, now with the `paused` status.

**Resume**

When a paused subscription resumes, either immediately when you resume it or automatically on its `resumes_at` date, a new billing period starts and the customer is charged right away. The following events are sent:

1. `subscription.updated`
2. `subscription.resumed`
3. `order.created`

The subscription data reflects the new period through `current_period_start` and `current_period_end`, and the order represents the immediate charge for the new cycle.

#### Migration Sequences

When Polar takes over billing of a subscription imported from another provider, these events are sent at cutover:

* `subscription.updated`
* `subscription.migrated`
* `customer.state_changed`
* `benefit_grant.created`, for any benefits attached to the product (seat-based products grant them when seats are claimed)
* `customer.updated`, if Polar sets the copied card as the customer's default payment method

Polar enqueues `subscription.updated` before `subscription.migrated`, but delivery order isn't guaranteed.

`subscription.migrated` carries `provider` and `provider_subscription_id` so you can match the Polar subscription to the source. Polar does not send `subscription.created`, `subscription.active`, or `subscription.resumed` for this path. A subscription already set to cancel at the end of its period arrives with `cancel_at_period_end` set, and gets `subscription.canceled` and `subscription.revoked` when that period ends.

### Orders

> **[order.created](https://polar.sh/docs/api-reference/current/order_created.md)**

> **[order.paid](https://polar.sh/docs/api-reference/current/order_paid.md)**

> **[order.updated](https://polar.sh/docs/api-reference/current/order_updated.md)**

> **[order.refunded](https://polar.sh/docs/api-reference/current/order_refunded.md)**

### Refunds

> **[refund.created](https://polar.sh/docs/api-reference/current/refund_created.md)**

> **[refund.updated](https://polar.sh/docs/api-reference/current/refund_updated.md)**

### Benefit Grants

> **[benefit\_grant.created](https://polar.sh/docs/api-reference/current/benefit_grant_created.md)**

> **[benefit\_grant.updated](https://polar.sh/docs/api-reference/current/benefit_grant_updated.md)**

> **[benefit\_grant.revoked](https://polar.sh/docs/api-reference/current/benefit_grant_revoked.md)**

## Organization Events

### Benefits

> **[benefit.created](https://polar.sh/docs/api-reference/current/benefit_created.md)**

> **[benefit.updated](https://polar.sh/docs/api-reference/current/benefit_updated.md)**

### Products

> **[product.created](https://polar.sh/docs/api-reference/current/product_created.md)**

> **[product.updated](https://polar.sh/docs/api-reference/current/product_updated.md)**

### Discounts

> **[discount.created](https://polar.sh/docs/api-reference/current/discount_created.md)**

> **[discount.updated](https://polar.sh/docs/api-reference/current/discount_updated.md)**

> **[discount.deleted](https://polar.sh/docs/api-reference/current/discount_deleted.md)**

### Organization

> **[organization.updated](https://polar.sh/docs/api-reference/current/organization_updated.md)**


This documentation is built and hosted on [Mintlify](https://mintlify.com), a developer documentation platform.
