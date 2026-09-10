> ## Documentation Index
> Fetch the complete documentation index at: https://polar.sh/docs/llms.txt
> Use this file to discover all available pages before exploring further.

# Generate Order Invoice

> Trigger generation of an order's invoice.

**Scopes**: `orders:read`

<Warning>
  Once the invoice is generated, it's permanent and cannot be modified.

  Make sure the billing details (name and address) are correct before generating the invoice. You can update them before generating the invoice by calling the [`PATCH /v1/orders/{id}`](/docs/api-reference/orders/update-order) endpoint.
</Warning>

<Note>
  After successfully calling this endpoint, you get a `202` response, meaning the generation of the invoice has been scheduled. It usually only takes a few seconds before you can retrieve the invoice using the [`GET /v1/orders/{id}/invoice`](/docs/api-reference/orders/get-order-invoice) endpoint.

  If you want a reliable notification when the invoice is ready, you can listen to the [`order.updated`](/docs/api-reference/orderupdated) webhook and check the [`is_invoice_generated` field](/docs/api-reference/orderupdated#schema-data-is-invoice-generated).
</Note>


## OpenAPI

````yaml /openapi/2026-10.openapi.json post /v1/orders/{id}/invoice
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
  /v1/orders/{id}/invoice:
    post:
      tags:
        - orders
        - public
      summary: Generate Order Invoice
      description: |-
        Trigger generation of an order's invoice.

        **Scopes**: `orders:read`
      operationId: orders:generate_invoice
      parameters:
        - name: id
          in: path
          required: true
          schema:
            type: string
            format: uuid4
            description: The order ID.
            title: Id
          description: The order ID.
      responses:
        '202':
          description: Successful Response
          content:
            application/json:
              schema: {}
        '404':
          description: Order not found.
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/ResourceNotFound'
        '409':
          description: Order is not eligible for invoice generation (invalid status).
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/OrderNotEligibleForInvoice'
        '422':
          description: Order is missing billing name or address.
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/MissingInvoiceBillingDetails'
      security:
        - oidc:
            - orders:read
        - pat:
            - orders:read
        - oat:
            - orders:read
      x-codeSamples:
        - lang: python
          source: |
            from polar.v2026_10 import Polar

            polar = Polar("polar_oat_xxx")

            response = polar.orders.generate_invoice(
                '00000000-0000-4000-8000-000000000000',
            )
            print(response)
        - lang: typescript
          source: |
            import { createPolar } from "@polar-sh/sdk/2026-10";

            const polar = createPolar({
              accessToken: "polar_oat_xxx",
            });

            const response = await polar.orders.generateInvoice(
              "00000000-0000-4000-8000-000000000000",
            );
            console.log(response);
components:
  schemas:
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
    OrderNotEligibleForInvoice:
      properties:
        error:
          type: string
          const: OrderNotEligibleForInvoice
          title: Error
          examples:
            - OrderNotEligibleForInvoice
        detail:
          type: string
          title: Detail
      type: object
      required:
        - error
        - detail
      title: OrderNotEligibleForInvoice
    MissingInvoiceBillingDetails:
      properties:
        error:
          type: string
          const: MissingInvoiceBillingDetails
          title: Error
          examples:
            - MissingInvoiceBillingDetails
        detail:
          type: string
          title: Detail
      type: object
      required:
        - error
        - detail
      title: MissingInvoiceBillingDetails
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
