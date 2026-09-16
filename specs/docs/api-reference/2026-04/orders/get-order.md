> ## Documentation Index
> Fetch the complete documentation index at: https://polar.sh/docs/llms.txt
> Use this file to discover all available pages before exploring further.

# Get Order

> Get an order by ID.

**Scopes**: `orders:read`



## OpenAPI

````yaml /openapi/2026-04.openapi.json get /v1/orders/{id}
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
  /v1/orders/{id}:
    get:
      tags:
        - orders
        - public
        - mcp
        - cli
      summary: Get Order
      description: |-
        Get an order by ID.

        **Scopes**: `orders:read`
      operationId: orders:get
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
                $ref: '#/components/schemas/Order'
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
        - oidc:
            - orders:read
        - pat:
            - orders:read
        - oat:
            - orders:read
      x-codeSamples:
        - lang: python
          source: |
            from polar.v2026_04 import Polar

            polar = Polar("polar_oat_xxx")

            response = polar.orders.get(
                '00000000-0000-4000-8000-000000000000',
            )
            print(response)
        - lang: typescript
          source: |
            import { createPolar } from "@polar-sh/sdk/2026-04";

            const polar = createPolar({
              accessToken: "polar_oat_xxx",
            });

            const response = await polar.orders.get(
              "00000000-0000-4000-8000-000000000000",
            );
            console.log(response);
components:
  schemas:
    Order:
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
        status:
          $ref: '#/components/schemas/OrderStatus'
          examples:
            - paid
        paid:
          type: boolean
          title: Paid
          description: Whether the order has been paid for.
          examples:
            - true
        subtotal_amount:
          type: integer
          title: Subtotal Amount
          description: Amount in cents, before discounts and taxes.
          examples:
            - 10000
        discount_amount:
          type: integer
          title: Discount Amount
          description: Discount amount in cents.
          examples:
            - 1000
        net_amount:
          type: integer
          title: Net Amount
          description: Amount in cents, after discounts but before taxes.
          examples:
            - 9000
        tax_amount:
          type: integer
          title: Tax Amount
          description: Sales tax amount in cents.
          examples:
            - 720
        total_amount:
          type: integer
          title: Total Amount
          description: Amount in cents, after discounts and taxes.
          examples:
            - 9720
        applied_balance_amount:
          type: integer
          title: Applied Balance Amount
          description: >-
            Customer's balance amount applied to this invoice. Can increase the
            total amount paid, if the customer has a negative balance,  or
            decrease it, if the customer has a positive balance.Amount in cents.
          examples:
            - 0
        due_amount:
          type: integer
          title: Due Amount
          description: Amount in cents that is due for this order.
          examples:
            - 0
        refunded_amount:
          type: integer
          title: Refunded Amount
          description: Amount refunded in cents.
          examples:
            - 0
        refunded_tax_amount:
          type: integer
          title: Refunded Tax Amount
          description: Sales tax refunded in cents.
          examples:
            - 0
        currency:
          type: string
          title: Currency
          examples:
            - usd
        billing_reason:
          $ref: '#/components/schemas/OrderBillingReason'
        billing_name:
          anyOf:
            - type: string
            - type: 'null'
          title: Billing Name
          description: 'The name of the customer that should appear on the invoice. '
        billing_address:
          anyOf:
            - $ref: '#/components/schemas/Address'
            - type: 'null'
        invoice_number:
          anyOf:
            - type: string
            - type: 'null'
          title: Invoice Number
          description: >-
            The invoice number associated with this order. `null` while the
            order is in `draft` status; assigned at finalize.
        is_invoice_generated:
          type: boolean
          title: Is Invoice Generated
          description: Whether an invoice has been generated for this order.
        receipt_number:
          anyOf:
            - type: string
            - type: 'null'
          title: Receipt Number
          description: >-
            The receipt number for this order. Set once the order is paid for
            organizations with receipts enabled. When set, a downloadable
            receipt PDF can be obtained via the receipt endpoint.
        seats:
          anyOf:
            - type: integer
            - type: 'null'
          title: Seats
          description: Number of seats purchased (for seat-based one-time orders).
        units:
          anyOf:
            - type: integer
            - type: 'null'
          title: Units
          description: Number of units purchased (for unit-based pricing).
        customer_id:
          type: string
          format: uuid4
          title: Customer Id
        product_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Product Id
        discount_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Discount Id
        subscription_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Subscription Id
        checkout_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Checkout Id
        next_payment_attempt_at:
          anyOf:
            - type: string
              format: date-time
              examples:
                - '2026-01-01T00:00:00.000000Z'
            - type: 'null'
          title: Next Payment Attempt At
          description: >-
            When the next automatic payment retry is scheduled. `null` if the
            order is not in dunning or all retries have been exhausted.
        metadata:
          $ref: '#/components/schemas/MetadataOutputType'
        custom_field_data:
          additionalProperties:
            anyOf:
              - type: string
              - type: integer
              - type: boolean
              - type: string
                format: date-time
                examples:
                  - '2026-01-01T00:00:00.000000Z'
              - type: 'null'
          type: object
          title: Custom Field Data
          description: Key-value object storing custom field values.
        platform_fee_amount:
          type: integer
          title: Platform Fee Amount
          description: Platform fee amount in cents.
          examples:
            - 500
        platform_fee_currency:
          anyOf:
            - type: string
            - type: 'null'
          title: Platform Fee Currency
          description: Currency of the platform fee.
          examples:
            - usd
        customer:
          $ref: '#/components/schemas/OrderCustomer'
        product:
          anyOf:
            - $ref: '#/components/schemas/OrderProduct'
            - type: 'null'
        discount:
          anyOf:
            - oneOf:
                - $ref: '#/components/schemas/DiscountFixedOnceForeverDurationBase'
                - $ref: '#/components/schemas/DiscountFixedRepeatDurationBase'
                - $ref: >-
                    #/components/schemas/DiscountPercentageOnceForeverDurationBase
                - $ref: '#/components/schemas/DiscountPercentageRepeatDurationBase'
              title: OrderDiscount
            - type: 'null'
          title: Discount
        subscription:
          anyOf:
            - $ref: '#/components/schemas/OrderSubscription'
            - type: 'null'
        items:
          items:
            $ref: '#/components/schemas/OrderItemSchema'
          type: array
          title: Items
          description: Line items composing the order.
        description:
          type: string
          title: Description
          description: A summary description of the order.
          examples:
            - Pro Plan
        refundable_amount:
          type: integer
          title: Refundable Amount
          description: >-
            Amount in cents that can still be refunded (net, before taxes).
            Accounts for any applied customer balance and previous refunds.
          readOnly: true
          examples:
            - 9000
        refundable_tax_amount:
          type: integer
          title: Refundable Tax Amount
          description: >-
            Sales tax in cents that would be refunded if the full refundable
            amount is refunded.
          readOnly: true
          examples:
            - 720
      type: object
      required:
        - id
        - created_at
        - modified_at
        - status
        - paid
        - subtotal_amount
        - discount_amount
        - net_amount
        - tax_amount
        - total_amount
        - applied_balance_amount
        - due_amount
        - refunded_amount
        - refunded_tax_amount
        - currency
        - billing_reason
        - billing_name
        - billing_address
        - invoice_number
        - is_invoice_generated
        - receipt_number
        - units
        - customer_id
        - product_id
        - discount_id
        - subscription_id
        - checkout_id
        - metadata
        - platform_fee_amount
        - platform_fee_currency
        - customer
        - product
        - discount
        - subscription
        - items
        - description
        - refundable_amount
        - refundable_tax_amount
      title: Order
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
    OrderStatus:
      type: string
      enum:
        - draft
        - pending
        - paid
        - refunded
        - partially_refunded
        - void
      title: OrderStatus
    OrderBillingReason:
      type: string
      enum:
        - purchase
        - subscription_create
        - subscription_cycle
        - subscription_update
        - subscription_meter_cycle
      title: OrderBillingReason
    Address:
      properties:
        line1:
          anyOf:
            - type: string
            - type: 'null'
          title: Line1
        line2:
          anyOf:
            - type: string
            - type: 'null'
          title: Line2
        postal_code:
          anyOf:
            - type: string
            - type: 'null'
          title: Postal Code
        city:
          anyOf:
            - type: string
            - type: 'null'
          title: City
        state:
          anyOf:
            - type: string
            - type: 'null'
          title: State
        country:
          type: string
          enum:
            - AD
            - AE
            - AF
            - AG
            - AI
            - AL
            - AM
            - AO
            - AQ
            - AR
            - AS
            - AT
            - AU
            - AW
            - AX
            - AZ
            - BA
            - BB
            - BD
            - BE
            - BF
            - BG
            - BH
            - BI
            - BJ
            - BL
            - BM
            - BN
            - BO
            - BQ
            - BR
            - BS
            - BT
            - BV
            - BW
            - BY
            - BZ
            - CA
            - CC
            - CD
            - CF
            - CG
            - CH
            - CI
            - CK
            - CL
            - CM
            - CN
            - CO
            - CR
            - CU
            - CV
            - CW
            - CX
            - CY
            - CZ
            - DE
            - DJ
            - DK
            - DM
            - DO
            - DZ
            - EC
            - EE
            - EG
            - EH
            - ER
            - ES
            - ET
            - FI
            - FJ
            - FK
            - FM
            - FO
            - FR
            - GA
            - GB
            - GD
            - GE
            - GF
            - GG
            - GH
            - GI
            - GL
            - GM
            - GN
            - GP
            - GQ
            - GR
            - GS
            - GT
            - GU
            - GW
            - GY
            - HK
            - HM
            - HN
            - HR
            - HT
            - HU
            - ID
            - IE
            - IL
            - IM
            - IN
            - IO
            - IQ
            - IR
            - IS
            - IT
            - JE
            - JM
            - JO
            - JP
            - KE
            - KG
            - KH
            - KI
            - KM
            - KN
            - KP
            - KR
            - KW
            - KY
            - KZ
            - LA
            - LB
            - LC
            - LI
            - LK
            - LR
            - LS
            - LT
            - LU
            - LV
            - LY
            - MA
            - MC
            - MD
            - ME
            - MF
            - MG
            - MH
            - MK
            - ML
            - MM
            - MN
            - MO
            - MP
            - MQ
            - MR
            - MS
            - MT
            - MU
            - MV
            - MW
            - MX
            - MY
            - MZ
            - NA
            - NC
            - NE
            - NF
            - NG
            - NI
            - NL
            - 'NO'
            - NP
            - NR
            - NU
            - NZ
            - OM
            - PA
            - PE
            - PF
            - PG
            - PH
            - PK
            - PL
            - PM
            - PN
            - PR
            - PS
            - PT
            - PW
            - PY
            - QA
            - RE
            - RO
            - RS
            - RU
            - RW
            - SA
            - SB
            - SC
            - SD
            - SE
            - SG
            - SH
            - SI
            - SJ
            - SK
            - SL
            - SM
            - SN
            - SO
            - SR
            - SS
            - ST
            - SV
            - SX
            - SY
            - SZ
            - TC
            - TD
            - TF
            - TG
            - TH
            - TJ
            - TK
            - TL
            - TM
            - TN
            - TO
            - TR
            - TT
            - TV
            - TW
            - TZ
            - UA
            - UG
            - UM
            - US
            - UY
            - UZ
            - VA
            - VC
            - VE
            - VG
            - VI
            - VN
            - VU
            - WF
            - WS
            - YE
            - YT
            - ZA
            - ZM
            - ZW
          title: CountryAlpha2
          examples:
            - US
            - SE
            - FR
          x-speakeasy-enums:
            - AD
            - AE
            - AF
            - AG
            - AI
            - AL
            - AM
            - AO
            - AQ
            - AR
            - AS
            - AT
            - AU
            - AW
            - AX
            - AZ
            - BA
            - BB
            - BD
            - BE
            - BF
            - BG
            - BH
            - BI
            - BJ
            - BL
            - BM
            - BN
            - BO
            - BQ
            - BR
            - BS
            - BT
            - BV
            - BW
            - BY
            - BZ
            - CA
            - CC
            - CD
            - CF
            - CG
            - CH
            - CI
            - CK
            - CL
            - CM
            - CN
            - CO
            - CR
            - CU
            - CV
            - CW
            - CX
            - CY
            - CZ
            - DE
            - DJ
            - DK
            - DM
            - DO
            - DZ
            - EC
            - EE
            - EG
            - EH
            - ER
            - ES
            - ET
            - FI
            - FJ
            - FK
            - FM
            - FO
            - FR
            - GA
            - GB
            - GD
            - GE
            - GF
            - GG
            - GH
            - GI
            - GL
            - GM
            - GN
            - GP
            - GQ
            - GR
            - GS
            - GT
            - GU
            - GW
            - GY
            - HK
            - HM
            - HN
            - HR
            - HT
            - HU
            - ID
            - IE
            - IL
            - IM
            - IN
            - IO
            - IQ
            - IR
            - IS
            - IT
            - JE
            - JM
            - JO
            - JP
            - KE
            - KG
            - KH
            - KI
            - KM
            - KN
            - KP
            - KR
            - KW
            - KY
            - KZ
            - LA
            - LB
            - LC
            - LI
            - LK
            - LR
            - LS
            - LT
            - LU
            - LV
            - LY
            - MA
            - MC
            - MD
            - ME
            - MF
            - MG
            - MH
            - MK
            - ML
            - MM
            - MN
            - MO
            - MP
            - MQ
            - MR
            - MS
            - MT
            - MU
            - MV
            - MW
            - MX
            - MY
            - MZ
            - NA
            - NC
            - NE
            - NF
            - NG
            - NI
            - NL
            - 'NO'
            - NP
            - NR
            - NU
            - NZ
            - OM
            - PA
            - PE
            - PF
            - PG
            - PH
            - PK
            - PL
            - PM
            - PN
            - PR
            - PS
            - PT
            - PW
            - PY
            - QA
            - RE
            - RO
            - RS
            - RU
            - RW
            - SA
            - SB
            - SC
            - SD
            - SE
            - SG
            - SH
            - SI
            - SJ
            - SK
            - SL
            - SM
            - SN
            - SO
            - SR
            - SS
            - ST
            - SV
            - SX
            - SY
            - SZ
            - TC
            - TD
            - TF
            - TG
            - TH
            - TJ
            - TK
            - TL
            - TM
            - TN
            - TO
            - TR
            - TT
            - TV
            - TW
            - TZ
            - UA
            - UG
            - UM
            - US
            - UY
            - UZ
            - VA
            - VC
            - VE
            - VG
            - VI
            - VN
            - VU
            - WF
            - WS
            - YE
            - YT
            - ZA
            - ZM
            - ZW
      type: object
      required:
        - country
      title: Address
    MetadataOutputType:
      additionalProperties:
        anyOf:
          - type: string
          - type: integer
          - type: number
          - type: boolean
      type: object
    OrderCustomer:
      properties:
        id:
          type: string
          format: uuid4
          title: Id
          description: The ID of the customer.
          examples:
            - 992fae2a-2a17-4b7a-8d9e-e287cf90131b
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
        metadata:
          $ref: '#/components/schemas/MetadataOutputType'
        external_id:
          anyOf:
            - type: string
            - type: 'null'
          title: External Id
          description: >-
            The ID of the customer in your system. This must be unique within
            the organization. Once set, it can't be updated.
          examples:
            - usr_1337
        email:
          anyOf:
            - type: string
            - type: 'null'
          title: Email
          description: >-
            The email address of the customer. This must be unique within the
            organization.
          examples:
            - customer@example.com
        email_verified:
          type: boolean
          title: Email Verified
          description: >-
            Whether the customer email address is verified. The address is
            automatically verified when the customer accesses the customer
            portal using their email address.
          examples:
            - true
        type:
          $ref: '#/components/schemas/CustomerType'
          description: >-
            The type of customer: 'individual' for single users, 'team' for
            customers with multiple members.
          examples:
            - individual
        name:
          anyOf:
            - type: string
            - type: 'null'
          title: Name
          description: The name of the customer.
          examples:
            - John Doe
        billing_name:
          anyOf:
            - type: string
            - type: 'null'
          title: Billing Name
          description: >-
            The name that should appear on the customer's invoices. Falls back
            to the customer name when not explicitly set.
          examples:
            - John Doe
        billing_address:
          anyOf:
            - $ref: '#/components/schemas/Address'
            - type: 'null'
        tax_id:
          anyOf:
            - prefixItems:
                - type: string
                - $ref: '#/components/schemas/TaxIDFormat'
              type: array
              maxItems: 2
              minItems: 2
              examples:
                - - '911144442'
                  - us_ein
                - - FR61954506077
                  - eu_vat
            - type: 'null'
          title: Tax Id
        locale:
          anyOf:
            - type: string
            - type: 'null'
          title: Locale
        organization_id:
          type: string
          format: uuid4
          title: Organization Id
          description: The ID of the organization owning the customer.
          examples:
            - 1dbfc517-0bbf-4301-9ba8-555ca42b9737
        default_payment_method_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Default Payment Method Id
          description: >-
            The ID of the customer's default payment method, if any. Use the
            payment methods endpoint to retrieve its details.
        deleted_at:
          anyOf:
            - type: string
              format: date-time
              examples:
                - '2026-01-01T00:00:00.000000Z'
            - type: 'null'
          title: Deleted At
          description: Timestamp for when the customer was soft deleted.
        first_user_event_at:
          anyOf:
            - type: string
              format: date-time
              examples:
                - '2026-01-01T00:00:00.000000Z'
            - type: 'null'
          title: First User Event At
          description: >-
            Timestamp of the first event ingested for this customer. Can predate
            `created_at`, and is null if no event was ever ingested.
        avatar_url:
          anyOf:
            - type: string
            - type: 'null'
          title: Avatar Url
          examples:
            - https://www.gravatar.com/avatar/xxx?d=404
      type: object
      required:
        - id
        - created_at
        - modified_at
        - metadata
        - email_verified
        - type
        - name
        - billing_name
        - billing_address
        - tax_id
        - organization_id
        - deleted_at
        - first_user_event_at
        - avatar_url
      title: OrderCustomer
    OrderProduct:
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
      title: OrderProduct
    DiscountFixedOnceForeverDurationBase:
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
      title: DiscountFixedOnceForeverDurationBase
    DiscountFixedRepeatDurationBase:
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
      title: DiscountFixedRepeatDurationBase
    DiscountPercentageOnceForeverDurationBase:
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
      title: DiscountPercentageOnceForeverDurationBase
    DiscountPercentageRepeatDurationBase:
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
      title: DiscountPercentageRepeatDurationBase
    OrderSubscription:
      properties:
        metadata:
          $ref: '#/components/schemas/MetadataOutputType'
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
        amount:
          type: integer
          title: Amount
          description: The amount of the subscription.
          examples:
            - 10000
        currency:
          type: string
          title: Currency
          description: The currency of the subscription.
          examples:
            - usd
        recurring_interval:
          $ref: '#/components/schemas/RecurringInterval'
          description: The interval at which the subscription recurs.
          examples:
            - month
        recurring_interval_count:
          type: integer
          title: Recurring Interval Count
          description: >-
            Number of interval units of the subscription. If this is set to 1
            the charge will happen every interval (e.g. every month), if set to
            2 it will be every other month, and so on.
        status:
          $ref: '#/components/schemas/SubscriptionStatus'
          description: The status of the subscription.
          examples:
            - active
        current_period_start:
          type: string
          format: date-time
          title: Current Period Start
          description: The start timestamp of the current billing period.
          examples:
            - '2026-01-01T00:00:00.000000Z'
        current_period_end:
          type: string
          format: date-time
          title: Current Period End
          description: The end timestamp of the current billing period.
          examples:
            - '2026-01-01T00:00:00.000000Z'
        current_meter_period_start:
          anyOf:
            - type: string
              format: date-time
              examples:
                - '2026-01-01T00:00:00.000000Z'
            - type: 'null'
          title: Current Meter Period Start
          description: >-
            The start timestamp of the current meter period, if the product has
            a meter cycle set. Metered credits are granted and overage is
            settled on this cadence.
        current_meter_period_end:
          anyOf:
            - type: string
              format: date-time
              examples:
                - '2026-01-01T00:00:00.000000Z'
            - type: 'null'
          title: Current Meter Period End
          description: >-
            The end timestamp of the current meter period, if the product has a
            meter cycle set. This is when credits next renew.
        trial_start:
          anyOf:
            - type: string
              format: date-time
              examples:
                - '2026-01-01T00:00:00.000000Z'
            - type: 'null'
          title: Trial Start
          description: The start timestamp of the trial period, if any.
        trial_end:
          anyOf:
            - type: string
              format: date-time
              examples:
                - '2026-01-01T00:00:00.000000Z'
            - type: 'null'
          title: Trial End
          description: The end timestamp of the trial period, if any.
        cancel_at_period_end:
          type: boolean
          title: Cancel At Period End
          description: >-
            Whether the subscription will be canceled at the end of the current
            period.
        canceled_at:
          anyOf:
            - type: string
              format: date-time
              examples:
                - '2026-01-01T00:00:00.000000Z'
            - type: 'null'
          title: Canceled At
          description: >-
            The timestamp when the subscription was canceled. The subscription
            might still be active if `cancel_at_period_end` is `true`.
        started_at:
          anyOf:
            - type: string
              format: date-time
              examples:
                - '2026-01-01T00:00:00.000000Z'
            - type: 'null'
          title: Started At
          description: The timestamp when the subscription started.
        ends_at:
          anyOf:
            - type: string
              format: date-time
              examples:
                - '2026-01-01T00:00:00.000000Z'
            - type: 'null'
          title: Ends At
          description: The timestamp when the subscription will end.
        ended_at:
          anyOf:
            - type: string
              format: date-time
              examples:
                - '2026-01-01T00:00:00.000000Z'
            - type: 'null'
          title: Ended At
          description: The timestamp when the subscription ended.
        past_due_at:
          anyOf:
            - type: string
              format: date-time
              examples:
                - '2026-01-01T00:00:00.000000Z'
            - type: 'null'
          title: Past Due At
          description: The timestamp when the subscription entered `past_due` status.
        pause_at_period_end:
          type: boolean
          title: Pause At Period End
          description: >-
            Whether the subscription will be paused at the end of the current
            period.
        paused_at:
          anyOf:
            - type: string
              format: date-time
              examples:
                - '2026-01-01T00:00:00.000000Z'
            - type: 'null'
          title: Paused At
          description: The timestamp when the subscription was paused.
        resumes_at:
          anyOf:
            - type: string
              format: date-time
              examples:
                - '2026-01-01T00:00:00.000000Z'
            - type: 'null'
          title: Resumes At
          description: >-
            The timestamp when a paused subscription is scheduled to
            automatically resume, if set.
        customer_id:
          type: string
          format: uuid4
          title: Customer Id
          description: The ID of the subscribed customer.
        product_id:
          type: string
          format: uuid4
          title: Product Id
          description: The ID of the subscribed product.
        discount_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Discount Id
          description: The ID of the applied discount, if any.
        checkout_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Checkout Id
        seats:
          anyOf:
            - type: integer
            - type: 'null'
          title: Seats
          description: >-
            The number of seats for seat-based subscriptions. None for non-seat
            subscriptions.
        units:
          anyOf:
            - type: integer
            - type: 'null'
          title: Units
          description: >-
            The number of units for unit-based subscriptions. None for non-unit
            subscriptions.
        customer_cancellation_reason:
          anyOf:
            - $ref: '#/components/schemas/CustomerCancellationReason'
            - type: 'null'
        customer_cancellation_comment:
          anyOf:
            - type: string
            - type: 'null'
          title: Customer Cancellation Comment
      type: object
      required:
        - metadata
        - created_at
        - modified_at
        - id
        - amount
        - currency
        - recurring_interval
        - recurring_interval_count
        - status
        - current_period_start
        - current_period_end
        - current_meter_period_start
        - current_meter_period_end
        - trial_start
        - trial_end
        - cancel_at_period_end
        - canceled_at
        - started_at
        - ends_at
        - ended_at
        - pause_at_period_end
        - paused_at
        - resumes_at
        - customer_id
        - product_id
        - discount_id
        - checkout_id
        - units
        - customer_cancellation_reason
        - customer_cancellation_comment
      title: OrderSubscription
    OrderItemSchema:
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
        label:
          type: string
          title: Label
          description: Description of the line item charge.
          examples:
            - Pro Plan
        amount:
          type: integer
          title: Amount
          description: Amount in cents, before discounts and taxes.
          examples:
            - 10000
        tax_amount:
          type: integer
          title: Tax Amount
          description: Sales tax amount in cents.
          examples:
            - 720
        proration:
          type: boolean
          title: Proration
          description: Whether this charge is due to a proration.
          examples:
            - false
        product_price_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Product Price Id
          description: Associated price ID, if any.
        start_timestamp:
          anyOf:
            - type: string
              format: date-time
              examples:
                - '2026-01-01T00:00:00.000000Z'
            - type: 'null'
          title: Start Timestamp
          description: Start of the period covered by this line item, if any.
        end_timestamp:
          anyOf:
            - type: string
              format: date-time
              examples:
                - '2026-01-01T00:00:00.000000Z'
            - type: 'null'
          title: End Timestamp
          description: End of the period covered by this line item, if any.
      type: object
      required:
        - created_at
        - modified_at
        - id
        - label
        - amount
        - tax_amount
        - proration
        - product_price_id
        - start_timestamp
        - end_timestamp
      title: OrderItemSchema
      description: An order line item.
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
    CustomerType:
      type: string
      enum:
        - individual
        - team
      title: CustomerType
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
    DiscountDuration:
      type: string
      enum:
        - once
        - forever
        - repeating
      title: DiscountDuration
    DiscountType:
      type: string
      enum:
        - fixed
        - percentage
      title: DiscountType
    SubscriptionStatus:
      type: string
      enum:
        - incomplete
        - incomplete_expired
        - trialing
        - active
        - past_due
        - canceled
        - unpaid
        - paused
      title: SubscriptionStatus
    CustomerCancellationReason:
      type: string
      enum:
        - customer_service
        - low_quality
        - missing_features
        - switched_service
        - too_complex
        - too_expensive
        - unused
        - other
      title: CustomerCancellationReason
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
