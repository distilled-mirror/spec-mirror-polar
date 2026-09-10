> ## Documentation Index
> Fetch the complete documentation index at: https://polar.sh/docs/llms.txt
> Use this file to discover all available pages before exploring further.

# TypeScript SDK

> SDK for JavaScript runtimes (Node.js and Browser)

The official TypeScript SDK provides a fully typed client for the Polar API.

<Info>
  The new SDK is currently in public preview. Install it from the `next` tag to
  try it before the stable release.
</Info>

## Installation

<Tabs>
  <Tab title="npm">
    ```bash Terminal theme={null}
    npm install @polar-sh/sdk@next
    ```
  </Tab>

  <Tab title="yarn">
    ```bash Terminal theme={null}
    yarn add @polar-sh/sdk@next
    ```
  </Tab>

  <Tab title="pnpm">
    ```bash Terminal theme={null}
    pnpm add @polar-sh/sdk@next
    ```
  </Tab>
</Tabs>

## Quickstart

Create an [organization access token](/docs/integrate/oat) for server-side use, keep it out of browser bundles, store it in `POLAR_ACCESS_TOKEN`, and make your first request:

```typescript icon="square-js" index.js theme={null}
import { createPolar } from "@polar-sh/sdk/2026-04";

const polar = createPolar({
  accessToken: process.env.POLAR_ACCESS_TOKEN!,
});

const customerState = await polar.customers.getStateExternal("customer_external_id");
console.log(customerState);
```

The import path pins your client to the `2026-04` API version.

## Sandbox environment

The client uses production by default. Pass `environment: "sandbox"` to use the isolated [sandbox environment](/docs/integrate/sandbox):

```typescript theme={null}
const polar = createPolar({
  accessToken: process.env.POLAR_ACCESS_TOKEN!,
  environment: "sandbox",
});
```

[View the source code on GitHub](https://github.com/polarsource/polar/tree/main/sdk/typescript).

## Framework adapters

Implement Checkout & Webhook handlers in few lines of code.

* [Better Auth](/docs/integrate/sdk/adapters/better-auth)
* [Next.js](/docs/integrate/sdk/adapters/nextjs)
* [Nuxt](/docs/integrate/sdk/adapters/nuxt)
* [TanStack Start](/docs/integrate/sdk/adapters/tanstack-start)

Adapters for other frameworks are deprecated, use the SDK directly instead.
The [`polar-integration` skill](https://github.com/polarsource/skills/tree/main/skills/polar-integration)
can help you integrate or migrate:

```bash theme={null}
npx skills add https://github.com/polarsource/skills --skill polar-integration
```
