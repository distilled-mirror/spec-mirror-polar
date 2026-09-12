> ## Documentation Index
> Fetch the complete documentation index at: https://polar.sh/docs/llms.txt
> Use this file to discover all available pages before exploring further.

# List Customer Payment Methods

> Get saved payment methods of the authenticated customer.

<Note>
  To change the default payment method, call the [`PATCH /v1/customer-portal/customers/me`](/docs/api-reference/customer_portal/update-customer) endpoint with the desired `default_payment_method_id`.
</Note>


## OpenAPI

````yaml /openapi/2026-04.openapi.json get /v1/customer-portal/customers/me/payment-methods
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
  /v1/customer-portal/customers/me/payment-methods:
    get:
      tags:
        - customer_portal
        - customers
        - public
      summary: List Customer Payment Methods
      description: Get saved payment methods of the authenticated customer.
      operationId: customer_portal:customers:list_payment_methods
      parameters:
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
                $ref: '#/components/schemas/ListResource_CustomerPaymentMethod_'
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
          source: >
            from polar.v2026_04 import Polar


            polar = Polar("polar_oat_xxx")


            for item in
            polar.customer_portal.customers.iter_list_payment_methods(
                page=1,
                limit=10,
            ):
                print(item)
        - lang: typescript
          source: >
            import { createPolar } from "@polar-sh/sdk/2026-04";


            const polar = createPolar({
              accessToken: "polar_oat_xxx",
            });


            for await (const item of
            polar.customerPortal.customers.iterListPaymentMethods(
              {
                "page": 1,
                "limit": 10
              },
            )) {
              console.log(item);
            }
components:
  schemas:
    ListResource_CustomerPaymentMethod_:
      properties:
        items:
          items:
            $ref: '#/components/schemas/CustomerPaymentMethod'
            title: CustomerPaymentMethod
          type: array
          title: Items
        pagination:
          $ref: '#/components/schemas/Pagination'
      type: object
      required:
        - items
        - pagination
      title: ListResource[CustomerPaymentMethod]
    HTTPValidationError:
      properties:
        detail:
          items:
            $ref: '#/components/schemas/ValidationError'
          type: array
          title: Detail
      type: object
      title: HTTPValidationError
    CustomerPaymentMethod:
      anyOf:
        - $ref: '#/components/schemas/PaymentMethodCard'
        - $ref: '#/components/schemas/PaymentMethodKrCard'
        - $ref: '#/components/schemas/PaymentMethodGeneric'
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
    PaymentMethodCard:
      properties:
        id:
          type: string
          format: uuid4
          title: Id
          description: The ID of the object.
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
        processor:
          $ref: '#/components/schemas/PaymentProcessor'
        customer_id:
          type: string
          format: uuid4
          title: Customer Id
        type:
          type: string
          const: card
          title: Type
        method_metadata:
          $ref: '#/components/schemas/PaymentMethodCardMetadata'
      type: object
      required:
        - id
        - created_at
        - modified_at
        - processor
        - customer_id
        - type
        - method_metadata
      title: PaymentMethodCard
    PaymentMethodKrCard:
      properties:
        id:
          type: string
          format: uuid4
          title: Id
          description: The ID of the object.
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
        processor:
          $ref: '#/components/schemas/PaymentProcessor'
        customer_id:
          type: string
          format: uuid4
          title: Customer Id
        type:
          type: string
          const: kr_card
          title: Type
        method_metadata:
          $ref: '#/components/schemas/PaymentMethodKrCardMetadata'
      type: object
      required:
        - id
        - created_at
        - modified_at
        - processor
        - customer_id
        - type
        - method_metadata
      title: PaymentMethodKrCard
    PaymentMethodGeneric:
      properties:
        id:
          type: string
          format: uuid4
          title: Id
          description: The ID of the object.
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
        processor:
          $ref: '#/components/schemas/PaymentProcessor'
        customer_id:
          type: string
          format: uuid4
          title: Customer Id
        type:
          type: string
          title: Type
      type: object
      required:
        - id
        - created_at
        - modified_at
        - processor
        - customer_id
        - type
      title: PaymentMethodGeneric
    PaymentProcessor:
      type: string
      enum:
        - stripe
      title: PaymentProcessor
    PaymentMethodCardMetadata:
      properties:
        brand:
          type: string
          title: Brand
        last4:
          type: string
          title: Last4
        exp_month:
          type: integer
          title: Exp Month
        exp_year:
          type: integer
          title: Exp Year
        wallet:
          anyOf:
            - type: string
            - type: 'null'
          title: Wallet
      type: object
      required:
        - brand
        - last4
        - exp_month
        - exp_year
      title: PaymentMethodCardMetadata
    PaymentMethodKrCardMetadata:
      properties:
        brand:
          anyOf:
            - type: string
            - type: 'null'
          title: Brand
        last4:
          anyOf:
            - type: string
            - type: 'null'
          title: Last4
      type: object
      required:
        - brand
        - last4
      title: PaymentMethodKrCardMetadata
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
