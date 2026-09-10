> ## Documentation Index
> Fetch the complete documentation index at: https://polar.sh/docs/llms.txt
> Use this file to discover all available pages before exploring further.

# Customer Management

> Get insights on your customers and sales

<img className="block dark:hidden" src="https://mintcdn.com/polar/Ut0vPUvE1pIdMcH2/assets/features/customer-management/details.light.png?fit=max&auto=format&n=Ut0vPUvE1pIdMcH2&q=85&s=dc8cb8a0793331ec889f89f3b84d33ac" width="3586" height="2058" data-path="assets/features/customer-management/details.light.png" />

<img className="hidden dark:block" src="https://mintcdn.com/polar/Ut0vPUvE1pIdMcH2/assets/features/customer-management/details.dark.png?fit=max&auto=format&n=Ut0vPUvE1pIdMcH2&q=85&s=53377b04b7d065e66102c0a2141118fb" width="5110" height="2642" data-path="assets/features/customer-management/details.dark.png" />

## Managing Customers

Polar has a built in feature to view and manage your Customers.

Everyone who has ever purchased something from you will be recorded as a Customer to your Organization. You’re able to see past orders and their ongoing subscriptions, as well as some additional metrics.

## External ID

Quite often, you'll have our own users management system in your application, where your customer already have an ID. To ease reconciliation between Polar and your system, we have a dedicated [`external_id`](/docs/api-reference/customers/get-customer-by-external-id#response-external-id) field on Customers. It's unique across your organization and can't be changed once set.

We have dedicated API endpoints that work with the `external_id` field, so you don't even have to store the internal Polar ID in your system.

<Card title="Get Customer by External ID" icon="link" href="/docs/api-reference/customers/get-customer-by-external-id" horizontal />

<Card title="Update Customer by External ID" icon="link" href="/docs/api-reference/customers/update-customer-by-external-id" horizontal />

<Card title="Delete Customer by External ID" icon="link" href="/docs/api-reference/customers/delete-customer-by-external-id" horizontal />

## Metadata

You may set additional metadata on Customers. This can be very useful to store additional data about your customer you want to be available through our API and webhooks.

<img className="block dark:hidden" src="https://mintcdn.com/polar/Ut0vPUvE1pIdMcH2/assets/features/customer-management/edit.light.png?fit=max&auto=format&n=Ut0vPUvE1pIdMcH2&q=85&s=00c8cdfc037e19bfe63045c1d9d9459e" width="1498" height="960" data-path="assets/features/customer-management/edit.light.png" />

<img className="hidden dark:block" src="https://mintcdn.com/polar/Ut0vPUvE1pIdMcH2/assets/features/customer-management/edit.dark.png?fit=max&auto=format&n=Ut0vPUvE1pIdMcH2&q=85&s=1981f637bea304c95d62ed7b7d85d14f" width="1512" height="964" data-path="assets/features/customer-management/edit.dark.png" />

It can be set through the dashboard or through the [API](/docs/api-reference/customers/update-customer#body-metadata). It can also be pre-set when creating a Checkout Session by using the [`customer_metadata`](/docs/api-reference/checkouts/create-checkout-session#body-customer-metadata) field. This way, after a successful checkout, the metadata will automatically be set on the newly created Customer.
