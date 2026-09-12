> ## Documentation Index
> Fetch the complete documentation index at: https://polar.sh/docs/llms.txt
> Use this file to discover all available pages before exploring further.

# Create Discount

> Create a discount.

**Scopes**: `discounts:write`



## OpenAPI

````yaml /openapi/2026-04.openapi.json post /v1/discounts/
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
  /v1/discounts/:
    post:
      tags:
        - discounts
        - public
      summary: Create Discount
      description: |-
        Create a discount.

        **Scopes**: `discounts:write`
      operationId: discounts:create
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/DiscountCreate'
      responses:
        '201':
          description: Discount created.
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Discount'
                title: Discount
        '422':
          description: Validation Error
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/HTTPValidationError'
      security:
        - oidc:
            - discounts:write
        - pat:
            - discounts:write
        - oat:
            - discounts:write
      x-codeSamples:
        - lang: python
          source: |
            from polar.v2026_04 import Polar

            polar = Polar("polar_oat_xxx")

            response = polar.discounts.create(
                name='string',
                type='fixed',
                duration='once',
                currency='usd',
            )
            print(response)
        - lang: typescript
          source: |
            import { createPolar } from "@polar-sh/sdk/2026-04";

            const polar = createPolar({
              accessToken: "polar_oat_xxx",
            });

            const response = await polar.discounts.create(
              {
                "name": "string",
                "type": "fixed",
                "duration": "once",
                "currency": "usd"
              },
            );
            console.log(response);
components:
  schemas:
    DiscountCreate:
      oneOf:
        - $ref: '#/components/schemas/DiscountFixedCreate'
        - $ref: '#/components/schemas/DiscountPercentageCreate'
      discriminator:
        propertyName: type
        mapping:
          fixed:
            $ref: '#/components/schemas/DiscountFixedCreate'
          percentage:
            $ref: '#/components/schemas/DiscountPercentageCreate'
    Discount:
      oneOf:
        - $ref: '#/components/schemas/DiscountFixedOnceForeverDuration'
        - $ref: '#/components/schemas/DiscountFixedRepeatDuration'
        - $ref: '#/components/schemas/DiscountPercentageOnceForeverDuration'
        - $ref: '#/components/schemas/DiscountPercentageRepeatDuration'
    HTTPValidationError:
      properties:
        detail:
          items:
            $ref: '#/components/schemas/ValidationError'
          type: array
          title: Detail
      type: object
      title: HTTPValidationError
    DiscountFixedCreate:
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
        name:
          type: string
          minLength: 1
          title: Name
          description: >-
            Name of the discount. Will be displayed to the customer when the
            discount is applied.
        code:
          anyOf:
            - type: string
            - type: 'null'
          title: Code
          description: >-
            Code customers can use to apply the discount during checkout. Must
            be between 3 and 256 characters long and contain only alphanumeric
            characters.If not provided, the discount can only be applied via the
            API.
        starts_at:
          anyOf:
            - type: string
              format: date-time
              examples:
                - '2026-01-01T00:00:00.000000Z'
            - type: 'null'
          title: Starts At
          description: Optional timestamp after which the discount is redeemable.
        ends_at:
          anyOf:
            - type: string
              format: date-time
              examples:
                - '2026-01-01T00:00:00.000000Z'
            - type: 'null'
          title: Ends At
          description: Optional timestamp after which the discount is no longer redeemable.
        max_redemptions:
          anyOf:
            - type: integer
              maximum: 2147483647
              minimum: 1
            - type: 'null'
          title: Max Redemptions
          description: Optional maximum number of times the discount can be redeemed.
        max_redemptions_per_customer:
          anyOf:
            - type: integer
              maximum: 2147483647
              minimum: 1
            - type: 'null'
          title: Max Redemptions Per Customer
          description: >-
            Optional maximum number of times the discount can be redeemed by a
            single customer.
        products:
          anyOf:
            - items:
                type: string
                format: uuid4
              type: array
              description: List of product IDs the discount can be applied to.
            - type: 'null'
          title: Products
        organization_id:
          anyOf:
            - type: string
              format: uuid4
              description: The organization ID.
              examples:
                - 1dbfc517-0bbf-4301-9ba8-555ca42b9737
            - type: 'null'
          title: Organization Id
          description: >-
            The ID of the organization owning the discount. **Required unless
            you use an organization token.**
        type:
          type: string
          const: fixed
          title: Type
          default: fixed
        duration:
          $ref: '#/components/schemas/DiscountDuration'
          description: >-
            For subscriptions, determines if the discount should be applied once
            on the first invoice, forever, or for a certain number of months
            determined by `duration_in_months`.
        duration_in_months:
          anyOf:
            - type: integer
              maximum: 999
              minimum: 1
            - type: 'null'
          title: Duration In Months
          description: |-
            Number of months the discount should be applied.

            Required when `duration` is `repeating`. Must be omitted otherwise.

            For this to work on yearly pricing, you should multiply this by 12.
            For example, to apply the discount for 2 years, set this to 24.
        amount:
          anyOf:
            - type: integer
              maximum: 999999999999
              minimum: 0
              description: Fixed amount to discount from the invoice total.
            - type: 'null'
          title: Amount
          deprecated: true
        currency:
          anyOf:
            - $ref: '#/components/schemas/PresentmentCurrency'
              description: The currency of the fixed amount discount.
            - type: 'null'
          default: usd
          deprecated: true
        amounts:
          anyOf:
            - additionalProperties:
                type: integer
                maximum: 999999999999
                minimum: 0
                description: Fixed amount to discount from the invoice total.
              propertyNames:
                $ref: '#/components/schemas/PresentmentCurrency'
              type: object
              minProperties: 1
              description: >-
                Map of currency to fixed amount to discount from the total. This
                allows specifying different discount amounts for different
                currencies.
            - type: 'null'
          title: Amounts
      type: object
      required:
        - name
        - duration
      title: DiscountFixedCreate
      description: Schema to create a fixed amount discount.
    DiscountPercentageCreate:
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
        name:
          type: string
          minLength: 1
          title: Name
          description: >-
            Name of the discount. Will be displayed to the customer when the
            discount is applied.
        code:
          anyOf:
            - type: string
            - type: 'null'
          title: Code
          description: >-
            Code customers can use to apply the discount during checkout. Must
            be between 3 and 256 characters long and contain only alphanumeric
            characters.If not provided, the discount can only be applied via the
            API.
        starts_at:
          anyOf:
            - type: string
              format: date-time
              examples:
                - '2026-01-01T00:00:00.000000Z'
            - type: 'null'
          title: Starts At
          description: Optional timestamp after which the discount is redeemable.
        ends_at:
          anyOf:
            - type: string
              format: date-time
              examples:
                - '2026-01-01T00:00:00.000000Z'
            - type: 'null'
          title: Ends At
          description: Optional timestamp after which the discount is no longer redeemable.
        max_redemptions:
          anyOf:
            - type: integer
              maximum: 2147483647
              minimum: 1
            - type: 'null'
          title: Max Redemptions
          description: Optional maximum number of times the discount can be redeemed.
        max_redemptions_per_customer:
          anyOf:
            - type: integer
              maximum: 2147483647
              minimum: 1
            - type: 'null'
          title: Max Redemptions Per Customer
          description: >-
            Optional maximum number of times the discount can be redeemed by a
            single customer.
        products:
          anyOf:
            - items:
                type: string
                format: uuid4
              type: array
              description: List of product IDs the discount can be applied to.
            - type: 'null'
          title: Products
        organization_id:
          anyOf:
            - type: string
              format: uuid4
              description: The organization ID.
              examples:
                - 1dbfc517-0bbf-4301-9ba8-555ca42b9737
            - type: 'null'
          title: Organization Id
          description: >-
            The ID of the organization owning the discount. **Required unless
            you use an organization token.**
        type:
          type: string
          const: percentage
          title: Type
          default: percentage
        duration:
          $ref: '#/components/schemas/DiscountDuration'
          description: >-
            For subscriptions, determines if the discount should be applied once
            on the first invoice, forever, or for a certain number of months
            determined by `duration_in_months`.
        duration_in_months:
          anyOf:
            - type: integer
              maximum: 999
              minimum: 1
            - type: 'null'
          title: Duration In Months
          description: |-
            Number of months the discount should be applied.

            Required when `duration` is `repeating`. Must be omitted otherwise.

            For this to work on yearly pricing, you should multiply this by 12.
            For example, to apply the discount for 2 years, set this to 24.
        basis_points:
          type: integer
          maximum: 10000
          minimum: 1
          title: Basis Points
          description: |-
            Discount percentage in basis points.

            A basis point is 1/100th of a percent.
            For example, to create a 25.5% discount, set this to 2550.
      type: object
      required:
        - name
        - duration
        - basis_points
      title: DiscountPercentageCreate
      description: Schema to create a percentage discount.
    DiscountFixedOnceForeverDuration:
      properties:
        duration:
          $ref: '#/components/schemas/DiscountDuration'
        type:
          $ref: '#/components/schemas/DiscountType'
        amount:
          type: integer
          title: Amount
          deprecated: true
          examples:
            - 1000
        currency:
          type: string
          title: Currency
          deprecated: true
          examples:
            - usd
        amounts:
          additionalProperties:
            type: integer
          type: object
          title: Amounts
          description: Map of currency to fixed amount to discount from the total.
          examples:
            - eur: 900
              usd: 1000
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
        name:
          type: string
          title: Name
          description: >-
            Name of the discount. Will be displayed to the customer when the
            discount is applied.
        code:
          anyOf:
            - type: string
            - type: 'null'
          title: Code
          description: Code customers can use to apply the discount during checkout.
        starts_at:
          anyOf:
            - type: string
              format: date-time
              examples:
                - '2026-01-01T00:00:00.000000Z'
            - type: 'null'
          title: Starts At
          description: Timestamp after which the discount is redeemable.
        ends_at:
          anyOf:
            - type: string
              format: date-time
              examples:
                - '2026-01-01T00:00:00.000000Z'
            - type: 'null'
          title: Ends At
          description: Timestamp after which the discount is no longer redeemable.
        max_redemptions:
          anyOf:
            - type: integer
            - type: 'null'
          title: Max Redemptions
          description: Maximum number of times the discount can be redeemed.
        max_redemptions_per_customer:
          anyOf:
            - type: integer
            - type: 'null'
          title: Max Redemptions Per Customer
          description: >-
            Maximum number of times the discount can be redeemed by a single
            customer.
        redemptions_count:
          type: integer
          title: Redemptions Count
          description: Number of times the discount has been redeemed.
        organization_id:
          type: string
          format: uuid4
          title: Organization Id
          description: The organization ID.
          examples:
            - 1dbfc517-0bbf-4301-9ba8-555ca42b9737
        products:
          items:
            $ref: '#/components/schemas/DiscountProduct'
          type: array
          title: Products
      type: object
      required:
        - duration
        - type
        - amount
        - currency
        - amounts
        - created_at
        - modified_at
        - id
        - metadata
        - name
        - code
        - starts_at
        - ends_at
        - max_redemptions
        - max_redemptions_per_customer
        - redemptions_count
        - organization_id
        - products
      title: DiscountFixedOnceForeverDuration
      description: Schema for a fixed amount discount that is applied once or forever.
    DiscountFixedRepeatDuration:
      properties:
        duration:
          $ref: '#/components/schemas/DiscountDuration'
        duration_in_months:
          type: integer
          title: Duration In Months
        type:
          $ref: '#/components/schemas/DiscountType'
        amount:
          type: integer
          title: Amount
          deprecated: true
          examples:
            - 1000
        currency:
          type: string
          title: Currency
          deprecated: true
          examples:
            - usd
        amounts:
          additionalProperties:
            type: integer
          type: object
          title: Amounts
          description: Map of currency to fixed amount to discount from the total.
          examples:
            - eur: 900
              usd: 1000
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
        name:
          type: string
          title: Name
          description: >-
            Name of the discount. Will be displayed to the customer when the
            discount is applied.
        code:
          anyOf:
            - type: string
            - type: 'null'
          title: Code
          description: Code customers can use to apply the discount during checkout.
        starts_at:
          anyOf:
            - type: string
              format: date-time
              examples:
                - '2026-01-01T00:00:00.000000Z'
            - type: 'null'
          title: Starts At
          description: Timestamp after which the discount is redeemable.
        ends_at:
          anyOf:
            - type: string
              format: date-time
              examples:
                - '2026-01-01T00:00:00.000000Z'
            - type: 'null'
          title: Ends At
          description: Timestamp after which the discount is no longer redeemable.
        max_redemptions:
          anyOf:
            - type: integer
            - type: 'null'
          title: Max Redemptions
          description: Maximum number of times the discount can be redeemed.
        max_redemptions_per_customer:
          anyOf:
            - type: integer
            - type: 'null'
          title: Max Redemptions Per Customer
          description: >-
            Maximum number of times the discount can be redeemed by a single
            customer.
        redemptions_count:
          type: integer
          title: Redemptions Count
          description: Number of times the discount has been redeemed.
        organization_id:
          type: string
          format: uuid4
          title: Organization Id
          description: The organization ID.
          examples:
            - 1dbfc517-0bbf-4301-9ba8-555ca42b9737
        products:
          items:
            $ref: '#/components/schemas/DiscountProduct'
          type: array
          title: Products
      type: object
      required:
        - duration
        - duration_in_months
        - type
        - amount
        - currency
        - amounts
        - created_at
        - modified_at
        - id
        - metadata
        - name
        - code
        - starts_at
        - ends_at
        - max_redemptions
        - max_redemptions_per_customer
        - redemptions_count
        - organization_id
        - products
      title: DiscountFixedRepeatDuration
      description: |-
        Schema for a fixed amount discount that is applied on every invoice
        for a certain number of months.
    DiscountPercentageOnceForeverDuration:
      properties:
        duration:
          $ref: '#/components/schemas/DiscountDuration'
        type:
          $ref: '#/components/schemas/DiscountType'
        basis_points:
          type: integer
          title: Basis Points
          description: >-
            Discount percentage in basis points. A basis point is 1/100th of a
            percent. For example, 1000 basis points equals a 10% discount.
          examples:
            - 1000
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
        name:
          type: string
          title: Name
          description: >-
            Name of the discount. Will be displayed to the customer when the
            discount is applied.
        code:
          anyOf:
            - type: string
            - type: 'null'
          title: Code
          description: Code customers can use to apply the discount during checkout.
        starts_at:
          anyOf:
            - type: string
              format: date-time
              examples:
                - '2026-01-01T00:00:00.000000Z'
            - type: 'null'
          title: Starts At
          description: Timestamp after which the discount is redeemable.
        ends_at:
          anyOf:
            - type: string
              format: date-time
              examples:
                - '2026-01-01T00:00:00.000000Z'
            - type: 'null'
          title: Ends At
          description: Timestamp after which the discount is no longer redeemable.
        max_redemptions:
          anyOf:
            - type: integer
            - type: 'null'
          title: Max Redemptions
          description: Maximum number of times the discount can be redeemed.
        max_redemptions_per_customer:
          anyOf:
            - type: integer
            - type: 'null'
          title: Max Redemptions Per Customer
          description: >-
            Maximum number of times the discount can be redeemed by a single
            customer.
        redemptions_count:
          type: integer
          title: Redemptions Count
          description: Number of times the discount has been redeemed.
        organization_id:
          type: string
          format: uuid4
          title: Organization Id
          description: The organization ID.
          examples:
            - 1dbfc517-0bbf-4301-9ba8-555ca42b9737
        products:
          items:
            $ref: '#/components/schemas/DiscountProduct'
          type: array
          title: Products
      type: object
      required:
        - duration
        - type
        - basis_points
        - created_at
        - modified_at
        - id
        - metadata
        - name
        - code
        - starts_at
        - ends_at
        - max_redemptions
        - max_redemptions_per_customer
        - redemptions_count
        - organization_id
        - products
      title: DiscountPercentageOnceForeverDuration
      description: Schema for a percentage discount that is applied once or forever.
    DiscountPercentageRepeatDuration:
      properties:
        duration:
          $ref: '#/components/schemas/DiscountDuration'
        duration_in_months:
          type: integer
          title: Duration In Months
        type:
          $ref: '#/components/schemas/DiscountType'
        basis_points:
          type: integer
          title: Basis Points
          description: >-
            Discount percentage in basis points. A basis point is 1/100th of a
            percent. For example, 1000 basis points equals a 10% discount.
          examples:
            - 1000
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
        name:
          type: string
          title: Name
          description: >-
            Name of the discount. Will be displayed to the customer when the
            discount is applied.
        code:
          anyOf:
            - type: string
            - type: 'null'
          title: Code
          description: Code customers can use to apply the discount during checkout.
        starts_at:
          anyOf:
            - type: string
              format: date-time
              examples:
                - '2026-01-01T00:00:00.000000Z'
            - type: 'null'
          title: Starts At
          description: Timestamp after which the discount is redeemable.
        ends_at:
          anyOf:
            - type: string
              format: date-time
              examples:
                - '2026-01-01T00:00:00.000000Z'
            - type: 'null'
          title: Ends At
          description: Timestamp after which the discount is no longer redeemable.
        max_redemptions:
          anyOf:
            - type: integer
            - type: 'null'
          title: Max Redemptions
          description: Maximum number of times the discount can be redeemed.
        max_redemptions_per_customer:
          anyOf:
            - type: integer
            - type: 'null'
          title: Max Redemptions Per Customer
          description: >-
            Maximum number of times the discount can be redeemed by a single
            customer.
        redemptions_count:
          type: integer
          title: Redemptions Count
          description: Number of times the discount has been redeemed.
        organization_id:
          type: string
          format: uuid4
          title: Organization Id
          description: The organization ID.
          examples:
            - 1dbfc517-0bbf-4301-9ba8-555ca42b9737
        products:
          items:
            $ref: '#/components/schemas/DiscountProduct'
          type: array
          title: Products
      type: object
      required:
        - duration
        - duration_in_months
        - type
        - basis_points
        - created_at
        - modified_at
        - id
        - metadata
        - name
        - code
        - starts_at
        - ends_at
        - max_redemptions
        - max_redemptions_per_customer
        - redemptions_count
        - organization_id
        - products
      title: DiscountPercentageRepeatDuration
      description: |-
        Schema for a percentage discount that is applied on every invoice
        for a certain number of months.
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
    DiscountDuration:
      type: string
      enum:
        - once
        - forever
        - repeating
      title: DiscountDuration
    PresentmentCurrency:
      type: string
      enum:
        - aed
        - all
        - amd
        - aoa
        - ars
        - aud
        - awg
        - azn
        - bam
        - bbd
        - bdt
        - bif
        - bmd
        - bnd
        - bob
        - brl
        - bsd
        - bwp
        - bzd
        - cad
        - cdf
        - chf
        - clp
        - cny
        - cop
        - crc
        - cve
        - czk
        - djf
        - dkk
        - dop
        - dzd
        - egp
        - etb
        - eur
        - fjd
        - fkp
        - gbp
        - gel
        - gip
        - gmd
        - gnf
        - gtq
        - gyd
        - hkd
        - hnl
        - htg
        - huf
        - idr
        - ils
        - inr
        - isk
        - jmd
        - jpy
        - kes
        - kgs
        - khr
        - kmf
        - krw
        - kyd
        - kzt
        - lak
        - lkr
        - lrd
        - lsl
        - mad
        - mdl
        - mga
        - mkd
        - mnt
        - mop
        - mur
        - mvr
        - mwk
        - mxn
        - myr
        - mzn
        - nad
        - ngn
        - nio
        - nok
        - npr
        - nzd
        - pab
        - pen
        - pgk
        - php
        - pkr
        - pln
        - pyg
        - qar
        - ron
        - rsd
        - rwf
        - sar
        - sbd
        - scr
        - sek
        - sgd
        - shp
        - sos
        - srd
        - szl
        - thb
        - tjs
        - top
        - try
        - ttd
        - twd
        - tzs
        - uah
        - ugx
        - usd
        - uyu
        - uzs
        - vnd
        - vuv
        - wst
        - xaf
        - xcd
        - xcg
        - xof
        - xpf
        - yer
        - zar
        - zmw
      title: PresentmentCurrency
    DiscountType:
      type: string
      enum:
        - fixed
        - percentage
      title: DiscountType
    MetadataOutputType:
      additionalProperties:
        anyOf:
          - type: string
          - type: integer
          - type: number
          - type: boolean
      type: object
    DiscountProduct:
      properties:
        metadata:
          $ref: '#/components/schemas/MetadataOutputType'
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
        trial_interval:
          anyOf:
            - $ref: '#/components/schemas/TrialInterval'
            - type: 'null'
          description: The interval unit for the trial period.
        trial_interval_count:
          anyOf:
            - type: integer
            - type: 'null'
          title: Trial Interval Count
          description: The number of interval units for the trial period.
        name:
          type: string
          title: Name
          description: The name of the product.
        description:
          anyOf:
            - type: string
            - type: 'null'
          title: Description
          description: The description of the product.
        visibility:
          $ref: '#/components/schemas/ProductVisibility'
          description: The visibility of the product.
        recurring_interval:
          anyOf:
            - $ref: '#/components/schemas/RecurringInterval'
            - type: 'null'
          description: >-
            The recurring interval of the product. If `None`, the product is a
            one-time purchase.
        recurring_interval_count:
          anyOf:
            - type: integer
            - type: 'null'
          title: Recurring Interval Count
          description: >-
            Number of interval units of the subscription. If this is set to 1
            the charge will happen every interval (e.g. every month), if set to
            2 it will be every other month, and so on. None for one-time
            products.
        meter_interval:
          anyOf:
            - $ref: '#/components/schemas/RecurringInterval'
            - type: 'null'
          description: >-
            The meter cycle of the product, independent of the billing interval.
            If `None`, metered concerns follow the billing interval.
        meter_interval_count:
          anyOf:
            - type: integer
            - type: 'null'
          title: Meter Interval Count
          description: Number of meter interval units. None when no meter cycle is set.
        is_recurring:
          type: boolean
          title: Is Recurring
          description: Whether the product is a subscription.
        is_archived:
          type: boolean
          title: Is Archived
          description: Whether the product is archived and no longer available.
        organization_id:
          type: string
          format: uuid4
          title: Organization Id
          description: The ID of the organization owning the product.
      type: object
      required:
        - metadata
        - id
        - created_at
        - modified_at
        - trial_interval
        - trial_interval_count
        - name
        - description
        - visibility
        - recurring_interval
        - recurring_interval_count
        - meter_interval
        - meter_interval_count
        - is_recurring
        - is_archived
        - organization_id
      title: DiscountProduct
      description: A product that a discount can be applied to.
    TrialInterval:
      type: string
      enum:
        - day
        - week
        - month
        - year
      title: TrialInterval
    ProductVisibility:
      type: string
      enum:
        - draft
        - private
        - public
      title: Visibility
    RecurringInterval:
      type: string
      enum:
        - day
        - week
        - month
        - year
      title: RecurringInterval
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
