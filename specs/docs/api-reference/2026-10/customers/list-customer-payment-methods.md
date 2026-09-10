> ## Documentation Index
> Fetch the complete documentation index at: https://polar.sh/docs/llms.txt
> Use this file to discover all available pages before exploring further.

# List Customer Payment Methods

> Get saved payment methods of a customer.

**Scopes**: `customers:read` `customers:write`



## OpenAPI

````yaml /openapi/2026-10.openapi.json get /v1/customers/{id}/payment-methods
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
  /v1/customers/{id}/payment-methods:
    get:
      tags:
        - customers
        - public
      summary: List Customer Payment Methods
      description: |-
        Get saved payment methods of a customer.

        **Scopes**: `customers:read` `customers:write`
      operationId: customers:list_payment_methods
      parameters:
        - name: id
          in: path
          required: true
          schema:
            type: string
            format: uuid4
            description: The customer ID.
            title: Id
          description: The customer ID.
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
                $ref: '#/components/schemas/ListResource_PaymentMethod_'
        '404':
          description: Customer not found.
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
            - customers:read
            - customers:write
        - pat:
            - customers:read
            - customers:write
        - oat:
            - customers:read
            - customers:write
      x-codeSamples:
        - lang: python
          source: |
            from polar.v2026_10 import Polar

            polar = Polar("polar_oat_xxx")

            for item in polar.customers.iter_list_payment_methods(
                '00000000-0000-4000-8000-000000000000',
                page=1,
                limit=10,
            ):
                print(item)
        - lang: typescript
          source: |
            import { createPolar } from "@polar-sh/sdk/2026-10";

            const polar = createPolar({
              accessToken: "polar_oat_xxx",
            });

            for await (const item of polar.customers.iterListPaymentMethods(
              "00000000-0000-4000-8000-000000000000",
              {
                "page": 1,
                "limit": 10
              },
            )) {
              console.log(item);
            }
components:
  schemas:
    ListResource_PaymentMethod_:
      properties:
        items:
          items:
            $ref: '#/components/schemas/PaymentMethod'
            title: PaymentMethod
          type: array
          title: Items
        pagination:
          $ref: '#/components/schemas/Pagination'
      type: object
      required:
        - items
        - pagination
      title: ListResource[PaymentMethod]
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
    PaymentMethod:
      anyOf:
        - $ref: '#/components/schemas/CustomerPaymentMethodCard'
        - $ref: '#/components/schemas/CustomerPaymentMethodKrCard'
        - $ref: '#/components/schemas/CustomerPaymentMethodGeneric'
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
    CustomerPaymentMethodCard:
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
        modified_at:
          anyOf:
            - type: string
              format: date-time
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
        is_default:
          type: boolean
          title: Is Default
          description: >-
            Whether this payment method is the customer's default payment
            method.
          examples:
            - true
      type: object
      required:
        - id
        - created_at
        - modified_at
        - processor
        - customer_id
        - type
        - method_metadata
        - is_default
      title: CustomerPaymentMethodCard
    CustomerPaymentMethodKrCard:
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
        modified_at:
          anyOf:
            - type: string
              format: date-time
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
        is_default:
          type: boolean
          title: Is Default
          description: >-
            Whether this payment method is the customer's default payment
            method.
          examples:
            - false
      type: object
      required:
        - id
        - created_at
        - modified_at
        - processor
        - customer_id
        - type
        - method_metadata
        - is_default
      title: CustomerPaymentMethodKrCard
    CustomerPaymentMethodGeneric:
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
        modified_at:
          anyOf:
            - type: string
              format: date-time
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
        is_default:
          type: boolean
          title: Is Default
          description: >-
            Whether this payment method is the customer's default payment
            method.
          examples:
            - false
      type: object
      required:
        - id
        - created_at
        - modified_at
        - processor
        - customer_id
        - type
        - is_default
      title: CustomerPaymentMethodGeneric
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
