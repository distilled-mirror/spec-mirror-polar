> ## Documentation Index
> Fetch the complete documentation index at: https://polar.sh/docs/llms.txt
> Use this file to discover all available pages before exploring further.

# Customer Portal Settings

> Configure what customers can do from the Customer Portal

All Customer Portal settings live under [**Settings → Customer Portal**](https://polar.sh/to/dashboard/settings/customer-portal) in your Polar dashboard. Each setting is a toggle that enables or disables a specific capability for your customers.

## Show metered usage

Adds a **Usage** tab to the Customer Portal where customers can see their current consumption for each meter on their subscription.

This is only relevant if you've configured [meters](https://polar.sh/docs/features/usage-based-billing/meters.md) on your products. Disabling this toggle hides usage from the portal UI but has no effect on the Customer Portal API — if you're building a custom usage surface in your own app, the [customer meters endpoints](https://polar.sh/docs/api-reference/current/customer_meters/list-customer-meters.md) continue to work regardless.

## Enable subscription seat management

Allows customers to change the number of seats on their active subscription, and to assign or revoke seats for their teammates, directly from the portal.

This setting is only relevant when you offer [seat-based prices](https://polar.sh/docs/features/seat-based-pricing.md). If you disable it, customers can't change seat counts or manage assignments from the portal — you'll need to build that flow yourself against the [Customer Seats API](https://polar.sh/docs/api-reference/current/customer-seats/assign-seat.md).

## Allow email address changes

Gives customers the option to change the email address associated with their customer record after purchasing. The new email must be verified through a confirmation link before the change takes effect.

> [!NOTE]
>
> When this is enabled, you'll probably want to listen for the
> [`customer.updated`](https://polar.sh/docs/api-reference/current/customer_updated) webhook and
> sync the new email back into your own user system, so the two stay in sync.

## Enable subscription plan changes

Lets customers switch between products on their own — upgrading, downgrading, or moving to a different plan entirely — without contacting you.

Plan changes follow your [proration settings](https://polar.sh/docs/features/subscriptions/proration.md). If you'd rather handle plan changes yourself (for example, because you gate them behind custom logic in your app), disable this and drive updates through the [Update Subscription](https://polar.sh/docs/api-reference/current/subscriptions/update-subscription.md) endpoint instead.

## Enable subscription pause

Lets customers pause their own subscription from the portal and resume it later, without contacting you.

A paused subscription stops billing at the end of the current period and revokes benefits until it resumes; resuming starts a new billing period and charges immediately. See [Pause and resume](https://polar.sh/docs/features/subscriptions/manage.md#pause-and-resume) for the full behavior.

This toggle only gates the portal. Pausing and resuming through the [Update Subscription](https://polar.sh/docs/api-reference/current/subscriptions/update-subscription.md) endpoint is always available to you, so you can drive it from your own backend whether or not customers can self-serve.


This documentation is built and hosted on [Mintlify](https://mintlify.com), a developer documentation platform.
