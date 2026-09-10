> ## Documentation Index
> Fetch the complete documentation index at: https://polar.sh/docs/llms.txt
> Use this file to discover all available pages before exploring further.

# Navigate Customers to the Portal

> Three ways your customers can reach their Customer Portal

There are three ways your customers can land on the Customer Portal: the default URL, a pre-authenticated link you generate from your application, or the links Polar automatically includes in the emails it sends to your customers.

## 1. The default portal URL

Every organization gets a Customer Portal hosted at:

```
https://polar.sh/<your-org-slug>/portal
```

Customers authenticate by entering the email address they used to purchase or subscribe. Polar then emails them a one-time code to complete sign-in.

<img className="block dark:hidden" src="https://mintcdn.com/polar/Ut0vPUvE1pIdMcH2/assets/features/customer-portal/signin.light.png?fit=max&auto=format&n=Ut0vPUvE1pIdMcH2&q=85&s=1320caeb29255a378aa93359741683dc" width="2560" height="1600" data-path="assets/features/customer-portal/signin.light.png" />

<img className="hidden dark:block" src="https://mintcdn.com/polar/Ut0vPUvE1pIdMcH2/assets/features/customer-portal/signin.dark.png?fit=max&auto=format&n=Ut0vPUvE1pIdMcH2&q=85&s=ccae95a5124e942c32465d86d3a3c2be" width="2560" height="1600" data-path="assets/features/customer-portal/signin.dark.png" />

This URL is a good choice to link from your marketing site, your app's help menu, or a support article — anywhere you need a stable, shareable link.

## 2. Pre-authenticated portal links

If your customer is already signed in to your application, you can generate an authenticated link that drops them directly into the portal — no email code required.

Under the hood, this calls the [Create Customer Session](/docs/api-reference/customer-sessions/create-customer-session) endpoint and redirects the user to the `customerPortalUrl` it returns.

```typescript theme={null}
import { Polar } from "@polar-sh/sdk";

const polar = new Polar({
  accessToken: process.env["POLAR_ACCESS_TOKEN"] ?? "",
});

async function run() {
  const result = await polar.customerSessions.create({
    customerId: "<value>",
  });

  redirect(result.customerPortalUrl);
}

run();
```

If you're on Next.js, the `@polar-sh/nextjs` adapter wraps this into a single route handler:

```typescript theme={null}
// app/portal/route.ts
import { CustomerPortal } from "@polar-sh/nextjs";

export const GET = CustomerPortal({
  accessToken: process.env.POLAR_ACCESS_TOKEN,
  getCustomerId: async (req) => "<value>",
  server: "sandbox", // Use sandbox if you're testing Polar — pass 'production' otherwise
});
```

Point a link in your app at `/portal` and your customer is one click away from managing their billing.

<Note>
  Customer Session tokens are short-lived. Always generate a fresh link at the
  moment the customer clicks, rather than storing the URL.
</Note>

## 3. Emails Polar sends to your customers

You don't have to build anything to get customers to the portal — Polar already includes a link to it in the transactional emails we send on your behalf, including:

* **Order confirmation** emails sent after a successful checkout
* **Subscription renewal** and **subscription updated** emails
* **Failed payment** notifications, so customers can update their card

This means that even if you never link to the portal from your own app, your customers already have a way to get back to it from their inbox.
