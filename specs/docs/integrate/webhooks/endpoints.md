> ## Documentation Index
> Fetch the complete documentation index at: https://polar.sh/docs/llms.txt
> Use this file to discover all available pages before exploring further.

# Setup Webhooks

> Get notifications asynchronously when events occur instead of having to poll for updates

Secrets generated on or after 8 September 2026, 00:00 UTC follow
[Standard Webhooks](https://www.standardwebhooks.com/). Older secrets use Polar HMAC.
See [Handle incoming webhooks](/docs/integrate/webhooks/delivery#custom-validation).

Our SDKs offer:

* Built-in webhook signature validation
* Fully typed webhook payloads

In addition, our webhooks offer built-in support for **Slack** & **Discord**
formatting. Making it a breeze to setup in-chat notifications for your team.

## Get Started

<Info>
  **Use our sandbox environment during development**

  So you can easily test purchases, subscriptions, cancellations and refunds to
  automatically trigger webhook events without spending a dime.
</Info>

<Steps>
  <Step title="Add new endpoint">
    Head over to your organization settings and click on the `Add Endpoint` button to create a new webhook.

    <img className="block dark:hidden" src="https://mintcdn.com/polar/0Af3hN6-oIM4IHT3/assets/integrate/webhooks/create.light.png?fit=max&auto=format&n=0Af3hN6-oIM4IHT3&q=85&s=23eb5510c1dbd2f511461dfe1e262485" width="1532" height="389" data-path="assets/integrate/webhooks/create.light.png" />

    <img className="hidden dark:block" src="https://mintcdn.com/polar/0Af3hN6-oIM4IHT3/assets/integrate/webhooks/create.dark.png?fit=max&auto=format&n=0Af3hN6-oIM4IHT3&q=85&s=c86d0770b5dc4d42279d9f1da568edb8" width="1494" height="388" data-path="assets/integrate/webhooks/create.dark.png" />
  </Step>

  <Step title="Specify your endpoint URL">
    Enter the URL to which the webhook events should be sent.

    <img className="block dark:hidden" src="https://mintcdn.com/polar/0Af3hN6-oIM4IHT3/assets/integrate/webhooks/url.light.png?fit=max&auto=format&n=0Af3hN6-oIM4IHT3&q=85&s=7ef69a33df9105e42fd8ab618a30669d" width="1075" height="402" data-path="assets/integrate/webhooks/url.light.png" />

    <img className="hidden dark:block" src="https://mintcdn.com/polar/0Af3hN6-oIM4IHT3/assets/integrate/webhooks/url.dark.png?fit=max&auto=format&n=0Af3hN6-oIM4IHT3&q=85&s=6a4514f5bd12fcf01ab1aa1bc2b1f26a" width="1068" height="398" data-path="assets/integrate/webhooks/url.dark.png" />
  </Step>

  <Step title="Choose a delivery format">
    For standard, custom integrations, leave this parameter on **Raw**. This will send a payload in JSON format.

    If you wish to send notifications to a Discord or Slack channel, you can select the corresponding format here. Polar will then adapt the payload so properly formatted messages are sent to your channel.

    <img className="block dark:hidden" src="https://mintcdn.com/polar/0Af3hN6-oIM4IHT3/assets/integrate/webhooks/format.light.png?fit=max&auto=format&n=0Af3hN6-oIM4IHT3&q=85&s=77c7f9e6a2f2c3bd1bede381f692908c" width="1034" height="402" data-path="assets/integrate/webhooks/format.light.png" />

    <img className="hidden dark:block" src="https://mintcdn.com/polar/0Af3hN6-oIM4IHT3/assets/integrate/webhooks/format.dark.png?fit=max&auto=format&n=0Af3hN6-oIM4IHT3&q=85&s=896fc481fe7c61892b034380247479ae" width="1050" height="418" data-path="assets/integrate/webhooks/format.dark.png" />

    If you paste a Discord or Slack Webhook URL, the format will be automatically selected.
  </Step>

  <Step title="Set a secret">
    We cryptographically sign the requests using this secret. So you can easily
    verify them using our SDKs to ensure they are legitimate webhook payloads
    from Polar.

    You can set your own or generate a random one.

    <img className="block dark:hidden" src="https://mintcdn.com/polar/0Af3hN6-oIM4IHT3/assets/integrate/webhooks/secret.light.png?fit=max&auto=format&n=0Af3hN6-oIM4IHT3&q=85&s=e9fa8ce33d0e86d6331813f4a37ab509" width="1074" height="331" data-path="assets/integrate/webhooks/secret.light.png" />

    <img className="hidden dark:block" src="https://mintcdn.com/polar/0Af3hN6-oIM4IHT3/assets/integrate/webhooks/secret.dark.png?fit=max&auto=format&n=0Af3hN6-oIM4IHT3&q=85&s=d447062a9726d8d9116aef2984a96cc2" width="1072" height="332" data-path="assets/integrate/webhooks/secret.dark.png" />
  </Step>

  <Step title="Subscribe to events">
    Finally, select all the events you want to be notified about and you're done 🎉
  </Step>
</Steps>

<Tip>
  **Developing locally?**

  Install Polar CLI to use the listening command. This will allow you to test your webhook handlers without deploying them to a live server.

  Install the Polar CLI

  ```bash Terminal theme={null}
  curl -fsSL https://polar.sh/install.sh | bash
  ```

  Once you have installed the Polar CLI, you can easily start a tunnel:

  ```bash Terminal theme={null}
  polar listen http://localhost:3000/
  ```

  This will relay webhooks automatically to the speicified URL.

  ```bash theme={null}
  ✔ Select Organization …  My Organization

    Connected  My Organization
    Secret     6t3c8ce2247c493a3ade20uea4484d64
    Forwarding http://localhost:3000

    Waiting for events...
  ```
</Tip>

[Now, it's time to integrate our endpoint to receive events
→](/docs/integrate/webhooks/delivery)
