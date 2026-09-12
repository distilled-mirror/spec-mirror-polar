> ## Documentation Index
> Fetch the complete documentation index at: https://polar.sh/docs/llms.txt
> Use this file to discover all available pages before exploring further.

# Create Refund

> Create a refund.

**Scopes**: `refunds:write`



## OpenAPI

````yaml /openapi/2026-04.openapi.json post /v1/refunds/
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
  /v1/refunds/:
    post:
      tags:
        - refunds
        - public
      summary: Create Refund
      description: |-
        Create a refund.

        **Scopes**: `refunds:write`
      operationId: refunds:create
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/RefundCreate'
      responses:
        '201':
          description: Refund created.
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Refund'
        '403':
          description: Order is already fully refunded.
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/RefundedAlready'
        '422':
          description: Validation Error
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/HTTPValidationError'
      security:
        - oidc:
            - refunds:write
        - pat:
            - refunds:write
        - oat:
            - refunds:write
      x-codeSamples:
        - lang: python
          source: |
            from polar.v2026_04 import Polar

            polar = Polar("polar_oat_xxx")

            response = polar.refunds.create(
                order_id='00000000-0000-4000-8000-000000000000',
                reason='duplicate',
                amount=1,
                revoke_benefits=False,
            )
            print(response)
        - lang: typescript
          source: |
            import { createPolar } from "@polar-sh/sdk/2026-04";

            const polar = createPolar({
              accessToken: "polar_oat_xxx",
            });

            const response = await polar.refunds.create(
              {
                "order_id": "00000000-0000-4000-8000-000000000000",
                "reason": "duplicate",
                "amount": 1,
                "revoke_benefits": false
              },
            );
            console.log(response);
components:
  schemas:
    RefundCreate:
      properties:
        metadata:
          additionalProperties:
            anyOf:
              - type: string
                maxLength: 500
                minLength: 1
              - type: integer
              - type: number
              - type: boolean
          propertyNames:
            maxLength: 40
            minLength: 1
          type: object
          maxProperties: 50
          title: Metadata
          description: |-
            Key-value object allowing you to store additional information.

            The key must be a string with a maximum length of **40 characters**.
            The value must be either:

            * A string with a maximum length of **500 characters**
            * An integer
            * A floating-point number
            * A boolean

            You can store up to **50 key-value pairs**.
        order_id:
          type: string
          format: uuid4
          title: Order Id
        reason:
          type: string
          enum:
            - duplicate
            - fraudulent
            - customer_request
            - service_disruption
            - satisfaction_guarantee
            - other
          title: Reason
          description: Reason for the refund.
        amount:
          type: integer
          exclusiveMinimum: 0
          title: Amount
          description: Amount to refund in cents. Minimum is 1.
        comment:
          anyOf:
            - type: string
            - type: 'null'
          title: Comment
          description: An internal comment about the refund.
        revoke_benefits:
          type: boolean
          title: Revoke Benefits
          description: >-
            Should this refund trigger the associated customer benefits to be
            revoked?


            **Note:**

            Only allowed in case the `order` is a one-time purchase.

            Subscriptions automatically revoke customer benefits once the

            subscription itself is revoked, i.e fully canceled.
          default: false
      type: object
      required:
        - order_id
        - reason
        - amount
      title: RefundCreate
    Refund:
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
        metadata:
          $ref: '#/components/schemas/MetadataOutputType'
        status:
          $ref: '#/components/schemas/RefundStatus'
        reason:
          $ref: '#/components/schemas/RefundReason'
        amount:
          type: integer
          title: Amount
        tax_amount:
          type: integer
          title: Tax Amount
        currency:
          type: string
          title: Currency
        organization_id:
          type: string
          format: uuid4
          title: Organization Id
        order_id:
          type: string
          format: uuid4
          title: Order Id
        subscription_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Subscription Id
        customer_id:
          type: string
          format: uuid4
          title: Customer Id
        revoke_benefits:
          type: boolean
          title: Revoke Benefits
        dispute:
          anyOf:
            - $ref: '#/components/schemas/RefundDispute'
            - type: 'null'
      type: object
      required:
        - created_at
        - modified_at
        - id
        - metadata
        - status
        - reason
        - amount
        - tax_amount
        - currency
        - organization_id
        - order_id
        - subscription_id
        - customer_id
        - revoke_benefits
        - dispute
      title: Refund
    RefundedAlready:
      properties:
        error:
          type: string
          const: RefundedAlready
          title: Error
          examples:
            - RefundedAlready
        detail:
          type: string
          title: Detail
      type: object
      required:
        - error
        - detail
      title: RefundedAlready
    HTTPValidationError:
      properties:
        detail:
          items:
            $ref: '#/components/schemas/ValidationError'
          type: array
          title: Detail
      type: object
      title: HTTPValidationError
    MetadataOutputType:
      additionalProperties:
        anyOf:
          - type: string
          - type: integer
          - type: number
          - type: boolean
      type: object
    RefundStatus:
      type: string
      enum:
        - pending
        - succeeded
        - failed
        - canceled
      title: RefundStatus
    RefundReason:
      type: string
      enum:
        - duplicate
        - fraudulent
        - customer_request
        - service_disruption
        - satisfaction_guarantee
        - dispute_prevention
        - other
      title: RefundReason
    RefundDispute:
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
        status:
          $ref: '#/components/schemas/DisputeStatus'
          description: >-
            Status of the dispute. `prevented` means we issued a refund before
            the dispute was escalated, avoiding any fees.
          examples:
            - needs_response
            - prevented
        resolved:
          type: boolean
          title: Resolved
          description: Whether the dispute has been resolved (won or lost).
          examples:
            - false
        closed:
          type: boolean
          title: Closed
          description: Whether the dispute is closed (prevented, won, or lost).
          examples:
            - false
        amount:
          type: integer
          title: Amount
          description: Amount in cents disputed.
          examples:
            - 1000
        tax_amount:
          type: integer
          title: Tax Amount
          description: Tax amount in cents disputed.
          examples:
            - 200
        currency:
          type: string
          title: Currency
          description: Currency code of the dispute.
          examples:
            - usd
        reason:
          anyOf:
            - type: string
            - type: 'null'
          title: Reason
          description: >-
            The reason for the dispute as reported by the card network (e.g.
            `fraudulent`, `product_not_received`). `None` until the processor
            reports it.
          examples:
            - fraudulent
        evidence_due_by:
          anyOf:
            - type: string
              format: date-time
              examples:
                - '2026-01-01T00:00:00.000000Z'
            - type: 'null'
          title: Evidence Due By
          description: >-
            Deadline to submit evidence in response to the dispute. `None` when
            no response is required.
        past_due:
          type: boolean
          title: Past Due
          description: Whether the evidence submission deadline has passed.
          examples:
            - false
        order_id:
          type: string
          format: uuid4
          title: Order Id
          description: The ID of the order associated with the dispute.
          examples:
            - 57107b74-8400-4d80-a2fc-54c2b4239cb3
        payment_id:
          type: string
          format: uuid4
          title: Payment Id
          description: The ID of the payment associated with the dispute.
          examples:
            - 42b94870-36b9-4573-96b6-b90b1c99a353
      type: object
      required:
        - created_at
        - modified_at
        - id
        - status
        - resolved
        - closed
        - amount
        - tax_amount
        - currency
        - reason
        - evidence_due_by
        - past_due
        - order_id
        - payment_id
      title: RefundDispute
      description: |-
        Dispute associated with a refund,
        in case we prevented a dispute by issuing a refund.
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
    DisputeStatus:
      type: string
      enum:
        - prevented
        - early_warning
        - needs_response
        - under_review
        - lost
        - won
      title: DisputeStatus
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
