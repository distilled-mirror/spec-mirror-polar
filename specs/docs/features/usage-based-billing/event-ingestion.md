> ## Documentation Index
> Fetch the complete documentation index at: https://polar.sh/docs/llms.txt
> Use this file to discover all available pages before exploring further.

# Event Ingestion

> Ingest events from your application

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

## Ingest events using the Polar SDK

To ingest events, you can use the Polar SDKs.

### TypeScript Example

```typescript theme={null}
import { Polar } from "@polar-sh/sdk";

const polar = new Polar({
  accessToken: process.env["POLAR_ACCESS_TOKEN"] ?? "",
});

await polar.events.ingest({
  events: [
    {
      name: "<value>",
      externalCustomerId: "<id>",
      metadata: {
        key: "value",
      },
    },
  ],
});
```

<Note>
  You are always responsible for checking the balance of your customers' Usage
  Meter. As events always are ingested, we will never prohibit any customer's
  action based on their Usage Meter balance.
</Note>

## Ingestion Strategies

To make it easier to ingest events, we have created a set of ingestion strategies for common event sources.

Learn more about our [Ingestion Strategies](/docs/features/usage-based-billing/ingestion-strategies).

## Good to know

### Events are immutable

Once an event is ingested, it cannot be changed, nor can it be deleted.

### Backdated events

You can set the `timestamp` field on an event to any point in the past — useful for batched ingestion or replaying from your own queue. The timestamp does not need to be part of the current billing cycle.

Polar attributes events to billing periods based on **when they were received by Polar**, not the supplied `timestamp`. An event ingested today with a `timestamp` from a previous billing period is included on the **current cycle** — it is not added to the already-closed invoice for the period it logically belongs to. Polar never issues retroactive invoices or credits for late events.

The supplied `timestamp` is still used to:

* Place events on the time series shown in the dashboard and the Customer Portal.
* Set the date range displayed on the metered line item of the next invoice.
