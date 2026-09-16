> ## Documentation Index
> Fetch the complete documentation index at: https://polar.sh/docs/llms.txt
> Use this file to discover all available pages before exploring further.

# List Webhook Endpoints

> List webhook endpoints.

**Scopes**: `webhooks:read` `webhooks:write`



## OpenAPI

````yaml /openapi/2026-04.openapi.json get /v1/webhooks/endpoints
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
  - name: mcp
    description: Endpoints supported by Polar's MCP server.
  - name: cli
    description: Endpoints exposed as commands by Polar's CLI.
paths:
  /v1/webhooks/endpoints:
    get:
      tags:
        - webhooks
        - public
        - mcp
        - cli
      summary: List Webhook Endpoints
      description: |-
        List webhook endpoints.

        **Scopes**: `webhooks:read` `webhooks:write`
      operationId: webhooks:list_webhook_endpoints
      parameters:
        - name: organization_id
          in: query
          required: false
          schema:
            anyOf:
              - type: string
                format: uuid4
                description: The organization ID.
                examples:
                  - 1dbfc517-0bbf-4301-9ba8-555ca42b9737
              - type: array
                items:
                  type: string
                  format: uuid4
                  description: The organization ID.
                  examples:
                    - 1dbfc517-0bbf-4301-9ba8-555ca42b9737
              - type: 'null'
            description: Filter by organization ID.
            title: Organization Id
          description: Filter by organization ID.
        - name: page
          in: query
          required: false
          schema:
            type: integer
            exclusiveMinimum: 0
            description: Page number, defaults to 1.
            default: 1
            title: Page
          description: Page number, defaults to 1.
        - name: limit
          in: query
          required: false
          schema:
            type: integer
            exclusiveMinimum: 0
            description: Size of a page, defaults to 10. Maximum is 100.
            default: 10
            title: Limit
          description: Size of a page, defaults to 10. Maximum is 100.
      responses:
        '200':
          description: Successful Response
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/ListResource_WebhookEndpoint_'
        '422':
          description: Validation Error
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/HTTPValidationError'
      security:
        - oidc:
            - webhooks:read
            - webhooks:write
        - pat:
            - webhooks:read
            - webhooks:write
        - oat:
            - webhooks:read
            - webhooks:write
      x-codeSamples:
        - lang: python
          source: |
            from polar.v2026_04 import Polar

            polar = Polar("polar_oat_xxx")

            for item in polar.webhooks.iter_list_webhook_endpoints(
                page=1,
                limit=10,
            ):
                print(item)
        - lang: typescript
          source: |
            import { createPolar } from "@polar-sh/sdk/2026-04";

            const polar = createPolar({
              accessToken: "polar_oat_xxx",
            });

            for await (const item of polar.webhooks.iterListWebhookEndpoints(
              {
                "page": 1,
                "limit": 10
              },
            )) {
              console.log(item);
            }
components:
  schemas:
    ListResource_WebhookEndpoint_:
      properties:
        items:
          items:
            $ref: '#/components/schemas/WebhookEndpoint'
          type: array
          title: Items
        pagination:
          $ref: '#/components/schemas/Pagination'
      type: object
      required:
        - items
        - pagination
      title: ListResource[WebhookEndpoint]
    HTTPValidationError:
      properties:
        detail:
          items:
            $ref: '#/components/schemas/ValidationError'
          type: array
          title: Detail
      type: object
      title: HTTPValidationError
    WebhookEndpoint:
      properties:
        created_at:
          type: string
          format: date-time
          title: Created At
          description: Creation timestamp of the object.
          examples:
            - '2026-01-01T00:00:00.000000Z'
        modified_at:
          anyOf:
            - type: string
              format: date-time
              examples:
                - '2026-01-01T00:00:00.000000Z'
            - type: 'null'
          title: Modified At
          description: Last modification timestamp of the object.
        id:
          type: string
          format: uuid4
          title: Id
          description: The ID of the object.
        url:
          type: string
          title: Url
          description: The URL where the webhook events will be sent.
          examples:
            - https://webhook.site/cb791d80-f26e-4f8c-be88-6e56054192b0
        name:
          anyOf:
            - type: string
            - type: 'null'
          title: Name
          description: >-
            An optional name for the webhook endpoint to help organize and
            identify it.
        api_version:
          type: string
          title: Api Version
          description: The API version that'll be used in event payloads.
        format:
          $ref: '#/components/schemas/WebhookFormat'
          description: The format of the webhook payload.
        secret:
          type: string
          title: Secret
          description: The secret used to sign the webhook events.
          examples:
            - whsec_ovyN6cPrTv56AApvzCaJno08SSmGJmgbWilb33N2JuK
        organization_id:
          type: string
          format: uuid4
          title: Organization Id
          description: The organization ID associated with the webhook endpoint.
        events:
          items:
            $ref: '#/components/schemas/WebhookEventType'
          type: array
          title: Events
          description: The events that will trigger the webhook.
        enabled:
          type: boolean
          title: Enabled
          description: Whether the webhook endpoint is enabled and will receive events.
        uses_standard_webhook_signature:
          type: boolean
          title: Uses Standard Webhook Signature
          description: >-
            Whether Polar signs deliveries to this endpoint with Standard
            Webhooks. False means Polar's original HMAC over the UTF-8 bytes of
            the full secret.
          readOnly: true
      type: object
      required:
        - created_at
        - modified_at
        - id
        - url
        - api_version
        - format
        - secret
        - organization_id
        - events
        - enabled
        - uses_standard_webhook_signature
      title: WebhookEndpoint
      description: A webhook endpoint.
    Pagination:
      properties:
        total_count:
          type: integer
          title: Total Count
        max_page:
          type: integer
          title: Max Page
      type: object
      required:
        - total_count
        - max_page
      title: Pagination
    ValidationError:
      properties:
        loc:
          items:
            anyOf:
              - type: string
              - type: integer
          type: array
          title: Location
        msg:
          type: string
          title: Message
        type:
          type: string
          title: Error Type
        input:
          title: Input
        ctx:
          type: object
          title: Context
      type: object
      required:
        - loc
        - msg
        - type
      title: ValidationError
    WebhookFormat:
      type: string
      enum:
        - raw
        - discord
        - slack
      title: WebhookFormat
    WebhookEventType:
      type: string
      enum:
        - checkout.created
        - checkout.updated
        - checkout.expired
        - customer.created
        - customer.updated
        - customer.deleted
        - customer.state_changed
        - customer_seat.assigned
        - customer_seat.claimed
        - customer_seat.revoked
        - member.created
        - member.updated
        - member.deleted
        - order.created
        - order.updated
        - order.paid
        - order.refunded
        - subscription.created
        - subscription.updated
        - subscription.active
        - subscription.canceled
        - subscription.uncanceled
        - subscription.cycled
        - subscription.revoked
        - subscription.past_due
        - subscription.paused
        - subscription.resumed
        - refund.created
        - refund.updated
        - product.created
        - product.updated
        - discount.created
        - discount.updated
        - discount.deleted
        - benefit.created
        - benefit.updated
        - benefit_grant.created
        - benefit_grant.cycled
        - benefit_grant.updated
        - benefit_grant.revoked
        - organization.updated
      title: WebhookEventType
  securitySchemes:
    oidc:
      type: openIdConnect
      openIdConnectUrl: /.well-known/openid-configuration
    pat:
      type: http
      description: >-
        You can generate a **Personal Access Token** from your
        [settings](https://polar.sh/settings).
      scheme: bearer
    oat:
      type: http
      description: >-
        You can generate an **Organization Access Token** from your
        organization's settings.
      scheme: bearer

````
