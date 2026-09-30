> ## Documentation Index
> Fetch the complete documentation index at: https://polar.sh/docs/llms.txt
> Use this file to discover all available pages before exploring further.

# TanStack Start

> Payments and Checkouts made dead simple with TanStack Start

## Examples

* [With TanStack Start](https://github.com/polarsource/examples/tree/main/with-tanstack-start)

## Installation

Install the required Polar packages using the following command:

<Tabs>
  <Tab title="npm">
    ```bash Terminal theme={null}
    npm install @polar-sh/tanstack-start
    ```
  </Tab>

  <Tab title="yarn">
    ```bash Terminal theme={null}
    yarn add @polar-sh/tanstack-start
    ```
  </Tab>

  <Tab title="pnpm">
    ```bash Terminal theme={null}
    pnpm add @polar-sh/tanstack-start
    ```
  </Tab>

  <Tab title="bun">
    ```bash Terminal theme={null}
    bun add @polar-sh/tanstack-start
    ```
  </Tab>
</Tabs>

## Checkout

Create a Checkout handler which takes care of redirections.

```typescript icon="square-js" routes/api/checkout.ts theme={null}
import { Checkout } from "@polar-sh/tanstack-start";
import { createFileRoute } from "@tanstack/react-router";

export const Route = createFileRoute("/api/checkout")({
  server: {
    handlers: {
      GET: Checkout({
        accessToken: process.env.POLAR_ACCESS_TOKEN!,
        successUrl: process.env.SUCCESS_URL,
        returnUrl: "https://myapp.com", // An optional URL which renders a back-button in the Checkout
        environment: "sandbox", // Use sandbox if you're testing Polar - omit the parameter or pass 'production' otherwise
        theme: "dark", // Enforces the theme - System-preferred theme will be set if left omitted
      }),
    },
  },
});
```

`successUrl` and `returnUrl` must be absolute URLs. The handler appends `checkout_id={CHECKOUT_ID}` to `successUrl`; pass `includeCheckoutId: false` to turn this off.

### Query Params

Pass query params to this route.

* products `?products=123` - Repeat the parameter for multiple products: `?products=123&products=456`
* customer\_id (optional) `?products=123&customer_id=xxx`
* external\_customer\_id (optional) `?products=123&external_customer_id=xxx`
* customer\_email (optional) `?products=123&customer_email=janedoe@gmail.com`
* customer\_name (optional) `?products=123&customer_name=Jane`
* customer\_billing\_address (optional) `URL-Encoded JSON string`
* customer\_tax\_id (optional) `?products=123&customer_tax_id=xxx`
* customer\_ip\_address (optional) `?products=123&customer_ip_address=xxx`
* customer\_metadata (optional) `URL-Encoded JSON string`
* allow\_discount\_codes (optional) `?products=123&allow_discount_codes=false`
* discount\_id (optional) `?products=123&discount_id=xxx`
* discount\_code (optional) `?products=123&discount_code=SAVE20` - Applied before redirecting. `discount_id` takes precedence when both are supplied.
* seats (optional) `?products=123&seats=5` - Number of seats for seat-based products
* metadata (optional) `URL-Encoded JSON string`

The handler returns `400` when `products` is missing and `500` when the checkout can't be created.

## Customer Portal

Create a customer portal where your customer can view orders and subscriptions.

```typescript icon="square-js" routes/api/portal.ts theme={null}
import { CustomerPortal } from "@polar-sh/tanstack-start";
import { createFileRoute } from "@tanstack/react-router";

export const Route = createFileRoute("/api/portal")({
  server: {
    handlers: {
      GET: CustomerPortal({
        accessToken: process.env.POLAR_ACCESS_TOKEN!,
        getCustomerId: async (request: Request) => "", // Function to resolve a Polar Customer ID
        returnUrl: "https://myapp.com", // An optional URL which renders a back-button in the Customer Portal
        environment: "sandbox", // Use sandbox if you're testing Polar - omit the parameter or pass 'production' otherwise
      }),
    },
  },
});
```

`getCustomerId` must resolve to a Polar customer ID. The handler returns `400` when it resolves to an empty value.

## Webhooks

A simple utility which verifies the signature of incoming webhook payloads with your webhook secret.

```typescript icon="square-js" routes/api/webhook/polar.ts theme={null}
import { Webhooks } from "@polar-sh/tanstack-start";
import { createFileRoute } from "@tanstack/react-router";

export const Route = createFileRoute("/api/webhook/polar")({
  server: {
    handlers: {
      POST: Webhooks({
        webhookSecret: process.env.POLAR_WEBHOOK_SECRET!,
        onPayload: async (payload) => {
          // Handle the payload
          // No need to return an acknowledge response
        },
      }),
    },
  },
});
```

### Payload Handlers

The Webhook handler also supports granular handlers for easy integration.

Every handler is an `async` function that receives the full webhook payload (`{ type, timestamp, data }`). Fields use the SDK's snake\_case names, for example `payload.data.customer_id`.

* `onPayload` - Called for every incoming webhook event, in addition to the matching handler below
* `onCheckoutCreated` - Triggered when a checkout is created
* `onCheckoutExpired` - Triggered when a checkout expires
* `onCheckoutUpdated` - Triggered when a checkout is updated
* `onOrderCreated` - Triggered when an order is created
* `onOrderUpdated` - Triggered when an order is updated
* `onOrderPaid` - Triggered when an order is paid
* `onOrderRefunded` - Triggered when an order is refunded
* `onRefundCreated` - Triggered when a refund is created
* `onRefundUpdated` - Triggered when a refund is updated
* `onSubscriptionCreated` - Triggered when a subscription is created
* `onSubscriptionUpdated` - Triggered when a subscription is updated
* `onSubscriptionActive` - Triggered when a subscription becomes active
* `onSubscriptionCanceled` - Triggered when a subscription is canceled
* `onSubscriptionCycled` - Triggered when a subscription enters a new billing period
* `onSubscriptionPastDue` - Triggered when a subscription payment fails and it becomes past due
* `onSubscriptionPaused` - Triggered when a subscription is paused
* `onSubscriptionResumed` - Triggered when a paused subscription is resumed
* `onSubscriptionRevoked` - Triggered when a subscription is revoked
* `onSubscriptionUncanceled` - Triggered when a subscription cancellation is reversed
* `onProductCreated` - Triggered when a product is created
* `onProductUpdated` - Triggered when a product is updated
* `onOrganizationUpdated` - Triggered when an organization is updated
* `onBenefitCreated` - Triggered when a benefit is created
* `onBenefitUpdated` - Triggered when a benefit is updated
* `onBenefitGrantCreated` - Triggered when a benefit grant is created
* `onBenefitGrantCycled` - Triggered when a benefit grant renews with its subscription
* `onBenefitGrantUpdated` - Triggered when a benefit grant is updated
* `onBenefitGrantRevoked` - Triggered when a benefit grant is revoked
* `onCustomerCreated` - Triggered when a customer is created
* `onCustomerUpdated` - Triggered when a customer is updated
* `onCustomerDeleted` - Triggered when a customer is deleted
* `onCustomerStateChanged` - Triggered when a customer state changes
* `onCustomerSeatAssigned` - Triggered when a seat is assigned to a customer
* `onCustomerSeatClaimed` - Triggered when a customer claims an assigned seat
* `onCustomerSeatRevoked` - Triggered when a seat is revoked from a customer
* `onDiscountCreated` - Triggered when a discount is created
* `onDiscountUpdated` - Triggered when a discount is updated
* `onDiscountDeleted` - Triggered when a discount is deleted
* `onMemberCreated` - Triggered when a member is added to a team customer
* `onMemberUpdated` - Triggered when a member of a team customer is updated
* `onMemberDeleted` - Triggered when a member is removed from a team customer

The handler verifies the `webhook-id`, `webhook-timestamp` and `webhook-signature` headers against your webhook secret before calling any handler:

* A missing or invalid signature returns `403`.
* A malformed payload returns `400`.
* A signed event type that the installed SDK doesn't know yet returns `200` and is ignored, so new event types don't cause retries.
* If a handler throws, the error propagates and the request fails, so Polar retries the delivery.
