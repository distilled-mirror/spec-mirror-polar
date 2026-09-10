> ## Documentation Index
> Fetch the complete documentation index at: https://polar.sh/docs/llms.txt
> Use this file to discover all available pages before exploring further.

# subscription.cycled

> Sent when a subscription enters a new billing period.

The payload carries the new `current_period_start` and `current_period_end`.
It fires when the period rolls over, before the renewal order exists and
regardless of whether the renewal payment succeeds — listen to `order.paid`
if you need the payment.

A trial converting to a paid subscription starts a new period, so it fires
there too. Read `status` to tell the two apart.

**Discord & Slack support:** Basic



## OpenAPI

````yaml /openapi/2026-10.openapi.json webhook subscription.cycled
openapi: 3.1.0
info:
  title: Polar API
  summary: Polar HTTP and Webhooks API
  description: Read the docs at https://polar.sh/docs/api-reference
  version: 2026-10
servers:
  - url: https://api.polar.sh
    description: Production environment
    x-speakeasy-server-id: production
    x-polar-environment: production
  - url: https://sandbox-api.polar.sh
    description: Sandbox environment
    x-speakeasy-server-id: sandbox
    x-polar-environment: sandbox
security: []
tags:
  - name: public
    description: >-
      Endpoints shown and documented in the Polar API documentation and
      available in our SDKs.
  - name: private
    description: >-
      Endpoints that should appear in the schema only in development to generate
      our internal JS SDK.
paths: {}

````
