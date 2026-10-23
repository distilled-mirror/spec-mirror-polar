> ## Documentation Index
> Fetch the complete documentation index at: https://polar.sh/docs/llms.txt
> Use this file to discover all available pages before exploring further.

# Customer State

> The quickest way to integrate billing in your application

Customer State is a concept allowing you to query for the current state of a customer, including their active subscriptions and granted [benefits](/docs/features/benefits/introduction.md), in a single [API call](/docs/api-reference/current/customers/get-customer-state-by-external-id.md) or single [webhook event](/docs/api-reference/current/customer_state_changed.md).

Combined with the [External ID](/docs/features/customer-management.md#external-id) feature, you can get up-and-running in minutes.

## The customer state object

The customer state object contains:

* All the data about the customer.
* The list of their **active** subscriptions.
* The list of their **granted** benefits.
* The list of their **active** meters, with their current balance.

Thus, with that single object, you have all the required information to check if you should provision access to your service or not.

> **[Get Customer State by External ID](/docs/api-reference/current/customers/get-customer-state-by-external-id.md)**
>
> One endpoint to rule them all, using your own customer ID.

> **[Get Customer State](/docs/api-reference/current/customers/get-customer-state.md)**
>
> The same one, but with internal Polar customer ID.

## The `customer.state_changed` webhook

To be notified of the customer state changes, you can listen to the `customer.state_changed` webhook event. It's triggered when:

* Customer is created, updated or deleted.
* A subscription is created or updated.
* A benefit is granted or revoked.

By subscribing to this webhook event, you keep your system up-to-date and update your customer's access accordingly.

> **[customer.state\_changed](/docs/api-reference/current/customer_state_changed.md)**
>
> One webhook to rule them all.


This documentation is built and hosted on [Mintlify](https://mintlify.com), a developer documentation platform.
