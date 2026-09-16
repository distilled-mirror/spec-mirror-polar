> ## Documentation Index
> Fetch the complete documentation index at: https://polar.sh/docs/llms.txt
> Use this file to discover all available pages before exploring further.

# Get Order Invoice

> Get an order's invoice data.

<Note>
  The invoice must be generated first before it can be retrieved. You should call the [`POST /v1/customer-portal/orders/{id}/invoice`](/docs/api-reference/customer_portal/generate-order-invoice) endpoint to generate the invoice.

  If the invoice is not generated, you will receive a `404` error.
</Note>


## OpenAPI

````yaml /openapi/2026-04.openapi.json get /v1/customer-portal/orders/{id}/invoice
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
  /v1/customer-portal/orders/{id}/invoice:
    get:
      tags:
        - customer_portal
        - orders
        - public
      summary: Get Order Invoice
      description: Get an order's invoice data.
      operationId: customer_portal:orders:invoice
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
        '200':
          description: Successful Response
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/CustomerOrderInvoice'
        '404':
          description: Order not found.
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
        - customer_session:
            - customer_portal:read
            - customer_portal:write
        - member_session:
            - customer_portal:read
            - customer_portal:write
      x-codeSamples:
        - lang: python
          source: |
            from polar.v2026_04 import Polar

            polar = Polar("polar_oat_xxx")

            response = polar.customer_portal.orders.invoice(
                '00000000-0000-4000-8000-000000000000',
            )
            print(response)
        - lang: typescript
          source: |
            import { createPolar } from "@polar-sh/sdk/2026-04";

            const polar = createPolar({
              accessToken: "polar_oat_xxx",
            });

            const response = await polar.customerPortal.orders.invoice(
              "00000000-0000-4000-8000-000000000000",
            );
            console.log(response);
components:
  schemas:
    CustomerOrderInvoice:
      properties:
        url:
          type: string
          title: Url
          description: The URL to the invoice.
      type: object
      required:
        - url
      title: CustomerOrderInvoice
      description: Order's invoice data.
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
    customer_session:
      type: http
      description: >-
        Customer session tokens are specific tokens that are used to
        authenticate customers on your organization. You can create those
        sessions programmatically using the [Create Customer Session
        endpoint](/api-reference/customer-sessions/create-customer-session).
      scheme: bearer
    member_session:
      type: http
      description: >-
        Member session tokens are specific tokens that are used to authenticate
        members on your organization. You can create those sessions
        programmatically using the [Create Member Session
        endpoint](/api-reference/customer-sessions/create-customer-session).
      scheme: bearer

````
