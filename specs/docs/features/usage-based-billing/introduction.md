> ## Documentation Index
> Fetch the complete documentation index at: https://polar.sh/docs/llms.txt
> Use this file to discover all available pages before exploring further.

# Metering

> Track customer usage with ingested events and meters

## Overview

Metering lets you track how your customers use your application. You ingest events from your application, and Meters aggregate those events into usage per customer.

You can then use that usage to:

* Charge customers for it with [metered prices](https://polar.sh/docs/features/usage-based-billing/billing.md)
* Grant usage [credits](https://polar.sh/docs/features/usage-based-billing/credits.md) that are consumed as usage comes in
* Show customers their usage and estimated charges in the [Customer Portal](https://polar.sh/docs/features/customer-portal/introduction.md)

The same events can also carry cost data, which [Cost Insights](https://polar.sh/docs/features/cost-insights/introduction.md) uses to track what each customer costs you.

## Concepts

### Events

Events are the core of Usage Based Billing. They represent *some* usage done by a customer in your application. Typical examples of events are:

* A customer consumed AI LLM tokens
* A customer streamed minutes of video
* A customer uploaded a file to your application

Events are sent to Polar using the [Events Ingestion API](https://polar.sh/docs/api-reference/current/events/ingest-events.md) and are stored in our database. An event consists of the following fields:

* A `name`, which is a string that can be used to identify the type of event. For example, `ai_usage`, `video_streamed` or `file_uploaded`.
* A `customer_id` or `external_customer_id`, which is Polar's customer ID or your user's ID. This is used to identify the customer that triggered the event.
* A `metadata` object, which is a JSON object that can contain any additional information about the event. This is useful for storing information that can be used to filter the events or compute the actual usage. For example, you can store the duration of the video streamed or the size of the file uploaded.

Here is an example of an event:

```json theme={null}
{
  "name": "ai_usage",
  "external_customer_id": "cus_123",
  "metadata": {
    "model": "gpt-4.1-nano",
    "requests": 1,
    "total_tokens": 77,
    "request_tokens": 58,
    "response_tokens": 19
  }
}
```

### Meters

Meters are there to filter and aggregate the events that are ingested. Said another way, this is how you define what usage you want to charge for, based on the events you send to Polar. For example:

* AI usage meter, which filters the events with the name `ai_usage` and sums the `total_tokens` field.
* Video streaming meter, which filters the events with the name `video_streamed` and sums the `duration` field.
* File upload meter, which filters the events with the name `file_uploaded` and sums the `size` field.

You can create and manage your meters from the dashboard. Polar is then able to compute the usage over time, both globally and per customer.

### Metered Price

To charge customers for their usage, add a metered price based on a meter to a Product. See [Usage-Based Pricing](https://polar.sh/docs/features/usage-based-billing/billing.md).

### Meter Credits benefit

You can give credits to your customers on a specific meter. This is done by creating a Meter Credits Benefit, which is a special type of benefit that allows you to give credits to your customers on a specific meter.

On a recurring product, the customer will be credited the amount of units specified in the benefit at the beginning of every subscription cycle period — monthly or yearly.

## Quickstart

Get up and running in 5 minutes

**1. Create a Meter**

Meters consist of filters and an aggregation function.
The filter is used to filter the events that should be included in the meter and the aggregation function is used to compute the usage.

![](https://mintcdn.com/polar/fnujBPxaFvfkZfB0/assets/features/usage/create-meter.light.png?fit=max\&auto=format\&n=fnujBPxaFvfkZfB0\&q=85\&s=0145ac51bc8100b038482d31612f3ea6)

**2. Add metered price to a Product**

Only needed if you want to charge for usage. Add a metered price to the Product to bill customers based on the meter. Metered prices are only applicable to Subscription Products.

![](https://mintcdn.com/polar/0Af3hN6-oIM4IHT3/assets/features/usage/product-meter.light.png?fit=max\&auto=format\&n=0Af3hN6-oIM4IHT3\&q=85\&s=c1a9f0493fa6b73f0d4581b19e5d757c)

**3. Ingest Events**

Now you're ready to ingest events from your application. Sending events which match the meter's filter will increment the meter's usage for the customer.

![](https://mintcdn.com/polar/fnujBPxaFvfkZfB0/assets/features/usage/ingest.light.png?fit=max\&auto=format\&n=fnujBPxaFvfkZfB0\&q=85\&s=6428f34bbf464c2e8248138ccbcea782)

**4. Customer Usage**

Customers can view their estimated charges for each meter in the Customer Portal.

![](https://mintcdn.com/polar/fnujBPxaFvfkZfB0/assets/features/usage/portal.light.png?fit=max\&auto=format\&n=fnujBPxaFvfkZfB0\&q=85\&s=85bdb395f87e972ed50a7381ba3aad43)


This documentation is built and hosted on [Mintlify](https://mintlify.com), a developer documentation platform.
