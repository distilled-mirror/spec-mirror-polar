> ## Documentation Index
> Fetch the complete documentation index at: https://polar.sh/docs/llms.txt
> Use this file to discover all available pages before exploring further.

# Polar for Zapier

> Connect Polar to hundreds of other apps with Zapier

export const ZapierEmbed = () => {
  if (typeof document === "undefined") {
    return null;
  } else {
    setTimeout(() => {
      const script = document.createElement("script");
      script.type = "module";
      script.src = "https://cdn.zapier.com/packages/partner-sdk/v0/zapier-elements/zapier-elements.esm.js";
      document.head.appendChild(script);
      const stylesheet = document.createElement("link");
      stylesheet.rel = "stylesheet";
      stylesheet.href = "https://cdn.zapier.com/packages/partner-sdk/v0/zapier-elements/zapier-elements.css";
      document.head.appendChild(stylesheet);
      const element = document.createElement("zapier-workflow");
      element.clientId = "Zci4gpfx7Co47mBoFOYm0m8bmnzB5UPcw7eGhpSR";
      element.theme = document.querySelector("html").classList.contains("dark") ? "dark" : "light";
      element.introCopyDisplay = "hide";
      element.manageZapsDisplay = "hide";
      element.guessZapDisplay = "hide";
      const container = document.querySelector("#zapier-container") || document.body;
      container.appendChild(element);
    }, 1);
    return <div id="zapier-container"></div>;
  }
};

[Zapier](https://zapier.com/apps/polar/integrations) lets you connect Polar to 2,000+ other web services. Automated connections called Zaps, set up in minutes with no coding, can automate your day-to-day tasks and build workflows between apps that otherwise wouldn't be possible.

Each Zap has one app as the **Trigger**, where your information comes from and which causes one or more **Actions** in other apps, where your data gets sent automatically.

<Note>
  We've focused on **triggers** (webhooks) for now, so you can react to events in Polar and trigger actions in other apps.

  Need to perform actions in Polar? Tell us about your use case [here](https://github.com/orgs/polarsource/discussions/new?category=integrations\&labels=integrations%2Fzapier) and we'll consider adding more actions in the future.
</Note>

## Getting Started with Zapier

Sign up for a free [Zapier](https://zapier.com/apps/polar/integrations) account, from there you can jump right in. To help you hit the ground running, you'll find popular pre-made Zaps below.

## How do I connect Polar to Zapier?

Log in to your [Zapier account](https://zapier.com/sign-up) or create a new account.
Navigate to "My Apps" from the top menu bar.
Now click on "Connect a new account..." and search for "Polar"
Use your credentials to connect your Polar account to Zapier.
Once that's done you can start creating an automation! Use a pre-made Zap or create your own with the Zap Editor. Creating a Zap requires no coding knowledge and you'll be walked step-by-step through the setup.
Need inspiration? See everything that's possible with [Polar and Zapier](https://zapier.com/apps/Polar/integrations).

If you have any additional questions, you can open a ticket with Zapier Support from [https://zapier.com/app/get-help](https://zapier.com/app/get-help)

## Popular use cases

<ZapierEmbed />
