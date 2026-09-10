> ## Documentation Index
> Fetch the complete documentation index at: https://polar.sh/docs/llms.txt
> Use this file to discover all available pages before exploring further.

# Meters

> Creating and managing meters for Usage Based Billing

Meters are there to filter and aggregate the events that are ingested. Said another way, this is how you define what usage you want to charge for, based on the events you send to Polar. For example:

* AI usage meter, which filters the events with the name `ai_usage` and sums the `total_tokens` field.
* Video streaming meter, which filters the events with the name `video_streamed` and sums the `duration` field.
* File upload meter, which filters the events with the name `file_uploaded` and sums the `size` field.

You can create and manage your meters from the dashboard. Polar is then able to compute the usage over time, both globally and per customer.

## Creating a Meter

To create a meter, [open the Meters page](https://polar.sh/to/dashboard/products/meters) and click the "Create Meter" button.

<img className="block dark:hidden" src="https://mintcdn.com/polar/fnujBPxaFvfkZfB0/assets/features/usage/create-meter.light.png?fit=max&auto=format&n=fnujBPxaFvfkZfB0&q=85&s=0145ac51bc8100b038482d31612f3ea6" width="3598" height="2070" data-path="assets/features/usage/create-meter.light.png" />

<img className="hidden dark:block" src="https://mintcdn.com/polar/fnujBPxaFvfkZfB0/assets/features/usage/create-meter.dark.png?fit=max&auto=format&n=fnujBPxaFvfkZfB0&q=85&s=2293c25c2e2c014abcf6c8a761568f95" width="3590" height="2066" data-path="assets/features/usage/create-meter.dark.png" />

## Filters

A filter is a set of clauses that are combined using conjunctions. They're used to filter events that you've ingested into Polar.

<img className="block dark:hidden" src="https://mintcdn.com/polar/fnujBPxaFvfkZfB0/assets/features/usage/filter.light.png?fit=max&auto=format&n=fnujBPxaFvfkZfB0&q=85&s=77aca67ac7630a38fefa9a6d567a58f3" width="1274" height="922" data-path="assets/features/usage/filter.light.png" />

<img className="hidden dark:block" src="https://mintcdn.com/polar/fnujBPxaFvfkZfB0/assets/features/usage/filter.dark.png?fit=max&auto=format&n=fnujBPxaFvfkZfB0&q=85&s=6c855b483334dba4b7f26e54c2878080" width="1276" height="914" data-path="assets/features/usage/filter.dark.png" />

### Clauses

A clause is a condition that an event must meet to be included in the meter.

#### Property

Properties are the properties of the event that you want to filter on.

If you want to match on a metadata field, you can use the metadata key directly. No need to include a `metadata.` prefix.

#### Operator

Operators are the operators that you want to use to filter the events.

* **Equals**
* **Not equals**
* **Greater Than**
* **Greater Than or Equals**
* **Less Than**
* **Less Than or Equals**
* **Contains**
* **Does Not Contain**

#### Value

Values are automatically parsed in the filter builder. They're parsed in the following order:

1. Number — Tries to parse the value as number
2. Boolean — Checks if value is "true" or "false"
3. String — Treats value as string as fallback

### Conjunctions

A conjunction is a logical operator that combines two or more clauses.

* **and** — All clauses must be true for the event to be included.
* **or** — At least one clause must be true for the event to be included.

## Aggregation

The aggregation is the function that is used to aggregate the events that match the filter.

For example, if you want to count the number of events that match the filter, you can use the **Count** aggregation. If you want to sum the value of a metadata field, you can use the **Sum** aggregation.

* **Count** — Counts the number of events that match the filter.
* **Sum** — Sums the value of a property.
* **Average** — Computes the average value of a property.
* **Minimum** — Computes the minimum value of a property.
* **Maximum** — Computes the maximum value of a property.
* **Unique** — Counts the number of unique values of a property.

<AccordionGroup>
  <Accordion title="Example">
    Consider the following events:

    ```json theme={null}
    [
      {
        "name": "ai_usage",
        "external_customer_id": "cus_123",
        "metadata": {
          "total_tokens": 10
        }
      },
      {
        "name": "ai_usage",
        "external_customer_id": "cus_123",
        "metadata": {
          "total_tokens": 20
        }
      },
      {
        "name": "ai_usage",
        "external_customer_id": "cus_123",
        "metadata": {
          "total_tokens": 30
        }
      },
      {
        "name": "ai_usage",
        "external_customer_id": "cus_123",
        "metadata": {
          "total_tokens": 30
        }
      }
    ]
    ```

    Here is the result of each aggregation function, over the `total_tokens` metadata property:

    * **Count**: 4 units
    * **Sum**: 90 units
    * **Average**: 22.5 units
    * **Minimum**: 10 units
    * **Maximum**: 30 units
    * **Unique**: 3 units
  </Accordion>
</AccordionGroup>

If you want to use a metadata property in the aggregation, you can use the metadata property directly. No need to include a `metadata.` prefix.

## Unit

The unit controls how prices for this meter are **formatted and displayed** to customers — on invoices, in the customer portal, and in your checkout. It does not affect billing calculation; it is purely presentational.

| Unit   | Display format           | Best for                                  |
| ------ | ------------------------ | ----------------------------------------- |
| Scalar | \$0.05 / unit            | Generic counts (API calls, events, seats) |
| Token  | \$20.00 / 1M tokens      | LLM token consumption                     |
| Custom | Configurable (see below) | Any unit not covered above                |

### Custom unit

Select **Custom** to define your own display format. Two additional fields appear:

* **Unit label** — The singular name shown after the price, e.g. `gigabyte` displays as `$0.023 / gigabyte`.
* **Unit multiplier** — Scales the displayed price so you can show a more readable denomination. For example, a multiplier of `1000` shows the price per 1 000 units rather than per single unit.

<Tip>
  The unit multiplier only affects how the price is shown. The raw `unit_amount`
  you set is still the price per single event unit — the multiplier scales the
  display amount for readability.
</Tip>

## Example

The following Meter Filter & Aggregation will match events that have the name `openai-usage` and sum units over metadata property `completionTokens`.

<img className="block dark:hidden" src="https://mintcdn.com/polar/fnujBPxaFvfkZfB0/assets/features/usage/meter.light.png?fit=max&auto=format&n=fnujBPxaFvfkZfB0&q=85&s=2c4ba244ceee17f9bda8665f6a22a6b6" width="1108" height="936" data-path="assets/features/usage/meter.light.png" />

<img className="hidden dark:block" src="https://mintcdn.com/polar/fnujBPxaFvfkZfB0/assets/features/usage/meter.dark.png?fit=max&auto=format&n=fnujBPxaFvfkZfB0&q=85&s=ef46a2a0420e3e01d7a181d3e7ddf118" width="1116" height="928" data-path="assets/features/usage/meter.dark.png" />

<Tip>
  You can **Preview** the events matched by the meter while creating it.
</Tip>

## Good to know

A few things to keep in mind when creating and managing meters:

### Updating a Meter

You may update a meter's filters or aggregation function as long as the meter doesn't have any processed events or does not have any customer purchase associated with it.
