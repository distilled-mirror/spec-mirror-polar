> ## Documentation Index
> Fetch the complete documentation index at: https://polar.sh/docs/llms.txt
> Use this file to discover all available pages before exploring further.

# Introduction

> Usage based billing using ingested events

<Info>
  Usage Based Billing is a new feature. We have a lot in store and welcome
  feedback!
</Info>

## Overview

Polar has a powerful Usage Based Billing infrastructure that allows you to charge your customers based on the usage of your application.

This is done by ingesting events from your application, creating Meters to represent that usage, and then adding metered prices to Products to charge for it.

## Concepts

### Events

Events are the core of Usage Based Billing. They represent *some* usage done by a customer in your application. Typical examples of events are:

* A customer consumed AI LLM tokens
* A customer streamed minutes of video
* A customer uploaded a file to your application

Events are sent to Polar using the [Events Ingestion API](/docs/api-reference/events/ingest-events) and are stored in our database. An event consists of the following fields:

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

A metered price is a price that is based on the usage of a meter, which is computed by filtering aggregating the events that are ingested. This is how you charge your customers for the usage of your application.

### Meter Credits benefit

You can give credits to your customers on a specific meter. This is done by creating a Meter Credits Benefit, which is a special type of benefit that allows you to give credits to your customers on a specific meter.

On a recurring product, the customer will be credited the amount of units specified in the benefit at the beginning of every subscription cycle period — monthly or yearly.

## Quickstart

Get up and running in 5 minutes

<Steps>
  <Step title="Create a Meter">
    Meters consist of filters and an aggregation function.
    The filter is used to filter the events that should be included in the meter and the aggregation function is used to compute the usage.

    <img className="block dark:hidden" src="https://mintcdn.com/polar/fnujBPxaFvfkZfB0/assets/features/usage/create-meter.light.png?fit=max&auto=format&n=fnujBPxaFvfkZfB0&q=85&s=0145ac51bc8100b038482d31612f3ea6" width="3598" height="2070" data-path="assets/features/usage/create-meter.light.png" />

    <img className="hidden dark:block" src="https://mintcdn.com/polar/fnujBPxaFvfkZfB0/assets/features/usage/create-meter.dark.png?fit=max&auto=format&n=fnujBPxaFvfkZfB0&q=85&s=2293c25c2e2c014abcf6c8a761568f95" width="3590" height="2066" data-path="assets/features/usage/create-meter.dark.png" />
  </Step>

  <Step title="Add metered price to a Product">
    To enable usage based billing for a Product, you need to add a metered price to the Product. Metered prices are only applicable to Subscription Products.

    <img className="block dark:hidden" src="https://mintcdn.com/polar/0Af3hN6-oIM4IHT3/assets/features/usage/product-meter.light.png?fit=max&auto=format&n=0Af3hN6-oIM4IHT3&q=85&s=c1a9f0493fa6b73f0d4581b19e5d757c" width="2552" height="1600" data-path="assets/features/usage/product-meter.light.png" />

    <img className="hidden dark:block" src="https://mintcdn.com/polar/0Af3hN6-oIM4IHT3/assets/features/usage/product-meter.dark.png?fit=max&auto=format&n=0Af3hN6-oIM4IHT3&q=85&s=4a54e18d784e6c6312c73dd42b02c473" width="2548" height="1596" data-path="assets/features/usage/product-meter.dark.png" />
  </Step>

  <Step title="Ingest Events">
    Now you're ready to ingest events from your application. Sending events which match the meter's filter will increment the meter's usage for the customer.

    <img className="block dark:hidden" src="https://mintcdn.com/polar/fnujBPxaFvfkZfB0/assets/features/usage/ingest.light.png?fit=max&auto=format&n=fnujBPxaFvfkZfB0&q=85&s=6428f34bbf464c2e8248138ccbcea782" width="1572" height="1104" data-path="assets/features/usage/ingest.light.png" />

    <img className="hidden dark:block" src="https://mintcdn.com/polar/fnujBPxaFvfkZfB0/assets/features/usage/ingest.dark.png?fit=max&auto=format&n=fnujBPxaFvfkZfB0&q=85&s=f8b485ff1e386a0fa79c50d428c5b958" width="1566" height="1102" data-path="assets/features/usage/ingest.dark.png" />
  </Step>

  <Step title="Customer Usage">
    Customers can view their estimated charges for each meter in the Customer Portal.

    <img className="block dark:hidden" src="https://mintcdn.com/polar/fnujBPxaFvfkZfB0/assets/features/usage/portal.light.png?fit=max&auto=format&n=fnujBPxaFvfkZfB0&q=85&s=85bdb395f87e972ed50a7381ba3aad43" width="2358" height="1218" data-path="assets/features/usage/portal.light.png" />

    <img className="hidden dark:block" src="https://mintcdn.com/polar/fnujBPxaFvfkZfB0/assets/features/usage/portal.dark.png?fit=max&auto=format&n=fnujBPxaFvfkZfB0&q=85&s=44f1b0e0f67aaa22b293a7dafaf3ab99" width="2364" height="1214" data-path="assets/features/usage/portal.dark.png" />
  </Step>
</Steps>
