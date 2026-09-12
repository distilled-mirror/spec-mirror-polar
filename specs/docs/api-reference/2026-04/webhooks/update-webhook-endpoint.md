> ## Documentation Index
> Fetch the complete documentation index at: https://polar.sh/docs/llms.txt
> Use this file to discover all available pages before exploring further.

# Update Webhook Endpoint

> Update a webhook endpoint.

**Scopes**: `webhooks:write`



## OpenAPI

````yaml /openapi/2026-04.openapi.json patch /v1/webhooks/endpoints/{id}
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
paths:
  /v1/webhooks/endpoints/{id}:
    patch:
      tags:
        - webhooks
        - public
      summary: Update Webhook Endpoint
      description: |-
        Update a webhook endpoint.

        **Scopes**: `webhooks:write`
      operationId: webhooks:update_webhook_endpoint
      parameters:
        - name: id
          in: path
          required: true
          schema:
            type: string
            format: uuid4
            description: The webhook endpoint ID.
            title: Id
          description: The webhook endpoint ID.
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/WebhookEndpointUpdate'
      responses:
        '200':
          description: Webhook endpoint updated.
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/WebhookEndpoint'
        '404':
          description: Webhook endpoint not found.
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/ResourceNotFound'
        '422':
          description: Validation Error
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/HTTPValidationError'
      security:
        - oidc:
            - webhooks:write
        - pat:
            - webhooks:write
        - oat:
            - webhooks:write
      x-codeSamples:
        - lang: python
          source: |
            from polar.v2026_04 import Polar

            polar = Polar("polar_oat_xxx")

            response = polar.webhooks.update_webhook_endpoint(
                '00000000-0000-4000-8000-000000000000',
            )
            print(response)
        - lang: typescript
          source: |
            import { createPolar } from "@polar-sh/sdk/2026-04";

            const polar = createPolar({
              accessToken: "polar_oat_xxx",
            });

            const response = await polar.webhooks.updateWebhookEndpoint(
              "00000000-0000-4000-8000-000000000000",
              {},
            );
            console.log(response);
components:
  schemas:
    WebhookEndpointUpdate:
      properties:
        url:
          anyOf:
            - type: string
              maxLength: 2083
              minLength: 1
              format: uri
              description: The URL where the webhook events will be sent.
              examples:
                - https://webhook.site/cb791d80-f26e-4f8c-be88-6e56054192b0
            - type: 'null'
          title: Url
        name:
          anyOf:
            - type: string
            - type: 'null'
          title: Name
          description: >-
            An optional name for the webhook endpoint to help organize and
            identify it.
        api_version:
          anyOf:
            - type: string
              enum:
                - 2026-04
                - 2026-10
            - type: 'null'
          title: Api Version
          description: The API version that'll be used in event payloads.
        format:
          anyOf:
            - $ref: '#/components/schemas/WebhookFormat'
              description: The format of the webhook payload.
            - type: 'null'
        events:
          anyOf:
            - items:
                $ref: '#/components/schemas/WebhookEventType'
              type: array
              description: The events that will trigger the webhook.
            - type: 'null'
          title: Events
        enabled:
          anyOf:
            - type: boolean
            - type: 'null'
          title: Enabled
          description: Whether the webhook endpoint is enabled.
      type: object
      title: DeprecatedWebhookEndpointUpdateWithSecret
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
    ResourceNotFound:
      properties:
        error:
          type: string
          const: ResourceNotFound
          title: Error
          examples:
            - ResourceNotFound
        detail:
          type: string
          title: Detail
      type: object
      required:
        - error
        - detail
      title: ResourceNotFound
    HTTPValidationError:
      properties:
        detail:
          items:
            $ref: '#/components/schemas/ValidationError'
          type: array
          title: Detail
      type: object
      title: HTTPValidationError
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
