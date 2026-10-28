> ## Documentation Index
> Fetch the complete documentation index at: https://polar.sh/docs/llms.txt
> Use this file to discover all available pages before exploring further.

# Setup Webhooks

> Get notifications asynchronously when events occur instead of having to poll for updates

Secrets generated on or after 8 September 2026, 00:00 UTC follow
[Standard Webhooks](https://www.standardwebhooks.com/). Older secrets use Polar HMAC.
See [Handle incoming webhooks](https://polar.sh/docs/integrate/webhooks/delivery.md#custom-validation).

Our SDKs offer:

* Built-in webhook signature validation
* Fully typed webhook payloads

In addition, our webhooks offer built-in support for **Slack** & **Discord**
formatting. Making it a breeze to setup in-chat notifications for your team.

## Get Started

> [!NOTE]
>
> **Use our sandbox environment during development**
>
> So you can easily test purchases, subscriptions, cancellations and refunds to
> automatically trigger webhook events without spending a dime.

**1. Add new endpoint**

Head over to [**Settings → Webhooks**](https://polar.sh/to/dashboard/settings/webhooks) and click on the `Add Endpoint` button to create a new webhook.

![](https://mintcdn.com/polar/0Af3hN6-oIM4IHT3/assets/integrate/webhooks/create.light.png?fit=max\&auto=format\&n=0Af3hN6-oIM4IHT3\&q=85\&s=23eb5510c1dbd2f511461dfe1e262485)

**2. Specify your endpoint URL**

Enter the URL to which the webhook events should be sent.

![](https://mintcdn.com/polar/0Af3hN6-oIM4IHT3/assets/integrate/webhooks/url.light.png?fit=max\&auto=format\&n=0Af3hN6-oIM4IHT3\&q=85\&s=7ef69a33df9105e42fd8ab618a30669d)

**3. Choose a delivery format**

For standard, custom integrations, leave this parameter on **Raw**. This will send a payload in JSON format.

If you wish to send notifications to a Discord or Slack channel, you can select the corresponding format here. Polar will then adapt the payload so properly formatted messages are sent to your channel.

![](https://mintcdn.com/polar/0Af3hN6-oIM4IHT3/assets/integrate/webhooks/format.light.png?fit=max\&auto=format\&n=0Af3hN6-oIM4IHT3\&q=85\&s=77c7f9e6a2f2c3bd1bede381f692908c)

If you paste a Discord or Slack Webhook URL, the format will be automatically selected.

**4. Set a secret**

We cryptographically sign the requests using this secret. So you can easily
verify them using our SDKs to ensure they are legitimate webhook payloads
from Polar.

You can set your own or generate a random one.

![](https://mintcdn.com/polar/0Af3hN6-oIM4IHT3/assets/integrate/webhooks/secret.light.png?fit=max\&auto=format\&n=0Af3hN6-oIM4IHT3\&q=85\&s=e9fa8ce33d0e86d6331813f4a37ab509)

**5. Subscribe to events**

Finally, select all the events you want to be notified about and you're done 🎉

> [!TIP]
>
> **Developing locally?**
>
> The [Polar CLI](https://polar.sh/docs/integrate/cli/webhooks.md) forwards your organization's webhooks to a URL on your machine, so you can test handlers without deploying them.
>
> ```bash Terminal theme={null}
> curl -fsSL https://polar.sh/install.sh | bash
> polar auth login --production
> polar webhook listen http://localhost:3000/api/webhooks
> ```
>
> It also sends sample events on demand with `polar webhook trigger`.

[Now, it's time to integrate our endpoint to receive events
→](https://polar.sh/docs/integrate/webhooks/delivery.md)


This documentation is built and hosted on [Mintlify](https://mintlify.com), a developer documentation platform.
