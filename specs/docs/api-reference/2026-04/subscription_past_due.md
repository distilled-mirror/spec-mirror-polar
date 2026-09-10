> ## Documentation Index
> Fetch the complete documentation index at: https://polar.sh/docs/llms.txt
> Use this file to discover all available pages before exploring further.

# subscription.past_due

> Sent when a subscription payment fails and the subscription enters `past_due` status.

This is a recoverable state - the customer can update their payment method to restore the subscription.
Benefits may be revoked depending on the organization's grace period settings.

If payment retries are exhausted, a `subscription.revoked` event will be sent.

**Discord & Slack support:** Full



## OpenAPI

````yaml /openapi/2026-04.openapi.json webhook subscription.past_due
openapi: 3.1.0
info:
  title: Polar API
  summary: Polar HTTP and Webhooks API
  description: Read the docs at https://polar.sh/docs/api-reference
  version: 2026-04
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
