> ## Documentation Index
> Fetch the complete documentation index at: https://polar.sh/docs/llms.txt
> Use this file to discover all available pages before exploring further.

# Polar Integration in Fernand

> Learn how to sync customer and payment data from Polar to Fernand.

<img src="https://mintcdn.com/polar/fnujBPxaFvfkZfB0/assets/features/integrations/fernand/overview.png?fit=max&auto=format&n=fnujBPxaFvfkZfB0&q=85&s=b88538e65544e963bc3d58002ae490a9" width="2160" height="1236" data-path="assets/features/integrations/fernand/overview.png" />

## What is Fernand?

[Fernand](https://getfernand.com/) is a modern customer support tool designed for SaaS — it’s fast, calm, and built to reduce the anxiety of answering support requests.

## How it works

After connecting your [Polar](https://polar.sh/) account to Fernand, you’ll be able to see customer payment information and product access details directly within each customer conversation.

This enables you to:

* Instantly verify if someone is an active customer
* Prioritize conversations from high-tier plans
* View product purchases and payment history in context

***

## How to connect Fernand with Polar

<img src="https://mintcdn.com/polar/fnujBPxaFvfkZfB0/assets/features/integrations/fernand/enable.png?fit=max&auto=format&n=fnujBPxaFvfkZfB0&q=85&s=fd7b3bb222aaf0f953bb3380b6191ae3" width="2160" height="1199" data-path="assets/features/integrations/fernand/enable.png" />

1. Open [Integrations](https://app.getfernand.com/settings/organization/integrations) in your Fernand organization settings.
2. Click on **Connect Polar**.
3. You'll be redirected to Polar to authorize the connection.
4. Once approved, Fernand will begin syncing customer data automatically.

That’s it! You’ll now see Polar customer info directly in Fernand's conversation list and sidebar.

***

## How to automate your inbox with Polar data

Once Polar is connected, you can create automation rules in Fernand based on Polar data.

Let’s walk through a basic example: auto-replying to all customers on your `Pro` plan.

<img src="https://mintcdn.com/polar/fnujBPxaFvfkZfB0/assets/features/integrations/fernand/inbox.png?fit=max&auto=format&n=fnujBPxaFvfkZfB0&q=85&s=d4e253667c841e2223bd94db3b34f409" width="1280" height="659" data-path="assets/features/integrations/fernand/inbox.png" />

### Create a new rule

<Steps>
  <Step title="Create a new rule">
    1. Go to [Rules](https://app.getfernand.com/settings/organization/rules) in Fernand.
    2. Click `Add rule` and give it a descriptive name.

    <img src="https://mintcdn.com/polar/fnujBPxaFvfkZfB0/assets/features/integrations/fernand/create-rule.png?fit=max&auto=format&n=fnujBPxaFvfkZfB0&q=85&s=25780e683d3c9796dde3245b65915c04" width="2320" height="1597" data-path="assets/features/integrations/fernand/create-rule.png" />
  </Step>

  <Step title="Select a trigger">
    This ensures the rule runs on each new customer message.

    <img src="https://mintcdn.com/polar/fnujBPxaFvfkZfB0/assets/features/integrations/fernand/rule-trigger.png?fit=max&auto=format&n=fnujBPxaFvfkZfB0&q=85&s=aeff3403e656453aa466b014e60b576b" width="2320" height="1597" data-path="assets/features/integrations/fernand/rule-trigger.png" />
  </Step>

  <Step title="Select a condition">
    Now add a condition based on Polar data. For example:

    * `Contact is a customer...`
    * `Contact has paid plan...`

    You can target specific plans (e.g. `Pro`, `Business`) or specific products to personalize support or automate prioritization.

    <img src="https://mintcdn.com/polar/fnujBPxaFvfkZfB0/assets/features/integrations/fernand/rule-conditions.png?fit=max&auto=format&n=fnujBPxaFvfkZfB0&q=85&s=7ed92fcbca3e73c386624f3fadd59e0f" width="2320" height="1597" data-path="assets/features/integrations/fernand/rule-conditions.png" />
  </Step>

  <Step title="Select an action">
    Now define what happens when the rule matches. For example:

    * Send an auto reply (with variables)
    * Assign the conversation to a specific agent
    * Tag the conversation with `priority` or `paid`
    * Trigger a webhook for external automation

    <img src="https://mintcdn.com/polar/fnujBPxaFvfkZfB0/assets/features/integrations/fernand/action.png?fit=max&auto=format&n=fnujBPxaFvfkZfB0&q=85&s=5991fcfc0687d4cc116fc5e67da3b579" width="2320" height="2352" data-path="assets/features/integrations/fernand/action.png" />
  </Step>
</Steps>

### Disconnecting the integration

If you ever want to disconnect Polar from your Fernand workspace:

<Steps>
  <Step title="Go to the Integrations page." />

  <Step title="Click Disconnect next to Polar." />
</Steps>

Deleting your organization on Fernand will also remove the Polar integration automatically.
