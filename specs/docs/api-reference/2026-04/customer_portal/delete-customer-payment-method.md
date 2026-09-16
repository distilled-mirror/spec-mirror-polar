> ## Documentation Index
> Fetch the complete documentation index at: https://polar.sh/docs/llms.txt
> Use this file to discover all available pages before exploring further.

# Delete Customer Payment Method

> Delete a payment method from the authenticated customer.



## OpenAPI

````yaml /openapi/2026-04.openapi.json delete /v1/customer-portal/customers/me/payment-methods/{id}
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
  /v1/customer-portal/customers/me/payment-methods/{id}:
    delete:
      tags:
        - customer_portal
        - customers
        - public
      summary: Delete Customer Payment Method
      description: Delete a payment method from the authenticated customer.
      operationId: customer_portal:customers:delete_payment_method
      parameters:
        - name: id
          in: path
          required: true
          schema:
            type: string
            format: uuid4
            title: Id
      responses:
        '204':
          description: Payment method deleted.
        '400':
          description: Payment method is still needed to bill a subscription.
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/PaymentMethodInUseByActiveSubscription'
        '404':
          description: Payment method not found.
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
            - customer_portal:write
        - member_session:
            - customer_portal:write
      x-codeSamples:
        - lang: python
          source: |
            from polar.v2026_04 import Polar

            polar = Polar("polar_oat_xxx")

            polar.customer_portal.customers.delete_payment_method(
                '00000000-0000-4000-8000-000000000000',
            )
        - lang: typescript
          source: |
            import { createPolar } from "@polar-sh/sdk/2026-04";

            const polar = createPolar({
              accessToken: "polar_oat_xxx",
            });

            await polar.customerPortal.customers.deletePaymentMethod(
              "00000000-0000-4000-8000-000000000000",
            );
components:
  schemas:
    PaymentMethodInUseByActiveSubscription:
      properties:
        error:
          type: string
          const: PaymentMethodInUseByActiveSubscription
          title: Error
          examples:
            - PaymentMethodInUseByActiveSubscription
        detail:
          type: string
          title: Detail
      type: object
      required:
        - error
        - detail
      title: PaymentMethodInUseByActiveSubscription
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
