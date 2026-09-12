> ## Documentation Index
> Fetch the complete documentation index at: https://polar.sh/docs/llms.txt
> Use this file to discover all available pages before exploring further.

# List Refunds

> List refunds.

**Scopes**: `refunds:read` `refunds:write`



## OpenAPI

````yaml /openapi/2026-10.openapi.json get /v1/refunds/
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
paths:
  /v1/refunds/:
    get:
      tags:
        - refunds
        - public
      summary: List Refunds
      description: |-
        List refunds.

        **Scopes**: `refunds:read` `refunds:write`
      operationId: refunds:list
      parameters:
        - name: id
          in: query
          required: false
          schema:
            anyOf:
              - type: string
                format: uuid4
                description: The refund ID.
              - type: array
                items:
                  type: string
                  format: uuid4
                  description: The refund ID.
              - type: 'null'
            title: RefundID Filter
            description: Filter by refund ID.
          description: Filter by refund ID.
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
            title: OrganizationID Filter
            description: Filter by organization ID.
          description: Filter by organization ID.
        - name: order_id
          in: query
          required: false
          schema:
            anyOf:
              - type: string
                format: uuid4
                description: The order ID.
              - type: array
                items:
                  type: string
                  format: uuid4
                  description: The order ID.
              - type: 'null'
            title: OrderID Filter
            description: Filter by order ID.
          description: Filter by order ID.
        - name: subscription_id
          in: query
          required: false
          schema:
            anyOf:
              - type: string
                format: uuid4
                description: The subscription ID.
              - type: array
                items:
                  type: string
                  format: uuid4
                  description: The subscription ID.
              - type: 'null'
            title: SubscriptionID Filter
            description: Filter by subscription ID.
          description: Filter by subscription ID.
        - name: customer_id
          in: query
          required: false
          schema:
            anyOf:
              - type: string
                format: uuid4
                description: The customer ID.
              - type: array
                items:
                  type: string
                  format: uuid4
                  description: The customer ID.
              - type: 'null'
            title: CustomerID Filter
            description: Filter by customer ID.
          description: Filter by customer ID.
        - name: external_customer_id
          in: query
          required: false
          schema:
            anyOf:
              - type: string
                description: The customer external ID.
              - type: array
                items:
                  type: string
                  description: The customer external ID.
              - type: 'null'
            title: ExternalCustomerID Filter
            description: Filter by customer external ID.
          description: Filter by customer external ID.
        - name: succeeded
          in: query
          required: false
          schema:
            anyOf:
              - type: boolean
              - type: 'null'
            title: RefundStatus Filter
            description: Filter by `succeeded`.
          description: Filter by `succeeded`.
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
        - name: sorting
          in: query
          required: false
          schema:
            anyOf:
              - type: array
                items:
                  $ref: '#/components/schemas/RefundSortProperty'
              - type: 'null'
            description: >-
              Sorting criterion. Several criteria can be used simultaneously and
              will be applied in order. Add a minus sign `-` before the criteria
              name to sort by descending order.
            default:
              - '-created_at'
            title: Sorting
          description: >-
            Sorting criterion. Several criteria can be used simultaneously and
            will be applied in order. Add a minus sign `-` before the criteria
            name to sort by descending order.
      responses:
        '200':
          description: Successful Response
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/ListResource_Refund_'
        '422':
          description: Validation Error
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/HTTPValidationError'
      security:
        - oidc:
            - refunds:read
            - refunds:write
        - pat:
            - refunds:read
            - refunds:write
        - oat:
            - refunds:read
            - refunds:write
      x-codeSamples:
        - lang: python
          source: |
            from polar.v2026_10 import Polar

            polar = Polar("polar_oat_xxx")

            for item in polar.refunds.iter_list(
                page=1,
                limit=10,
                sorting=['-created_at'],
            ):
                print(item)
        - lang: typescript
          source: |
            import { createPolar } from "@polar-sh/sdk/2026-10";

            const polar = createPolar({
              accessToken: "polar_oat_xxx",
            });

            for await (const item of polar.refunds.iterList(
              {
                "page": 1,
                "limit": 10,
                "sorting": [
                  "-created_at"
                ]
              },
            )) {
              console.log(item);
            }
components:
  schemas:
    RefundSortProperty:
      type: string
      enum:
        - created_at
        - '-created_at'
        - amount
        - '-amount'
      title: RefundSortProperty
    ListResource_Refund_:
      properties:
        items:
          items:
            $ref: '#/components/schemas/Refund'
          type: array
          title: Items
        pagination:
          $ref: '#/components/schemas/Pagination'
      type: object
      required:
        - items
        - pagination
      title: ListResource[Refund]
    HTTPValidationError:
      properties:
        detail:
          items:
            $ref: '#/components/schemas/ValidationError'
          type: array
          title: Detail
      type: object
      title: HTTPValidationError
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
