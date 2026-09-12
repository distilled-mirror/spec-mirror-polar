> ## Documentation Index
> Fetch the complete documentation index at: https://polar.sh/docs/llms.txt
> Use this file to discover all available pages before exploring further.

# Create Checkout Session

> Create a checkout session.

**Scopes**: `checkouts:write`



## OpenAPI

````yaml /openapi/2026-04.openapi.json post /v1/checkouts/
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
  /v1/checkouts/:
    post:
      tags:
        - checkouts
        - public
      summary: Create Checkout Session
      description: |-
        Create a checkout session.

        **Scopes**: `checkouts:write`
      operationId: checkouts:create
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/CheckoutCreate'
      responses:
        '201':
          description: Checkout session created.
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Checkout'
        '422':
          description: Validation Error
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/HTTPValidationError'
      security:
        - oidc:
            - checkouts:write
        - pat:
            - checkouts:write
        - oat:
            - checkouts:write
      x-codeSamples:
        - lang: python
          source: |
            from polar.v2026_04 import Polar

            polar = Polar("polar_oat_xxx")

            response = polar.checkouts.create(
                allow_discount_codes=True,
                require_billing_address=False,
                allow_trial=True,
                is_business_customer=False,
                products=['00000000-0000-4000-8000-000000000000'],
            )
            print(response)
        - lang: typescript
          source: |
            import { createPolar } from "@polar-sh/sdk/2026-04";

            const polar = createPolar({
              accessToken: "polar_oat_xxx",
            });

            const response = await polar.checkouts.create(
              {
                "allow_discount_codes": true,
                "require_billing_address": false,
                "allow_trial": true,
                "is_business_customer": false,
                "products": [
                  "00000000-0000-4000-8000-000000000000"
                ]
              },
            );
            console.log(response);
components:
  schemas:
    CheckoutCreate:
      $ref: '#/components/schemas/CheckoutProductsCreate'
    Checkout:
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
        payment_processor:
          $ref: '#/components/schemas/PaymentProcessor'
          description: Payment processor used.
        status:
          $ref: '#/components/schemas/CheckoutStatus'
          description: |2-

                    Status of the checkout session.

                    - Open: the checkout session was opened.
                    - Expired: the checkout session was expired and is no more accessible.
                    - Confirmed: the user on the checkout session clicked Pay. This is not indicative of the payment's success status.
                    - Failed: the checkout definitely failed for technical reasons and cannot be retried. In most cases, this state is never reached.
                    - Succeeded: the payment on the checkout was performed successfully.
                    
        client_secret:
          type: string
          title: Client Secret
          description: >-
            Client secret used to update and complete the checkout session from
            the client.
        url:
          type: string
          title: Url
          description: URL where the customer can access the checkout session.
        expires_at:
          type: string
          format: date-time
          title: Expires At
          description: Expiration date and time of the checkout session.
          examples:
            - '2026-01-01T00:00:00.000000Z'
        success_url:
          type: string
          title: Success Url
          description: >-
            URL where the customer will be redirected after a successful
            payment.
        return_url:
          anyOf:
            - type: string
            - type: 'null'
          title: Return Url
          description: >-
            When set, a back button will be shown in the checkout to return to
            this URL.
        embed_origin:
          anyOf:
            - type: string
            - type: 'null'
          title: Embed Origin
          description: >-
            When checkout is embedded, represents the Origin of the page
            embedding the checkout. Used as a security measure to send messages
            only to the embedding page.
        amount:
          type: integer
          title: Amount
          description: Amount in cents, before discounts and taxes.
        seats:
          anyOf:
            - type: integer
            - type: 'null'
          title: Seats
          description: Predefined number of seats (works with seat-based pricing only)
        min_seats:
          anyOf:
            - type: integer
            - type: 'null'
          title: Min Seats
          description: Minimum number of seats (works with seat-based pricing only)
        max_seats:
          anyOf:
            - type: integer
            - type: 'null'
          title: Max Seats
          description: Maximum number of seats (works with seat-based pricing only)
        units:
          anyOf:
            - type: integer
            - type: 'null'
          title: Units
          description: Predefined number of units (works with unit-based pricing only)
        min_units:
          anyOf:
            - type: integer
            - type: 'null'
          title: Min Units
          description: Minimum number of units (works with unit-based pricing only)
        max_units:
          anyOf:
            - type: integer
            - type: 'null'
          title: Max Units
          description: Maximum number of units (works with unit-based pricing only)
        discount_amount:
          type: integer
          title: Discount Amount
          description: Discount amount in cents.
        net_amount:
          type: integer
          title: Net Amount
          description: Amount in cents, after discounts but before taxes.
        tax_amount:
          anyOf:
            - type: integer
            - type: 'null'
          title: Tax Amount
          description: >-
            Sales tax amount in cents. If `null`, it means there is no enough
            information yet to calculate it.
        tax_behavior:
          anyOf:
            - $ref: '#/components/schemas/TaxBehavior'
            - type: 'null'
          description: >-
            Tax behavior of the checkout. `inclusive` means the price includes
            tax, `exclusive` means tax is added on top. If `null`, tax is not
            yet calculated.
        total_amount:
          type: integer
          title: Total Amount
          description: Amount in cents, after discounts and taxes.
        currency:
          type: string
          title: Currency
          description: Currency code of the checkout session.
        allow_trial:
          anyOf:
            - type: boolean
            - type: 'null'
          title: Allow Trial
          description: >-
            Whether to enable the trial period for the checkout session. If
            `false`, the trial period will be disabled, even if the selected
            product has a trial configured.
        active_trial_interval:
          anyOf:
            - $ref: '#/components/schemas/TrialInterval'
            - type: 'null'
          description: >-
            Interval unit of the trial period, if any. This value is either set
            from the checkout, if `trial_interval` is set, or from the selected
            product.
        active_trial_interval_count:
          anyOf:
            - type: integer
            - type: 'null'
          title: Active Trial Interval Count
          description: >-
            Number of interval units of the trial period, if any. This value is
            either set from the checkout, if `trial_interval_count` is set, or
            from the selected product.
        trial_end:
          anyOf:
            - type: string
              format: date-time
              examples:
                - '2026-01-01T00:00:00.000000Z'
            - type: 'null'
          title: Trial End
          description: End date and time of the trial period, if any.
        organization_id:
          type: string
          format: uuid4
          title: Organization Id
          description: ID of the organization owning the checkout session.
        product_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Product Id
          description: ID of the product to checkout.
        product_price_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Product Price Id
          description: ID of the product price to checkout.
          deprecated: true
        discount_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Discount Id
          description: ID of the discount applied to the checkout.
        allow_discount_codes:
          type: boolean
          title: Allow Discount Codes
          description: >-
            Whether to allow the customer to apply discount codes. If you apply
            a discount through `discount_id`, it'll still be applied, but the
            customer won't be able to change it.
        require_billing_address:
          type: boolean
          title: Require Billing Address
          description: >-
            Whether to require the customer to fill their full billing address,
            instead of just the country. Customers in the US will always be
            required to fill their full address, regardless of this setting. If
            you preset the billing address, this setting will be automatically
            set to `true`.
        is_discount_applicable:
          type: boolean
          title: Is Discount Applicable
          description: >-
            Whether the discount is applicable to the checkout. Typically, free
            and custom prices are not discountable.
        is_free_product_price:
          type: boolean
          title: Is Free Product Price
          description: Whether the product price is free, regardless of discounts.
        is_payment_required:
          type: boolean
          title: Is Payment Required
          description: >-
            Whether the checkout requires payment, e.g. in case of free products
            or discounts that cover the total amount.
        is_payment_setup_required:
          type: boolean
          title: Is Payment Setup Required
          description: >-
            Whether the checkout requires setting up a payment method,
            regardless of the amount, e.g. subscriptions that have first free
            cycles.
        is_payment_form_required:
          type: boolean
          title: Is Payment Form Required
          description: >-
            Whether the checkout requires a payment form, whether because of a
            payment or payment method setup.
        customer_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Customer Id
        is_business_customer:
          type: boolean
          title: Is Business Customer
          description: >-
            Whether the customer is a business or an individual. If `true`, the
            customer will be required to fill their full billing address and
            billing name.
        customer_name:
          anyOf:
            - type: string
            - type: 'null'
          title: Customer Name
          description: Name of the customer.
        customer_email:
          anyOf:
            - type: string
            - type: 'null'
          title: Customer Email
          description: Email address of the customer.
        customer_ip_address:
          anyOf:
            - type: string
              format: ipvanyaddress
            - type: 'null'
          title: Customer Ip Address
        customer_billing_name:
          anyOf:
            - type: string
            - type: 'null'
          title: Customer Billing Name
        customer_billing_address:
          anyOf:
            - $ref: '#/components/schemas/Address'
              description: Billing address of the customer.
            - type: 'null'
        customer_tax_id:
          anyOf:
            - type: string
            - type: 'null'
          title: Customer Tax Id
        locale:
          anyOf:
            - type: string
            - type: 'null'
          title: Locale
        payment_method_type:
          anyOf:
            - type: string
            - type: 'null'
          title: Payment Method Type
          description: >-
            Payment method type selected by the customer in the checkout form,
            e.g. `card`, `apple_pay` or `upi`.
        payment_processor_metadata:
          additionalProperties:
            type: string
          type: object
          title: Payment Processor Metadata
        billing_address_fields:
          $ref: '#/components/schemas/CheckoutBillingAddressFields'
          description: >-
            Determine which billing address fields should be disabled, optional
            or required in the checkout form.
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
        metadata:
          $ref: '#/components/schemas/MetadataOutputType'
        external_customer_id:
          anyOf:
            - type: string
            - type: 'null'
          title: External Customer Id
          description: >-
            ID of the customer in your system. If a matching customer exists on
            Polar, the resulting order will be linked to this customer.
            Otherwise, a new customer will be created with this external ID set.
        products:
          items:
            $ref: '#/components/schemas/CheckoutProduct'
          type: array
          title: Products
          description: List of products available to select.
        product:
          anyOf:
            - $ref: '#/components/schemas/CheckoutProduct'
            - type: 'null'
          description: Product selected to checkout.
        product_price:
          anyOf:
            - oneOf:
                - $ref: '#/components/schemas/LegacyRecurringProductPrice'
                - $ref: '#/components/schemas/ProductPrice'
            - type: 'null'
          title: Product Price
          description: Price of the selected product.
          deprecated: true
        prices:
          anyOf:
            - additionalProperties:
                items:
                  oneOf:
                    - $ref: '#/components/schemas/LegacyRecurringProductPrice'
                    - $ref: '#/components/schemas/ProductPrice'
                type: array
                description: List of prices for this product.
              propertyNames:
                format: uuid4
              type: object
            - type: 'null'
          title: Prices
          description: Mapping of product IDs to their list of prices.
        discount:
          anyOf:
            - oneOf:
                - $ref: >-
                    #/components/schemas/CheckoutDiscountFixedOnceForeverDuration
                - $ref: '#/components/schemas/CheckoutDiscountFixedRepeatDuration'
                - $ref: >-
                    #/components/schemas/CheckoutDiscountPercentageOnceForeverDuration
                - $ref: >-
                    #/components/schemas/CheckoutDiscountPercentageRepeatDuration
            - type: 'null'
          title: Discount
        subscription_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Subscription Id
        attached_custom_fields:
          anyOf:
            - items:
                $ref: '#/components/schemas/AttachedCustomField'
              type: array
            - type: 'null'
          title: Attached Custom Fields
        customer_metadata:
          additionalProperties:
            anyOf:
              - type: string
              - type: integer
              - type: boolean
          type: object
          title: Customer Metadata
      type: object
      required:
        - id
        - created_at
        - modified_at
        - payment_processor
        - status
        - client_secret
        - url
        - expires_at
        - success_url
        - return_url
        - embed_origin
        - amount
        - units
        - min_units
        - max_units
        - discount_amount
        - net_amount
        - tax_amount
        - tax_behavior
        - total_amount
        - currency
        - allow_trial
        - active_trial_interval
        - active_trial_interval_count
        - trial_end
        - organization_id
        - product_id
        - product_price_id
        - discount_id
        - allow_discount_codes
        - require_billing_address
        - is_discount_applicable
        - is_free_product_price
        - is_payment_required
        - is_payment_setup_required
        - is_payment_form_required
        - customer_id
        - is_business_customer
        - customer_name
        - customer_email
        - customer_ip_address
        - customer_billing_name
        - customer_billing_address
        - customer_tax_id
        - payment_method_type
        - payment_processor_metadata
        - billing_address_fields
        - trial_interval
        - trial_interval_count
        - metadata
        - external_customer_id
        - products
        - product
        - product_price
        - prices
        - discount
        - subscription_id
        - attached_custom_fields
        - customer_metadata
      title: Checkout
      description: Checkout session data retrieved using an access token.
    HTTPValidationError:
      properties:
        detail:
          items:
            $ref: '#/components/schemas/ValidationError'
          type: array
          title: Detail
      type: object
      title: HTTPValidationError
    CheckoutProductsCreate:
      properties:
        trial_interval:
          anyOf:
            - $ref: '#/components/schemas/TrialInterval'
            - type: 'null'
          description: The interval unit for the trial period.
        trial_interval_count:
          anyOf:
            - type: integer
              maximum: 1000
              minimum: 1
            - type: 'null'
          title: Trial Interval Count
          description: The number of interval units for the trial period.
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
        discount_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Discount Id
          description: ID of the discount to apply to the checkout.
        allow_discount_codes:
          type: boolean
          title: Allow Discount Codes
          description: >-
            Whether to allow the customer to apply discount codes. If you apply
            a discount through `discount_id`, it'll still be applied, but the
            customer won't be able to change it.
          default: true
        require_billing_address:
          type: boolean
          title: Require Billing Address
          description: >-
            Whether to require the customer to fill their full billing address,
            instead of just the country. Customers in the US will always be
            required to fill their full address, regardless of this setting. If
            you preset the billing address, this setting will be automatically
            set to `true`.
          default: false
        amount:
          anyOf:
            - type: integer
              minimum: 0
              description: >-
                Amount in cents, before discounts and taxes. Only useful for
                custom prices, it'll be ignored for fixed and free prices. 
            - type: 'null'
          title: Amount
        seats:
          anyOf:
            - type: integer
              maximum: 10000
              minimum: 1
            - type: 'null'
          title: Seats
          description: Predefined number of seats (works with seat-based pricing only)
        min_seats:
          anyOf:
            - type: integer
              maximum: 10000
              minimum: 1
            - type: 'null'
          title: Min Seats
          description: Minimum number of seats (works with seat-based pricing only)
        max_seats:
          anyOf:
            - type: integer
              maximum: 10000
              minimum: 1
            - type: 'null'
          title: Max Seats
          description: Maximum number of seats (works with seat-based pricing only)
        units:
          anyOf:
            - type: integer
              maximum: 2147483647
              minimum: 1
            - type: 'null'
          title: Units
          description: Predefined number of units (works with unit-based pricing only)
        min_units:
          anyOf:
            - type: integer
              maximum: 2147483647
              minimum: 1
            - type: 'null'
          title: Min Units
          description: Minimum number of units (works with unit-based pricing only)
        max_units:
          anyOf:
            - type: integer
              maximum: 2147483647
              minimum: 1
            - type: 'null'
          title: Max Units
          description: Maximum number of units (works with unit-based pricing only)
        allow_trial:
          type: boolean
          title: Allow Trial
          description: >-
            Whether to enable the trial period for the checkout session. If
            `false`, the trial period will be disabled, even if the selected
            product has a trial configured.
          default: true
        customer_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Customer Id
          description: >-
            ID of an existing customer in the organization. The customer data
            will be pre-filled in the checkout form. The resulting order will be
            linked to this customer.
        is_business_customer:
          type: boolean
          title: Is Business Customer
          description: >-
            Whether the customer is a business or an individual. If `true`, the
            customer will be required to fill their full billing address and
            billing name.
          default: false
        external_customer_id:
          anyOf:
            - type: string
            - type: 'null'
          title: External Customer Id
          description: >-
            ID of the customer in your system. If a matching customer exists on
            Polar, the resulting order will be linked to this customer.
            Otherwise, a new customer will be created with this external ID set.
        customer_name:
          anyOf:
            - type: string
              maxLength: 256
              description: The name of the customer.
              examples:
                - John Doe
            - type: 'null'
          title: Customer Name
        customer_email:
          anyOf:
            - type: string
              format: email
              description: Email address of the customer.
            - type: 'null'
          title: Customer Email
        customer_ip_address:
          anyOf:
            - type: string
              format: ipvanyaddress
            - type: 'null'
          title: Customer Ip Address
        customer_billing_name:
          anyOf:
            - type: string
            - type: 'null'
          title: Customer Billing Name
        customer_billing_address:
          anyOf:
            - $ref: '#/components/schemas/AddressInput'
              description: Billing address of the customer.
            - type: 'null'
        customer_tax_id:
          anyOf:
            - type: string
            - type: 'null'
          title: Customer Tax Id
        customer_metadata:
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
          title: Customer Metadata
          description: >-
            Key-value object allowing you to store additional information
            that'll be copied to the created customer.


            The key must be a string with a maximum length of **40 characters**.

            The value must be either:


            * A string with a maximum length of **500 characters**

            * An integer

            * A floating-point number

            * A boolean


            You can store up to **50 key-value pairs**.
        subscription_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Subscription Id
          description: >-
            ID of a subscription to upgrade. It must be on a free pricing. If
            checkout is successful, metadata set on this checkout will be copied
            to the subscription, and existing keys will be overwritten.
        success_url:
          anyOf:
            - type: string
              maxLength: 2083
              minLength: 1
              format: uri
            - type: 'null'
          title: Success Url
          description: >-
            URL where the customer will be redirected after a successful
            payment.You can add the `checkout_id={CHECKOUT_ID}` query parameter
            to retrieve the checkout session id.
        return_url:
          anyOf:
            - type: string
              maxLength: 2083
              minLength: 1
              format: uri
            - type: 'null'
          title: Return Url
          description: >-
            When set, a back button will be shown in the checkout to return to
            this URL.
        embed_origin:
          anyOf:
            - type: string
            - type: 'null'
          title: Embed Origin
          description: >-
            If you plan to embed the checkout session, set this to the Origin of
            the embedding page. It'll allow the Polar iframe to communicate with
            the parent page.
        locale:
          anyOf:
            - type: string
              description: >-
                Locale of the customer, given as an IETF BCP 47 language tag,
                e.g. `en`, `en-US` or `en-GB-oxendict`. If `null` or
                unsupported, the locale will default to `en`.
              examples:
                - en
                - en-US
                - fr
                - fr-CA
            - type: 'null'
          title: Locale
        currency:
          anyOf:
            - $ref: '#/components/schemas/PresentmentCurrency'
              maxLength: 3
              minLength: 3
            - type: 'null'
        products:
          items:
            type: string
            format: uuid4
          type: array
          minItems: 1
          title: Products
          description: >-
            List of product IDs available to select at that checkout. The first
            one will be selected by default.
        prices:
          anyOf:
            - additionalProperties:
                items:
                  oneOf:
                    - $ref: '#/components/schemas/ProductPriceFixedCreate'
                    - $ref: '#/components/schemas/ProductPriceCustomCreate'
                    - $ref: '#/components/schemas/ProductPriceSeatBasedCreate'
                    - $ref: '#/components/schemas/ProductPriceUnitBasedCreate'
                    - $ref: '#/components/schemas/ProductPriceMeteredUnitCreate'
                    - $ref: '#/components/schemas/ProductPriceMeteredTiersCreate'
                  discriminator:
                    propertyName: amount_type
                    mapping:
                      custom:
                        $ref: '#/components/schemas/ProductPriceCustomCreate'
                      fixed:
                        $ref: '#/components/schemas/ProductPriceFixedCreate'
                      metered_tiers:
                        $ref: '#/components/schemas/ProductPriceMeteredTiersCreate'
                      metered_unit:
                        $ref: '#/components/schemas/ProductPriceMeteredUnitCreate'
                      seat_based:
                        $ref: '#/components/schemas/ProductPriceSeatBasedCreate'
                      unit_based:
                        $ref: '#/components/schemas/ProductPriceUnitBasedCreate'
                type: array
                minItems: 1
                description: >-
                  List of prices for the product. At most one fixed price and
                  one seat-based price may be combined (billed as `fixed +
                  seat_charge`), or a single custom price may stand alone, plus
                  any number of metered prices. A custom price cannot be
                  combined with a fixed or seat-based price.
              propertyNames:
                format: uuid4
              type: object
            - type: 'null'
          title: Prices
          description: >-
            Optional mapping of product IDs to a list of ad-hoc prices to create
            for that product. If not set, catalog prices of the product will be
            used.
      type: object
      required:
        - products
      title: CheckoutProductsCreate
      description: |-
        Create a new checkout session from a list of products.
        Customers will be able to switch between those products.

        Metadata set on the checkout will be copied
        to the resulting order and/or subscription.
    PaymentProcessor:
      type: string
      enum:
        - stripe
      title: PaymentProcessor
    CheckoutStatus:
      type: string
      enum:
        - open
        - expired
        - confirmed
        - succeeded
        - failed
      title: CheckoutStatus
    TaxBehavior:
      type: string
      enum:
        - inclusive
        - exclusive
      title: TaxBehavior
    TrialInterval:
      type: string
      enum:
        - day
        - week
        - month
        - year
      title: TrialInterval
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
    CheckoutBillingAddressFields:
      properties:
        country:
          $ref: '#/components/schemas/BillingAddressFieldMode'
        state:
          $ref: '#/components/schemas/BillingAddressFieldMode'
        city:
          $ref: '#/components/schemas/BillingAddressFieldMode'
        postal_code:
          $ref: '#/components/schemas/BillingAddressFieldMode'
        line1:
          $ref: '#/components/schemas/BillingAddressFieldMode'
        line2:
          $ref: '#/components/schemas/BillingAddressFieldMode'
      type: object
      required:
        - country
        - state
        - city
        - postal_code
        - line1
        - line2
      title: CheckoutBillingAddressFields
    MetadataOutputType:
      additionalProperties:
        anyOf:
          - type: string
          - type: integer
          - type: number
          - type: boolean
      type: object
    CheckoutProduct:
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
        prices:
          items:
            oneOf:
              - $ref: '#/components/schemas/LegacyRecurringProductPrice'
              - $ref: '#/components/schemas/ProductPrice'
          type: array
          title: Prices
          description: List of prices for this product.
        benefits:
          items:
            $ref: '#/components/schemas/BenefitPublic'
            title: BenefitPublic
          type: array
          title: Benefits
          description: List of benefits granted by the product.
        medias:
          items:
            $ref: '#/components/schemas/ProductMediaFileRead'
          type: array
          title: Medias
          description: List of medias associated to the product.
      type: object
      required:
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
        - prices
        - benefits
        - medias
      title: CheckoutProduct
      description: Product data for a checkout session.
    LegacyRecurringProductPrice:
      oneOf:
        - $ref: '#/components/schemas/LegacyRecurringProductPriceFixed'
        - $ref: '#/components/schemas/LegacyRecurringProductPriceCustom'
      discriminator:
        propertyName: amount_type
        mapping:
          custom:
            $ref: '#/components/schemas/LegacyRecurringProductPriceCustom'
          fixed:
            $ref: '#/components/schemas/LegacyRecurringProductPriceFixed'
    ProductPrice:
      oneOf:
        - $ref: '#/components/schemas/ProductPriceFixed'
        - $ref: '#/components/schemas/ProductPriceCustom'
        - $ref: '#/components/schemas/ProductPriceSeatBased'
        - $ref: '#/components/schemas/ProductPriceUnitBased'
        - $ref: '#/components/schemas/ProductPriceMeteredUnit'
        - $ref: '#/components/schemas/ProductPriceMeteredTiers'
      discriminator:
        propertyName: amount_type
        mapping:
          custom:
            $ref: '#/components/schemas/ProductPriceCustom'
          fixed:
            $ref: '#/components/schemas/ProductPriceFixed'
          metered_tiers:
            $ref: '#/components/schemas/ProductPriceMeteredTiers'
          metered_unit:
            $ref: '#/components/schemas/ProductPriceMeteredUnit'
          seat_based:
            $ref: '#/components/schemas/ProductPriceSeatBased'
          unit_based:
            $ref: '#/components/schemas/ProductPriceUnitBased'
    CheckoutDiscountFixedOnceForeverDuration:
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
        id:
          type: string
          format: uuid4
          title: Id
          description: The ID of the object.
        name:
          type: string
          title: Name
        code:
          anyOf:
            - type: string
            - type: 'null'
          title: Code
      type: object
      required:
        - duration
        - type
        - amount
        - currency
        - amounts
        - id
        - name
        - code
      title: CheckoutDiscountFixedOnceForeverDuration
      description: Schema for a fixed amount discount that is applied once or forever.
    CheckoutDiscountFixedRepeatDuration:
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
        id:
          type: string
          format: uuid4
          title: Id
          description: The ID of the object.
        name:
          type: string
          title: Name
        code:
          anyOf:
            - type: string
            - type: 'null'
          title: Code
      type: object
      required:
        - duration
        - duration_in_months
        - type
        - amount
        - currency
        - amounts
        - id
        - name
        - code
      title: CheckoutDiscountFixedRepeatDuration
      description: |-
        Schema for a fixed amount discount that is applied on every invoice
        for a certain number of months.
    CheckoutDiscountPercentageOnceForeverDuration:
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
        id:
          type: string
          format: uuid4
          title: Id
          description: The ID of the object.
        name:
          type: string
          title: Name
        code:
          anyOf:
            - type: string
            - type: 'null'
          title: Code
      type: object
      required:
        - duration
        - type
        - basis_points
        - id
        - name
        - code
      title: CheckoutDiscountPercentageOnceForeverDuration
      description: Schema for a percentage discount that is applied once or forever.
    CheckoutDiscountPercentageRepeatDuration:
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
        id:
          type: string
          format: uuid4
          title: Id
          description: The ID of the object.
        name:
          type: string
          title: Name
        code:
          anyOf:
            - type: string
            - type: 'null'
          title: Code
      type: object
      required:
        - duration
        - duration_in_months
        - type
        - basis_points
        - id
        - name
        - code
      title: CheckoutDiscountPercentageRepeatDuration
      description: |-
        Schema for a percentage discount that is applied on every invoice
        for a certain number of months.
    AttachedCustomField:
      properties:
        custom_field_id:
          type: string
          format: uuid4
          title: Custom Field Id
          description: ID of the custom field.
        custom_field:
          $ref: '#/components/schemas/CustomField'
          title: CustomField
        order:
          type: integer
          title: Order
          description: Order of the custom field in the resource.
        required:
          type: boolean
          title: Required
          description: Whether the value is required for this custom field.
      type: object
      required:
        - custom_field_id
        - custom_field
        - order
        - required
      title: AttachedCustomField
      description: Schema of a custom field attached to a resource.
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
    AddressInput:
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
          title: CountryAlpha2Input
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
      title: AddressInput
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
    ProductPriceFixedCreate:
      properties:
        amount_type:
          type: string
          const: fixed
          title: Amount Type
        price_currency:
          $ref: '#/components/schemas/PresentmentCurrency'
          description: The currency in which the customer will be charged.
          default: usd
        tax_behavior:
          anyOf:
            - $ref: '#/components/schemas/TaxBehaviorOption'
            - type: 'null'
          description: >-
            The tax behavior of the price. If not set, it will default to the
            organization's default tax behavior.
        price_amount:
          type: integer
          minimum: 0
          title: Price Amount
          description: |-
            The price in cents. Set to `0` for a free price.
            Minimum amounts per currency:
            - USD: 0.5
            - AED: 2
            - ALL: 50
            - AMD: 200
            - AOA: 500
            - ARS: 750
            - AUD: 0.7
            - AWG: 1
            - AZN: 1
            - BAM: 1
            - BBD: 2
            - BDT: 70
            - BIF: 2,000
            - BMD: 1
            - BND: 1
            - BOB: 5
            - BRL: 2.5
            - BSD: 1
            - BWP: 10
            - BZD: 2
            - CAD: 0.7
            - CDF: 2,000
            - CHF: 0.5
            - CLP: 500
            - CNY: 5
            - COP: 2,000
            - CRC: 300
            - CVE: 50
            - CZK: 15
            - DJF: 100
            - DKK: 3.2
            - DOP: 40
            - DZD: 70
            - EGP: 30
            - ETB: 80
            - EUR: 0.5
            - FJD: 2
            - FKP: 1
            - GBP: 0.4
            - GEL: 2
            - GNF: 5,000
            - GIP: 1
            - GMD: 40
            - GTQ: 5
            - GYD: 200
            - HKD: 4
            - HNL: 20
            - HTG: 70
            - HUF: 175
            - IDR: 9,000
            - ILS: 1.5
            - INR: 60
            - ISK: 70
            - JMD: 80
            - JPY: 80
            - KES: 70
            - KGS: 50
            - KHR: 3,000
            - KMF: 500
            - KRW: 800
            - KYD: 1
            - KZT: 300
            - LAK: 20,000
            - LKR: 200
            - LRD: 100
            - LSL: 10
            - MAD: 5
            - MDL: 10
            - MGA: 3,000
            - MKD: 50
            - MNT: 2,000
            - MOP: 5
            - MUR: 50
            - MVR: 8
            - MXN: 9
            - MWK: 1,000
            - MYR: 2
            - MZN: 50
            - NAD: 10
            - NGN: 700
            - NIO: 20
            - NOK: 5
            - NPR: 80
            - NZD: 0.9
            - PAB: 1
            - PEN: 2
            - PGK: 3
            - PHP: 35
            - PKR: 200
            - PLN: 2
            - PYG: 4,000
            - QAR: 2
            - RON: 2.5
            - RSD: 60
            - RWF: 1,000
            - SAR: 2
            - SBD: 4
            - SCR: 8
            - SEK: 5
            - SGD: 0.7
            - SHP: 1
            - SOS: 500
            - SRD: 20
            - SZL: 10
            - THB: 20
            - TJS: 5
            - TOP: 2
            - TRY: 30
            - TTD: 4
            - TWD: 20
            - TZS: 2,000
            - UAH: 30
            - UGX: 2,000
            - UYU: 20
            - UZS: 7,000
            - VND: 20,000
            - VUV: 100
            - WST: 2
            - XAF: 500
            - XCD: 2
            - XCG: 1
            - XOF: 500
            - XPF: 100
            - YER: 200
            - ZAR: 9
            - ZMW: 10
            - Other currencies: 50 minor units
      type: object
      required:
        - amount_type
        - price_amount
      title: ProductPriceFixedCreate
      description: Schema to create a fixed price.
    ProductPriceCustomCreate:
      properties:
        amount_type:
          type: string
          const: custom
          title: Amount Type
        price_currency:
          $ref: '#/components/schemas/PresentmentCurrency'
          description: The currency in which the customer will be charged.
          default: usd
        tax_behavior:
          anyOf:
            - $ref: '#/components/schemas/TaxBehaviorOption'
            - type: 'null'
          description: >-
            The tax behavior of the price. If not set, it will default to the
            organization's default tax behavior.
        minimum_amount:
          type: integer
          minimum: 0
          title: Minimum Amount
          description: >-
            The minimum amount the customer can pay. If set to 0, the price is
            'free or pay what you want' and $0 is accepted. If set to a value
            below the minimum price amount for the currency, it will be
            rejected. Defaults to the minimum price amount for the currency.
            Minimum per currency:

            - USD: 0.5

            - AED: 2

            - ALL: 50

            - AMD: 200

            - AOA: 500

            - ARS: 750

            - AUD: 0.7

            - AWG: 1

            - AZN: 1

            - BAM: 1

            - BBD: 2

            - BDT: 70

            - BIF: 2,000

            - BMD: 1

            - BND: 1

            - BOB: 5

            - BRL: 2.5

            - BSD: 1

            - BWP: 10

            - BZD: 2

            - CAD: 0.7

            - CDF: 2,000

            - CHF: 0.5

            - CLP: 500

            - CNY: 5

            - COP: 2,000

            - CRC: 300

            - CVE: 50

            - CZK: 15

            - DJF: 100

            - DKK: 3.2

            - DOP: 40

            - DZD: 70

            - EGP: 30

            - ETB: 80

            - EUR: 0.5

            - FJD: 2

            - FKP: 1

            - GBP: 0.4

            - GEL: 2

            - GNF: 5,000

            - GIP: 1

            - GMD: 40

            - GTQ: 5

            - GYD: 200

            - HKD: 4

            - HNL: 20

            - HTG: 70

            - HUF: 175

            - IDR: 9,000

            - ILS: 1.5

            - INR: 60

            - ISK: 70

            - JMD: 80

            - JPY: 80

            - KES: 70

            - KGS: 50

            - KHR: 3,000

            - KMF: 500

            - KRW: 800

            - KYD: 1

            - KZT: 300

            - LAK: 20,000

            - LKR: 200

            - LRD: 100

            - LSL: 10

            - MAD: 5

            - MDL: 10

            - MGA: 3,000

            - MKD: 50

            - MNT: 2,000

            - MOP: 5

            - MUR: 50

            - MVR: 8

            - MXN: 9

            - MWK: 1,000

            - MYR: 2

            - MZN: 50

            - NAD: 10

            - NGN: 700

            - NIO: 20

            - NOK: 5

            - NPR: 80

            - NZD: 0.9

            - PAB: 1

            - PEN: 2

            - PGK: 3

            - PHP: 35

            - PKR: 200

            - PLN: 2

            - PYG: 4,000

            - QAR: 2

            - RON: 2.5

            - RSD: 60

            - RWF: 1,000

            - SAR: 2

            - SBD: 4

            - SCR: 8

            - SEK: 5

            - SGD: 0.7

            - SHP: 1

            - SOS: 500

            - SRD: 20

            - SZL: 10

            - THB: 20

            - TJS: 5

            - TOP: 2

            - TRY: 30

            - TTD: 4

            - TWD: 20

            - TZS: 2,000

            - UAH: 30

            - UGX: 2,000

            - UYU: 20

            - UZS: 7,000

            - VND: 20,000

            - VUV: 100

            - WST: 2

            - XAF: 500

            - XCD: 2

            - XCG: 1

            - XOF: 500

            - XPF: 100

            - YER: 200

            - ZAR: 9

            - ZMW: 10

            - Other currencies: 50 minor units
          default: 50
        maximum_amount:
          anyOf:
            - type: integer
              minimum: 1
              description: |-
                The price in cents.
                Minimum amounts per currency:
                - USD: 0.5
                - AED: 2
                - ALL: 50
                - AMD: 200
                - AOA: 500
                - ARS: 750
                - AUD: 0.7
                - AWG: 1
                - AZN: 1
                - BAM: 1
                - BBD: 2
                - BDT: 70
                - BIF: 2,000
                - BMD: 1
                - BND: 1
                - BOB: 5
                - BRL: 2.5
                - BSD: 1
                - BWP: 10
                - BZD: 2
                - CAD: 0.7
                - CDF: 2,000
                - CHF: 0.5
                - CLP: 500
                - CNY: 5
                - COP: 2,000
                - CRC: 300
                - CVE: 50
                - CZK: 15
                - DJF: 100
                - DKK: 3.2
                - DOP: 40
                - DZD: 70
                - EGP: 30
                - ETB: 80
                - EUR: 0.5
                - FJD: 2
                - FKP: 1
                - GBP: 0.4
                - GEL: 2
                - GNF: 5,000
                - GIP: 1
                - GMD: 40
                - GTQ: 5
                - GYD: 200
                - HKD: 4
                - HNL: 20
                - HTG: 70
                - HUF: 175
                - IDR: 9,000
                - ILS: 1.5
                - INR: 60
                - ISK: 70
                - JMD: 80
                - JPY: 80
                - KES: 70
                - KGS: 50
                - KHR: 3,000
                - KMF: 500
                - KRW: 800
                - KYD: 1
                - KZT: 300
                - LAK: 20,000
                - LKR: 200
                - LRD: 100
                - LSL: 10
                - MAD: 5
                - MDL: 10
                - MGA: 3,000
                - MKD: 50
                - MNT: 2,000
                - MOP: 5
                - MUR: 50
                - MVR: 8
                - MXN: 9
                - MWK: 1,000
                - MYR: 2
                - MZN: 50
                - NAD: 10
                - NGN: 700
                - NIO: 20
                - NOK: 5
                - NPR: 80
                - NZD: 0.9
                - PAB: 1
                - PEN: 2
                - PGK: 3
                - PHP: 35
                - PKR: 200
                - PLN: 2
                - PYG: 4,000
                - QAR: 2
                - RON: 2.5
                - RSD: 60
                - RWF: 1,000
                - SAR: 2
                - SBD: 4
                - SCR: 8
                - SEK: 5
                - SGD: 0.7
                - SHP: 1
                - SOS: 500
                - SRD: 20
                - SZL: 10
                - THB: 20
                - TJS: 5
                - TOP: 2
                - TRY: 30
                - TTD: 4
                - TWD: 20
                - TZS: 2,000
                - UAH: 30
                - UGX: 2,000
                - UYU: 20
                - UZS: 7,000
                - VND: 20,000
                - VUV: 100
                - WST: 2
                - XAF: 500
                - XCD: 2
                - XCG: 1
                - XOF: 500
                - XPF: 100
                - YER: 200
                - ZAR: 9
                - ZMW: 10
                - Other currencies: 50 minor units
            - type: 'null'
          title: Maximum Amount
          description: |-
            The maximum amount the customer can pay. Maximum per currency:
            - USD: 999,999.99
            - EUR: 999,999.99
            - GBP: 999,999.99
            - ARS: 1,400,000
            - CDF: 2,800,000
            - COP: 4,000,000
            - IDR: 16,000,000
            - KHR: 4,000,000
            - LAK: 21,000,000
            - MNT: 3,500,000
            - MWK: 1,750,000
            - NGN: 1,550,000
            - TZS: 2,500,000
            - UGX: 3,700,000
            - UZS: 12,500,000
            - Other currencies: 99,999,999 minor units
        preset_amount:
          anyOf:
            - type: integer
              minimum: 0
              description: |-
                The price in cents.
                Minimum amounts per currency:
                - USD: 0.5
                - AED: 2
                - ALL: 50
                - AMD: 200
                - AOA: 500
                - ARS: 750
                - AUD: 0.7
                - AWG: 1
                - AZN: 1
                - BAM: 1
                - BBD: 2
                - BDT: 70
                - BIF: 2,000
                - BMD: 1
                - BND: 1
                - BOB: 5
                - BRL: 2.5
                - BSD: 1
                - BWP: 10
                - BZD: 2
                - CAD: 0.7
                - CDF: 2,000
                - CHF: 0.5
                - CLP: 500
                - CNY: 5
                - COP: 2,000
                - CRC: 300
                - CVE: 50
                - CZK: 15
                - DJF: 100
                - DKK: 3.2
                - DOP: 40
                - DZD: 70
                - EGP: 30
                - ETB: 80
                - EUR: 0.5
                - FJD: 2
                - FKP: 1
                - GBP: 0.4
                - GEL: 2
                - GNF: 5,000
                - GIP: 1
                - GMD: 40
                - GTQ: 5
                - GYD: 200
                - HKD: 4
                - HNL: 20
                - HTG: 70
                - HUF: 175
                - IDR: 9,000
                - ILS: 1.5
                - INR: 60
                - ISK: 70
                - JMD: 80
                - JPY: 80
                - KES: 70
                - KGS: 50
                - KHR: 3,000
                - KMF: 500
                - KRW: 800
                - KYD: 1
                - KZT: 300
                - LAK: 20,000
                - LKR: 200
                - LRD: 100
                - LSL: 10
                - MAD: 5
                - MDL: 10
                - MGA: 3,000
                - MKD: 50
                - MNT: 2,000
                - MOP: 5
                - MUR: 50
                - MVR: 8
                - MXN: 9
                - MWK: 1,000
                - MYR: 2
                - MZN: 50
                - NAD: 10
                - NGN: 700
                - NIO: 20
                - NOK: 5
                - NPR: 80
                - NZD: 0.9
                - PAB: 1
                - PEN: 2
                - PGK: 3
                - PHP: 35
                - PKR: 200
                - PLN: 2
                - PYG: 4,000
                - QAR: 2
                - RON: 2.5
                - RSD: 60
                - RWF: 1,000
                - SAR: 2
                - SBD: 4
                - SCR: 8
                - SEK: 5
                - SGD: 0.7
                - SHP: 1
                - SOS: 500
                - SRD: 20
                - SZL: 10
                - THB: 20
                - TJS: 5
                - TOP: 2
                - TRY: 30
                - TTD: 4
                - TWD: 20
                - TZS: 2,000
                - UAH: 30
                - UGX: 2,000
                - UYU: 20
                - UZS: 7,000
                - VND: 20,000
                - VUV: 100
                - WST: 2
                - XAF: 500
                - XCD: 2
                - XCG: 1
                - XOF: 500
                - XPF: 100
                - YER: 200
                - ZAR: 9
                - ZMW: 10
                - Other currencies: 50 minor units
            - type: 'null'
          title: Preset Amount
          description: >-
            The initial amount shown to the customer. If 0, the customer will
            see $0 as the default. If set to a value below the minimum price
            amount for the currency, it will be rejected.Minimum per currency:

            - USD: 0.5

            - AED: 2

            - ALL: 50

            - AMD: 200

            - AOA: 500

            - ARS: 750

            - AUD: 0.7

            - AWG: 1

            - AZN: 1

            - BAM: 1

            - BBD: 2

            - BDT: 70

            - BIF: 2,000

            - BMD: 1

            - BND: 1

            - BOB: 5

            - BRL: 2.5

            - BSD: 1

            - BWP: 10

            - BZD: 2

            - CAD: 0.7

            - CDF: 2,000

            - CHF: 0.5

            - CLP: 500

            - CNY: 5

            - COP: 2,000

            - CRC: 300

            - CVE: 50

            - CZK: 15

            - DJF: 100

            - DKK: 3.2

            - DOP: 40

            - DZD: 70

            - EGP: 30

            - ETB: 80

            - EUR: 0.5

            - FJD: 2

            - FKP: 1

            - GBP: 0.4

            - GEL: 2

            - GNF: 5,000

            - GIP: 1

            - GMD: 40

            - GTQ: 5

            - GYD: 200

            - HKD: 4

            - HNL: 20

            - HTG: 70

            - HUF: 175

            - IDR: 9,000

            - ILS: 1.5

            - INR: 60

            - ISK: 70

            - JMD: 80

            - JPY: 80

            - KES: 70

            - KGS: 50

            - KHR: 3,000

            - KMF: 500

            - KRW: 800

            - KYD: 1

            - KZT: 300

            - LAK: 20,000

            - LKR: 200

            - LRD: 100

            - LSL: 10

            - MAD: 5

            - MDL: 10

            - MGA: 3,000

            - MKD: 50

            - MNT: 2,000

            - MOP: 5

            - MUR: 50

            - MVR: 8

            - MXN: 9

            - MWK: 1,000

            - MYR: 2

            - MZN: 50

            - NAD: 10

            - NGN: 700

            - NIO: 20

            - NOK: 5

            - NPR: 80

            - NZD: 0.9

            - PAB: 1

            - PEN: 2

            - PGK: 3

            - PHP: 35

            - PKR: 200

            - PLN: 2

            - PYG: 4,000

            - QAR: 2

            - RON: 2.5

            - RSD: 60

            - RWF: 1,000

            - SAR: 2

            - SBD: 4

            - SCR: 8

            - SEK: 5

            - SGD: 0.7

            - SHP: 1

            - SOS: 500

            - SRD: 20

            - SZL: 10

            - THB: 20

            - TJS: 5

            - TOP: 2

            - TRY: 30

            - TTD: 4

            - TWD: 20

            - TZS: 2,000

            - UAH: 30

            - UGX: 2,000

            - UYU: 20

            - UZS: 7,000

            - VND: 20,000

            - VUV: 100

            - WST: 2

            - XAF: 500

            - XCD: 2

            - XCG: 1

            - XOF: 500

            - XPF: 100

            - YER: 200

            - ZAR: 9

            - ZMW: 10

            - Other currencies: 50 minor units
      type: object
      required:
        - amount_type
      title: ProductPriceCustomCreate
      description: Schema to create a pay-what-you-want price.
    ProductPriceSeatBasedCreate:
      properties:
        amount_type:
          type: string
          const: seat_based
          title: Amount Type
        price_currency:
          $ref: '#/components/schemas/PresentmentCurrency'
          description: The currency in which the customer will be charged.
          default: usd
        tax_behavior:
          anyOf:
            - $ref: '#/components/schemas/TaxBehaviorOption'
            - type: 'null'
          description: >-
            The tax behavior of the price. If not set, it will default to the
            organization's default tax behavior.
        seat_tiers:
          $ref: '#/components/schemas/ProductPriceSeatTiers-Input'
          description: Tiered pricing based on seat quantity
      type: object
      required:
        - amount_type
        - seat_tiers
      title: ProductPriceSeatBasedCreate
      description: Schema to create a seat-based price with volume-based tiers.
    ProductPriceUnitBasedCreate:
      properties:
        amount_type:
          type: string
          const: unit_based
          title: Amount Type
        price_currency:
          $ref: '#/components/schemas/PresentmentCurrency'
          description: The currency in which the customer will be charged.
          default: usd
        tax_behavior:
          anyOf:
            - $ref: '#/components/schemas/TaxBehaviorOption'
            - type: 'null'
          description: >-
            The tax behavior of the price. If not set, it will default to the
            organization's default tax behavior.
        tiers:
          $ref: '#/components/schemas/TiersInput'
          description: Tiered pricing based on the purchased unit quantity.
        minimum_units:
          anyOf:
            - type: integer
              minimum: 1
            - type: 'null'
          title: Minimum Units
          description: >-
            The minimum purchasable quantity (inclusive). Defaults to 1 when not
            set.
        unit_label:
          anyOf:
            - additionalProperties:
                additionalProperties:
                  type: string
                type: object
              type: object
              minLength: 1
              examples:
                - en:
                    '=1': seat
                    other: seats
            - type: 'null'
          title: Unit Label
          description: >-
            Per-locale unit nouns shown at checkout and on invoices. `{"en":
            {"=1": "device", "other": "devices"}}`. Defaults to "unit"/"units"
            when unset.
      type: object
      required:
        - amount_type
        - tiers
      title: ProductPriceUnitBasedCreate
      description: >-
        Schema to create a unit-based price: the buyer picks a quantity of
        units,

        pays for it up-front. On subscriptions, quantity changes are prorated.
    ProductPriceMeteredUnitCreate:
      properties:
        amount_type:
          type: string
          const: metered_unit
          title: Amount Type
        price_currency:
          $ref: '#/components/schemas/PresentmentCurrency'
          description: The currency in which the customer will be charged.
          default: usd
        tax_behavior:
          anyOf:
            - $ref: '#/components/schemas/TaxBehaviorOption'
            - type: 'null'
          description: >-
            The tax behavior of the price. If not set, it will default to the
            organization's default tax behavior.
        meter_id:
          type: string
          format: uuid4
          title: Meter Id
          description: The ID of the meter associated to the price.
        unit_amount:
          anyOf:
            - type: number
              exclusiveMinimum: 0
            - type: string
              pattern: >-
                ^(?!^[-+.]*$)[+-]?0*(?:\d{0,5}|(?=[\d.]{1,18}0*$)\d{0,5}\.\d{0,12}0*$)
          title: Unit Amount
          description: The price per unit in cents. Supports up to 12 decimal places.
        cap_amount:
          anyOf:
            - type: integer
              maximum: 2147483647
              minimum: 0
            - type: 'null'
          title: Cap Amount
          description: >-
            Optional maximum amount in cents that can be charged, regardless of
            the number of units consumed.
      type: object
      required:
        - amount_type
        - meter_id
        - unit_amount
      title: ProductPriceMeteredUnitCreate
      description: Schema to create a metered price with a fixed unit price.
    ProductPriceMeteredTiersCreate:
      properties:
        amount_type:
          type: string
          const: metered_tiers
          title: Amount Type
        price_currency:
          $ref: '#/components/schemas/PresentmentCurrency'
          description: The currency in which the customer will be charged.
          default: usd
        tax_behavior:
          anyOf:
            - $ref: '#/components/schemas/TaxBehaviorOption'
            - type: 'null'
          description: >-
            The tax behavior of the price. If not set, it will default to the
            organization's default tax behavior.
        meter_id:
          type: string
          format: uuid4
          title: Meter Id
          description: The ID of the meter associated to the price.
        tiers:
          $ref: '#/components/schemas/TiersInput'
          description: Tiered pricing based on consumed units.
        cap_amount:
          anyOf:
            - type: integer
              maximum: 2147483647
              minimum: 0
            - type: 'null'
          title: Cap Amount
          description: >-
            Optional maximum amount in cents that can be charged, regardless of
            the number of units consumed.
      type: object
      required:
        - amount_type
        - meter_id
        - tiers
      title: ProductPriceMeteredTiersCreate
      description: Schema to create a metered price billed from tiers on consumed units.
    BillingAddressFieldMode:
      type: string
      enum:
        - required
        - optional
        - disabled
      title: BillingAddressFieldMode
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
    BenefitPublic:
      oneOf:
        - $ref: '#/components/schemas/BenefitCustomPublic'
        - $ref: '#/components/schemas/BenefitDiscordPublic'
        - $ref: '#/components/schemas/BenefitGitHubRepositoryPublic'
        - $ref: '#/components/schemas/BenefitDownloadablesPublic'
        - $ref: '#/components/schemas/BenefitLicenseKeysPublic'
        - $ref: '#/components/schemas/BenefitFeatureFlagPublic'
        - $ref: '#/components/schemas/BenefitSlackSharedChannelPublic'
        - $ref: '#/components/schemas/BenefitMeterCreditPublic'
      discriminator:
        propertyName: type
        mapping:
          custom:
            $ref: '#/components/schemas/BenefitCustomPublic'
          discord:
            $ref: '#/components/schemas/BenefitDiscordPublic'
          downloadables:
            $ref: '#/components/schemas/BenefitDownloadablesPublic'
          feature_flag:
            $ref: '#/components/schemas/BenefitFeatureFlagPublic'
          github_repository:
            $ref: '#/components/schemas/BenefitGitHubRepositoryPublic'
          license_keys:
            $ref: '#/components/schemas/BenefitLicenseKeysPublic'
          meter_credit:
            $ref: '#/components/schemas/BenefitMeterCreditPublic'
          slack_shared_channel:
            $ref: '#/components/schemas/BenefitSlackSharedChannelPublic'
    ProductMediaFileRead:
      properties:
        id:
          type: string
          format: uuid4
          title: Id
          description: The ID of the object.
        organization_id:
          type: string
          format: uuid4
          title: Organization Id
        name:
          type: string
          title: Name
        path:
          type: string
          title: Path
        mime_type:
          type: string
          title: Mime Type
        size:
          type: integer
          title: Size
        storage_version:
          anyOf:
            - type: string
            - type: 'null'
          title: Storage Version
        checksum_etag:
          anyOf:
            - type: string
            - type: 'null'
          title: Checksum Etag
        checksum_sha256_base64:
          anyOf:
            - type: string
            - type: 'null'
          title: Checksum Sha256 Base64
        checksum_sha256_hex:
          anyOf:
            - type: string
            - type: 'null'
          title: Checksum Sha256 Hex
        last_modified_at:
          anyOf:
            - type: string
              format: date-time
              examples:
                - '2026-01-01T00:00:00.000000Z'
            - type: 'null'
          title: Last Modified At
        version:
          anyOf:
            - type: string
            - type: 'null'
          title: Version
        service:
          type: string
          const: product_media
          title: Service
        is_uploaded:
          type: boolean
          title: Is Uploaded
        created_at:
          type: string
          format: date-time
          title: Created At
          examples:
            - '2026-01-01T00:00:00.000000Z'
        size_readable:
          type: string
          title: Size Readable
          readOnly: true
        public_url:
          type: string
          title: Public Url
          readOnly: true
      type: object
      required:
        - id
        - organization_id
        - name
        - path
        - mime_type
        - size
        - storage_version
        - checksum_etag
        - checksum_sha256_base64
        - checksum_sha256_hex
        - last_modified_at
        - version
        - service
        - is_uploaded
        - created_at
        - size_readable
        - public_url
      title: ProductMediaFileRead
      description: File to be used as a product media file.
    LegacyRecurringProductPriceFixed:
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
          description: The ID of the price.
        source:
          $ref: '#/components/schemas/ProductPriceSource'
          description: >-
            The source of the price . `catalog` is a predefined price, while
            `ad_hoc` is a price created dynamically on a Checkout session.
        amount_type:
          type: string
          const: fixed
          title: Amount Type
        price_currency:
          type: string
          title: Price Currency
          description: The currency in which the customer will be charged.
        tax_behavior:
          anyOf:
            - $ref: '#/components/schemas/TaxBehaviorOption'
            - type: 'null'
          description: >-
            The tax behavior of the price. If null, it defaults to the
            organization's default tax behavior.
        is_archived:
          type: boolean
          title: Is Archived
          description: Whether the price is archived and no longer available.
        product_id:
          type: string
          format: uuid4
          title: Product Id
          description: The ID of the product owning the price.
        type:
          type: string
          const: recurring
          title: Type
          description: The type of the price.
        recurring_interval:
          $ref: '#/components/schemas/RecurringInterval'
          description: The recurring interval of the price.
        price_amount:
          type: integer
          title: Price Amount
          description: The price in cents.
        legacy:
          type: boolean
          const: true
          title: Legacy
          readOnly: true
      type: object
      required:
        - created_at
        - modified_at
        - id
        - source
        - amount_type
        - price_currency
        - tax_behavior
        - is_archived
        - product_id
        - type
        - recurring_interval
        - price_amount
        - legacy
      title: LegacyRecurringProductPriceFixed
      description: >-
        A recurring price for a product, i.e. a subscription.


        **Deprecated**: The recurring interval should be set on the product
        itself.
    LegacyRecurringProductPriceCustom:
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
          description: The ID of the price.
        source:
          $ref: '#/components/schemas/ProductPriceSource'
          description: >-
            The source of the price . `catalog` is a predefined price, while
            `ad_hoc` is a price created dynamically on a Checkout session.
        amount_type:
          type: string
          const: custom
          title: Amount Type
        price_currency:
          type: string
          title: Price Currency
          description: The currency in which the customer will be charged.
        tax_behavior:
          anyOf:
            - $ref: '#/components/schemas/TaxBehaviorOption'
            - type: 'null'
          description: >-
            The tax behavior of the price. If null, it defaults to the
            organization's default tax behavior.
        is_archived:
          type: boolean
          title: Is Archived
          description: Whether the price is archived and no longer available.
        product_id:
          type: string
          format: uuid4
          title: Product Id
          description: The ID of the product owning the price.
        type:
          type: string
          const: recurring
          title: Type
          description: The type of the price.
        recurring_interval:
          $ref: '#/components/schemas/RecurringInterval'
          description: The recurring interval of the price.
        minimum_amount:
          type: integer
          title: Minimum Amount
          description: >-
            The minimum amount the customer can pay. If 0, the price is 'free or
            pay what you want'.
        maximum_amount:
          anyOf:
            - type: integer
            - type: 'null'
          title: Maximum Amount
          description: The maximum amount the customer can pay.
        preset_amount:
          anyOf:
            - type: integer
            - type: 'null'
          title: Preset Amount
          description: The initial amount shown to the customer.
        legacy:
          type: boolean
          const: true
          title: Legacy
          readOnly: true
      type: object
      required:
        - created_at
        - modified_at
        - id
        - source
        - amount_type
        - price_currency
        - tax_behavior
        - is_archived
        - product_id
        - type
        - recurring_interval
        - minimum_amount
        - maximum_amount
        - preset_amount
        - legacy
      title: LegacyRecurringProductPriceCustom
      description: >-
        A pay-what-you-want recurring price for a product, i.e. a subscription.


        **Deprecated**: The recurring interval should be set on the product
        itself.
    ProductPriceFixed:
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
          description: The ID of the price.
        source:
          $ref: '#/components/schemas/ProductPriceSource'
          description: >-
            The source of the price . `catalog` is a predefined price, while
            `ad_hoc` is a price created dynamically on a Checkout session.
        amount_type:
          type: string
          const: fixed
          title: Amount Type
        price_currency:
          type: string
          title: Price Currency
          description: The currency in which the customer will be charged.
        tax_behavior:
          anyOf:
            - $ref: '#/components/schemas/TaxBehaviorOption'
            - type: 'null'
          description: >-
            The tax behavior of the price. If null, it defaults to the
            organization's default tax behavior.
        is_archived:
          type: boolean
          title: Is Archived
          description: Whether the price is archived and no longer available.
        product_id:
          type: string
          format: uuid4
          title: Product Id
          description: The ID of the product owning the price.
        price_amount:
          type: integer
          title: Price Amount
          description: The price in cents.
      type: object
      required:
        - created_at
        - modified_at
        - id
        - source
        - amount_type
        - price_currency
        - tax_behavior
        - is_archived
        - product_id
        - price_amount
      title: ProductPriceFixed
      description: A fixed price for a product.
    ProductPriceCustom:
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
          description: The ID of the price.
        source:
          $ref: '#/components/schemas/ProductPriceSource'
          description: >-
            The source of the price . `catalog` is a predefined price, while
            `ad_hoc` is a price created dynamically on a Checkout session.
        amount_type:
          type: string
          const: custom
          title: Amount Type
        price_currency:
          type: string
          title: Price Currency
          description: The currency in which the customer will be charged.
        tax_behavior:
          anyOf:
            - $ref: '#/components/schemas/TaxBehaviorOption'
            - type: 'null'
          description: >-
            The tax behavior of the price. If null, it defaults to the
            organization's default tax behavior.
        is_archived:
          type: boolean
          title: Is Archived
          description: Whether the price is archived and no longer available.
        product_id:
          type: string
          format: uuid4
          title: Product Id
          description: The ID of the product owning the price.
        minimum_amount:
          type: integer
          title: Minimum Amount
          description: >-
            The minimum amount the customer can pay. If 0, the price is 'free or
            pay what you want'.
        maximum_amount:
          anyOf:
            - type: integer
            - type: 'null'
          title: Maximum Amount
          description: The maximum amount the customer can pay.
        preset_amount:
          anyOf:
            - type: integer
            - type: 'null'
          title: Preset Amount
          description: The initial amount shown to the customer.
      type: object
      required:
        - created_at
        - modified_at
        - id
        - source
        - amount_type
        - price_currency
        - tax_behavior
        - is_archived
        - product_id
        - minimum_amount
        - maximum_amount
        - preset_amount
      title: ProductPriceCustom
      description: A pay-what-you-want price for a product.
    ProductPriceSeatBased:
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
          description: The ID of the price.
        source:
          $ref: '#/components/schemas/ProductPriceSource'
          description: >-
            The source of the price . `catalog` is a predefined price, while
            `ad_hoc` is a price created dynamically on a Checkout session.
        amount_type:
          type: string
          const: seat_based
          title: Amount Type
        price_currency:
          type: string
          title: Price Currency
          description: The currency in which the customer will be charged.
        tax_behavior:
          anyOf:
            - $ref: '#/components/schemas/TaxBehaviorOption'
            - type: 'null'
          description: >-
            The tax behavior of the price. If null, it defaults to the
            organization's default tax behavior.
        is_archived:
          type: boolean
          title: Is Archived
          description: Whether the price is archived and no longer available.
        product_id:
          type: string
          format: uuid4
          title: Product Id
          description: The ID of the product owning the price.
        seat_tiers:
          $ref: '#/components/schemas/ProductPriceSeatTiers-Output'
          description: Tiered pricing based on seat quantity
      type: object
      required:
        - created_at
        - modified_at
        - id
        - source
        - amount_type
        - price_currency
        - tax_behavior
        - is_archived
        - product_id
        - seat_tiers
      title: ProductPriceSeatBased
      description: A seat-based price for a product.
    ProductPriceUnitBased:
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
          description: The ID of the price.
        source:
          $ref: '#/components/schemas/ProductPriceSource'
          description: >-
            The source of the price . `catalog` is a predefined price, while
            `ad_hoc` is a price created dynamically on a Checkout session.
        amount_type:
          type: string
          const: unit_based
          title: Amount Type
        price_currency:
          type: string
          title: Price Currency
          description: The currency in which the customer will be charged.
        tax_behavior:
          anyOf:
            - $ref: '#/components/schemas/TaxBehaviorOption'
            - type: 'null'
          description: >-
            The tax behavior of the price. If null, it defaults to the
            organization's default tax behavior.
        is_archived:
          type: boolean
          title: Is Archived
          description: Whether the price is archived and no longer available.
        product_id:
          type: string
          format: uuid4
          title: Product Id
          description: The ID of the product owning the price.
        tiers:
          $ref: '#/components/schemas/Tiers'
          description: Tiered pricing based on the purchased unit quantity.
        minimum_units:
          anyOf:
            - type: integer
            - type: 'null'
          title: Minimum Units
          description: The minimum purchasable quantity (inclusive).
        unit_label:
          anyOf:
            - additionalProperties:
                additionalProperties:
                  type: string
                type: object
              type: object
              minLength: 1
              examples:
                - en:
                    '=1': seat
                    other: seats
            - type: 'null'
          title: Unit Label
          description: >-
            Per-locale unit nouns shown at checkout and on invoices. `null`
            defaults to "unit"/"units".
        maximum_units:
          anyOf:
            - type: integer
            - type: 'null'
          title: Maximum Units
          description: >-
            The maximum purchasable quantity, from the last tier's bound. `null`
            for unlimited.
          readOnly: true
      type: object
      required:
        - created_at
        - modified_at
        - id
        - source
        - amount_type
        - price_currency
        - tax_behavior
        - is_archived
        - product_id
        - tiers
        - minimum_units
        - unit_label
        - maximum_units
      title: ProductPriceUnitBased
      description: |-
        A unit-based price for a product: the buyer picks a quantity of units,
        pays for it up-front. On subscriptions, quantity changes are prorated.
    ProductPriceMeteredUnit:
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
          description: The ID of the price.
        source:
          $ref: '#/components/schemas/ProductPriceSource'
          description: >-
            The source of the price . `catalog` is a predefined price, while
            `ad_hoc` is a price created dynamically on a Checkout session.
        amount_type:
          type: string
          const: metered_unit
          title: Amount Type
        price_currency:
          type: string
          title: Price Currency
          description: The currency in which the customer will be charged.
        tax_behavior:
          anyOf:
            - $ref: '#/components/schemas/TaxBehaviorOption'
            - type: 'null'
          description: >-
            The tax behavior of the price. If null, it defaults to the
            organization's default tax behavior.
        is_archived:
          type: boolean
          title: Is Archived
          description: Whether the price is archived and no longer available.
        product_id:
          type: string
          format: uuid4
          title: Product Id
          description: The ID of the product owning the price.
        cap_amount:
          anyOf:
            - type: integer
            - type: 'null'
          title: Cap Amount
          description: >-
            The maximum amount in cents that can be charged, regardless of the
            number of units consumed.
        meter_id:
          type: string
          format: uuid4
          title: Meter Id
          description: The ID of the meter associated to the price.
        meter:
          $ref: '#/components/schemas/ProductPriceMeter'
          description: The meter associated to the price.
        unit_amount:
          type: string
          pattern: ^(?!^[-+.]*$)[+-]?0*\d*\.?\d*$
          title: Unit Amount
          description: The price per unit in cents.
      type: object
      required:
        - created_at
        - modified_at
        - id
        - source
        - amount_type
        - price_currency
        - tax_behavior
        - is_archived
        - product_id
        - cap_amount
        - meter_id
        - meter
        - unit_amount
      title: ProductPriceMeteredUnit
      description: A metered, usage-based, price for a product, with a fixed unit price.
    ProductPriceMeteredTiers:
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
          description: The ID of the price.
        source:
          $ref: '#/components/schemas/ProductPriceSource'
          description: >-
            The source of the price . `catalog` is a predefined price, while
            `ad_hoc` is a price created dynamically on a Checkout session.
        amount_type:
          type: string
          const: metered_tiers
          title: Amount Type
        price_currency:
          type: string
          title: Price Currency
          description: The currency in which the customer will be charged.
        tax_behavior:
          anyOf:
            - $ref: '#/components/schemas/TaxBehaviorOption'
            - type: 'null'
          description: >-
            The tax behavior of the price. If null, it defaults to the
            organization's default tax behavior.
        is_archived:
          type: boolean
          title: Is Archived
          description: Whether the price is archived and no longer available.
        product_id:
          type: string
          format: uuid4
          title: Product Id
          description: The ID of the product owning the price.
        cap_amount:
          anyOf:
            - type: integer
            - type: 'null'
          title: Cap Amount
          description: >-
            The maximum amount in cents that can be charged, regardless of the
            number of units consumed.
        meter_id:
          type: string
          format: uuid4
          title: Meter Id
          description: The ID of the meter associated to the price.
        meter:
          $ref: '#/components/schemas/ProductPriceMeter'
          description: The meter associated to the price.
        tiers:
          $ref: '#/components/schemas/Tiers'
          description: The pricing tiers based on consumed units.
      type: object
      required:
        - created_at
        - modified_at
        - id
        - source
        - amount_type
        - price_currency
        - tax_behavior
        - is_archived
        - product_id
        - cap_amount
        - meter_id
        - meter
        - tiers
      title: ProductPriceMeteredTiers
      description: A metered, usage-based, price for a product, billed from tiers.
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
    CustomField:
      oneOf:
        - $ref: '#/components/schemas/CustomFieldText'
        - $ref: '#/components/schemas/CustomFieldNumber'
        - $ref: '#/components/schemas/CustomFieldDate'
        - $ref: '#/components/schemas/CustomFieldCheckbox'
        - $ref: '#/components/schemas/CustomFieldSelect'
      discriminator:
        propertyName: type
        mapping:
          checkbox:
            $ref: '#/components/schemas/CustomFieldCheckbox'
          date:
            $ref: '#/components/schemas/CustomFieldDate'
          number:
            $ref: '#/components/schemas/CustomFieldNumber'
          select:
            $ref: '#/components/schemas/CustomFieldSelect'
          text:
            $ref: '#/components/schemas/CustomFieldText'
    TaxBehaviorOption:
      type: string
      enum:
        - location
        - inclusive
        - exclusive
      title: TaxBehaviorOption
    ProductPriceSeatTiers-Input:
      properties:
        seat_tier_type:
          $ref: '#/components/schemas/SeatTierType'
          description: >-
            How tiers are applied. 'volume' prices all seats at the matching
            tier's rate. 'graduated' prices each tier's range independently.
          default: volume
        tiers:
          items:
            $ref: '#/components/schemas/ProductPriceSeatTier'
          type: array
          minItems: 1
          title: Tiers
          description: List of pricing tiers
      type: object
      required:
        - tiers
      title: ProductPriceSeatTiers
      description: |-
        List of pricing tiers for seat-based pricing.

        The minimum and maximum seat limits are derived from the tiers:
        - minimum_seats = first tier's min_seats
        - maximum_seats = last tier's max_seats (None for unlimited)
    TiersInput:
      properties:
        type:
          $ref: '#/components/schemas/TierType'
        tiers:
          items:
            $ref: '#/components/schemas/TierInput'
          type: array
          minItems: 1
          title: Tiers
      type: object
      required:
        - type
        - tiers
      title: TiersInput
      description: |-
        Tiers submitted through the API. Kept apart from `Tiers` so tightening
        a rule here never stops a stored row from loading.
    BenefitCustomPublic:
      properties:
        id:
          type: string
          format: uuid4
          title: Id
          description: The ID of the benefit.
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
        type:
          type: string
          const: custom
          title: Type
        description:
          type: string
          title: Description
          description: The description of the benefit.
        selectable:
          type: boolean
          title: Selectable
          description: Whether the benefit is selectable when creating a product.
        deletable:
          type: boolean
          title: Deletable
          description: Whether the benefit is deletable.
        is_deleted:
          type: boolean
          title: Is Deleted
          description: Whether the benefit is deleted.
        organization_id:
          type: string
          format: uuid4
          title: Organization Id
          description: The ID of the organization owning the benefit.
      type: object
      required:
        - id
        - created_at
        - modified_at
        - type
        - description
        - selectable
        - deletable
        - is_deleted
        - organization_id
      title: BenefitCustomPublic
    BenefitDiscordPublic:
      properties:
        id:
          type: string
          format: uuid4
          title: Id
          description: The ID of the benefit.
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
        type:
          type: string
          const: discord
          title: Type
        description:
          type: string
          title: Description
          description: The description of the benefit.
        selectable:
          type: boolean
          title: Selectable
          description: Whether the benefit is selectable when creating a product.
        deletable:
          type: boolean
          title: Deletable
          description: Whether the benefit is deletable.
        is_deleted:
          type: boolean
          title: Is Deleted
          description: Whether the benefit is deleted.
        organization_id:
          type: string
          format: uuid4
          title: Organization Id
          description: The ID of the organization owning the benefit.
      type: object
      required:
        - id
        - created_at
        - modified_at
        - type
        - description
        - selectable
        - deletable
        - is_deleted
        - organization_id
      title: BenefitDiscordPublic
    BenefitGitHubRepositoryPublic:
      properties:
        id:
          type: string
          format: uuid4
          title: Id
          description: The ID of the benefit.
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
        type:
          type: string
          const: github_repository
          title: Type
        description:
          type: string
          title: Description
          description: The description of the benefit.
        selectable:
          type: boolean
          title: Selectable
          description: Whether the benefit is selectable when creating a product.
        deletable:
          type: boolean
          title: Deletable
          description: Whether the benefit is deletable.
        is_deleted:
          type: boolean
          title: Is Deleted
          description: Whether the benefit is deleted.
        organization_id:
          type: string
          format: uuid4
          title: Organization Id
          description: The ID of the organization owning the benefit.
      type: object
      required:
        - id
        - created_at
        - modified_at
        - type
        - description
        - selectable
        - deletable
        - is_deleted
        - organization_id
      title: BenefitGitHubRepositoryPublic
    BenefitDownloadablesPublic:
      properties:
        id:
          type: string
          format: uuid4
          title: Id
          description: The ID of the benefit.
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
        type:
          type: string
          const: downloadables
          title: Type
        description:
          type: string
          title: Description
          description: The description of the benefit.
        selectable:
          type: boolean
          title: Selectable
          description: Whether the benefit is selectable when creating a product.
        deletable:
          type: boolean
          title: Deletable
          description: Whether the benefit is deletable.
        is_deleted:
          type: boolean
          title: Is Deleted
          description: Whether the benefit is deleted.
        organization_id:
          type: string
          format: uuid4
          title: Organization Id
          description: The ID of the organization owning the benefit.
      type: object
      required:
        - id
        - created_at
        - modified_at
        - type
        - description
        - selectable
        - deletable
        - is_deleted
        - organization_id
      title: BenefitDownloadablesPublic
    BenefitLicenseKeysPublic:
      properties:
        id:
          type: string
          format: uuid4
          title: Id
          description: The ID of the benefit.
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
        type:
          type: string
          const: license_keys
          title: Type
        description:
          type: string
          title: Description
          description: The description of the benefit.
        selectable:
          type: boolean
          title: Selectable
          description: Whether the benefit is selectable when creating a product.
        deletable:
          type: boolean
          title: Deletable
          description: Whether the benefit is deletable.
        is_deleted:
          type: boolean
          title: Is Deleted
          description: Whether the benefit is deleted.
        organization_id:
          type: string
          format: uuid4
          title: Organization Id
          description: The ID of the organization owning the benefit.
      type: object
      required:
        - id
        - created_at
        - modified_at
        - type
        - description
        - selectable
        - deletable
        - is_deleted
        - organization_id
      title: BenefitLicenseKeysPublic
    BenefitFeatureFlagPublic:
      properties:
        id:
          type: string
          format: uuid4
          title: Id
          description: The ID of the benefit.
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
        type:
          type: string
          const: feature_flag
          title: Type
        description:
          type: string
          title: Description
          description: The description of the benefit.
        selectable:
          type: boolean
          title: Selectable
          description: Whether the benefit is selectable when creating a product.
        deletable:
          type: boolean
          title: Deletable
          description: Whether the benefit is deletable.
        is_deleted:
          type: boolean
          title: Is Deleted
          description: Whether the benefit is deleted.
        organization_id:
          type: string
          format: uuid4
          title: Organization Id
          description: The ID of the organization owning the benefit.
      type: object
      required:
        - id
        - created_at
        - modified_at
        - type
        - description
        - selectable
        - deletable
        - is_deleted
        - organization_id
      title: BenefitFeatureFlagPublic
    BenefitSlackSharedChannelPublic:
      properties:
        id:
          type: string
          format: uuid4
          title: Id
          description: The ID of the benefit.
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
        type:
          type: string
          const: slack_shared_channel
          title: Type
        description:
          type: string
          title: Description
          description: The description of the benefit.
        selectable:
          type: boolean
          title: Selectable
          description: Whether the benefit is selectable when creating a product.
        deletable:
          type: boolean
          title: Deletable
          description: Whether the benefit is deletable.
        is_deleted:
          type: boolean
          title: Is Deleted
          description: Whether the benefit is deleted.
        organization_id:
          type: string
          format: uuid4
          title: Organization Id
          description: The ID of the organization owning the benefit.
      type: object
      required:
        - id
        - created_at
        - modified_at
        - type
        - description
        - selectable
        - deletable
        - is_deleted
        - organization_id
      title: BenefitSlackSharedChannelPublic
    BenefitMeterCreditPublic:
      properties:
        id:
          type: string
          format: uuid4
          title: Id
          description: The ID of the benefit.
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
        type:
          type: string
          const: meter_credit
          title: Type
        description:
          type: string
          title: Description
          description: The description of the benefit.
        selectable:
          type: boolean
          title: Selectable
          description: Whether the benefit is selectable when creating a product.
        deletable:
          type: boolean
          title: Deletable
          description: Whether the benefit is deletable.
        is_deleted:
          type: boolean
          title: Is Deleted
          description: Whether the benefit is deleted.
        organization_id:
          type: string
          format: uuid4
          title: Organization Id
          description: The ID of the organization owning the benefit.
        properties:
          $ref: '#/components/schemas/BenefitMeterCreditPublicProperties'
      type: object
      required:
        - id
        - created_at
        - modified_at
        - type
        - description
        - selectable
        - deletable
        - is_deleted
        - organization_id
        - properties
      title: BenefitMeterCreditPublic
      description: |-
        A benefit of type `meter_credit`.

        Grants a number of units on a specific meter.
    ProductPriceSource:
      type: string
      enum:
        - catalog
        - ad_hoc
      title: ProductPriceSource
    ProductPriceSeatTiers-Output:
      properties:
        seat_tier_type:
          $ref: '#/components/schemas/SeatTierType'
          description: >-
            How tiers are applied. 'volume' prices all seats at the matching
            tier's rate. 'graduated' prices each tier's range independently.
          default: volume
        tiers:
          items:
            $ref: '#/components/schemas/ProductPriceSeatTier'
          type: array
          minItems: 1
          title: Tiers
          description: List of pricing tiers
        minimum_seats:
          type: integer
          title: Minimum Seats
          description: >-
            Minimum number of seats required for purchase, derived from first
            tier.
          readOnly: true
        maximum_seats:
          anyOf:
            - type: integer
            - type: 'null'
          title: Maximum Seats
          description: >-
            Maximum number of seats allowed for purchase, derived from last
            tier. None for unlimited.
          readOnly: true
      type: object
      required:
        - tiers
        - minimum_seats
        - maximum_seats
      title: ProductPriceSeatTiers
      description: |-
        List of pricing tiers for seat-based pricing.

        The minimum and maximum seat limits are derived from the tiers:
        - minimum_seats = first tier's min_seats
        - maximum_seats = last tier's max_seats (None for unlimited)
    Tiers:
      properties:
        type:
          $ref: '#/components/schemas/TierType'
        tiers:
          items:
            $ref: '#/components/schemas/Tier'
          type: array
          minItems: 1
          title: Tiers
      type: object
      required:
        - type
        - tiers
      title: Tiers
      description: |-
        The structure of the shared tiers JSONB column, used by every tiered
        price type. Purchasable quantity bounds live in the `minimum_units` and
        `maximum_units` columns, not here.
    ProductPriceMeter:
      properties:
        id:
          type: string
          format: uuid4
          title: Id
          description: The ID of the object.
        name:
          type: string
          title: Name
          description: The name of the meter.
        unit:
          $ref: '#/components/schemas/MeterUnit'
          description: The unit of the meter.
        custom_label:
          anyOf:
            - type: string
            - type: 'null'
          title: Custom Label
          description: The label for the custom unit.
        custom_multiplier:
          anyOf:
            - type: integer
            - type: 'null'
          title: Custom Multiplier
          description: The multiplier to convert from base unit to display scale.
      type: object
      required:
        - id
        - name
        - unit
        - custom_label
        - custom_multiplier
      title: ProductPriceMeter
      description: A meter associated to a metered price.
    CustomFieldText:
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
        type:
          type: string
          const: text
          title: Type
        slug:
          type: string
          title: Slug
          description: >-
            Identifier of the custom field. It'll be used as key when storing
            the value.
        name:
          type: string
          title: Name
          description: Name of the custom field.
        organization_id:
          type: string
          format: uuid4
          title: Organization Id
          description: The ID of the organization owning the custom field.
          examples:
            - 1dbfc517-0bbf-4301-9ba8-555ca42b9737
        properties:
          $ref: '#/components/schemas/CustomFieldTextProperties'
      type: object
      required:
        - created_at
        - modified_at
        - id
        - metadata
        - type
        - slug
        - name
        - organization_id
        - properties
      title: CustomFieldText
      description: Schema for a custom field of type text.
    CustomFieldNumber:
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
        type:
          type: string
          const: number
          title: Type
        slug:
          type: string
          title: Slug
          description: >-
            Identifier of the custom field. It'll be used as key when storing
            the value.
        name:
          type: string
          title: Name
          description: Name of the custom field.
        organization_id:
          type: string
          format: uuid4
          title: Organization Id
          description: The ID of the organization owning the custom field.
          examples:
            - 1dbfc517-0bbf-4301-9ba8-555ca42b9737
        properties:
          $ref: '#/components/schemas/CustomFieldNumberProperties'
      type: object
      required:
        - created_at
        - modified_at
        - id
        - metadata
        - type
        - slug
        - name
        - organization_id
        - properties
      title: CustomFieldNumber
      description: Schema for a custom field of type number.
    CustomFieldDate:
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
        type:
          type: string
          const: date
          title: Type
        slug:
          type: string
          title: Slug
          description: >-
            Identifier of the custom field. It'll be used as key when storing
            the value.
        name:
          type: string
          title: Name
          description: Name of the custom field.
        organization_id:
          type: string
          format: uuid4
          title: Organization Id
          description: The ID of the organization owning the custom field.
          examples:
            - 1dbfc517-0bbf-4301-9ba8-555ca42b9737
        properties:
          $ref: '#/components/schemas/CustomFieldDateProperties'
      type: object
      required:
        - created_at
        - modified_at
        - id
        - metadata
        - type
        - slug
        - name
        - organization_id
        - properties
      title: CustomFieldDate
      description: Schema for a custom field of type date.
    CustomFieldCheckbox:
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
        type:
          type: string
          const: checkbox
          title: Type
        slug:
          type: string
          title: Slug
          description: >-
            Identifier of the custom field. It'll be used as key when storing
            the value.
        name:
          type: string
          title: Name
          description: Name of the custom field.
        organization_id:
          type: string
          format: uuid4
          title: Organization Id
          description: The ID of the organization owning the custom field.
          examples:
            - 1dbfc517-0bbf-4301-9ba8-555ca42b9737
        properties:
          $ref: '#/components/schemas/CustomFieldCheckboxProperties'
      type: object
      required:
        - created_at
        - modified_at
        - id
        - metadata
        - type
        - slug
        - name
        - organization_id
        - properties
      title: CustomFieldCheckbox
      description: Schema for a custom field of type checkbox.
    CustomFieldSelect:
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
        type:
          type: string
          const: select
          title: Type
        slug:
          type: string
          title: Slug
          description: >-
            Identifier of the custom field. It'll be used as key when storing
            the value.
        name:
          type: string
          title: Name
          description: Name of the custom field.
        organization_id:
          type: string
          format: uuid4
          title: Organization Id
          description: The ID of the organization owning the custom field.
          examples:
            - 1dbfc517-0bbf-4301-9ba8-555ca42b9737
        properties:
          $ref: '#/components/schemas/CustomFieldSelectProperties'
      type: object
      required:
        - created_at
        - modified_at
        - id
        - metadata
        - type
        - slug
        - name
        - organization_id
        - properties
      title: CustomFieldSelect
      description: Schema for a custom field of type select.
    SeatTierType:
      type: string
      enum:
        - volume
        - graduated
      title: SeatTierType
    ProductPriceSeatTier:
      properties:
        min_seats:
          type: integer
          minimum: 1
          title: Min Seats
          description: Minimum number of seats (inclusive)
        max_seats:
          anyOf:
            - type: integer
              minimum: 1
            - type: 'null'
          title: Max Seats
          description: Maximum number of seats (inclusive). None for unlimited.
        price_per_seat:
          type: integer
          maximum: 9223372036854776000
          minimum: 0
          title: Price Per Seat
          description: Price per seat in cents for this tier
      type: object
      required:
        - min_seats
        - price_per_seat
      title: ProductPriceSeatTier
      description: A pricing tier for seat-based pricing.
    TierType:
      type: string
      enum:
        - volume
        - graduated
      title: TierType
    TierInput:
      properties:
        bound:
          anyOf:
            - type: integer
              exclusiveMinimum: 0
            - type: 'null'
          title: Bound
        unit_amount:
          anyOf:
            - type: number
              maximum: 9223372036854776000
              minimum: 0
            - type: string
              pattern: >-
                ^(?!^[-+.]*$)[+-]?0*(?:\d{0,19}|(?=[\d.]{1,32}0*$)\d{0,19}\.\d{0,12}0*$)
          title: Unit Amount
      type: object
      required:
        - unit_amount
      title: TierInput
      description: |-
        A tier submitted through the API. Rates stop at the reach of the
        BigInteger amount columns, with 12 decimal places.
    BenefitMeterCreditPublicProperties:
      properties:
        units:
          type: integer
          title: Units
        meter_id:
          type: string
          format: uuid4
          title: Meter Id
      type: object
      required:
        - units
        - meter_id
      title: BenefitMeterCreditPublicProperties
      description: Properties for a benefit of type `meter_credit`.
    Tier:
      properties:
        bound:
          anyOf:
            - type: integer
              exclusiveMinimum: 0
            - type: 'null'
          title: Bound
        unit_amount:
          type: string
          pattern: ^(?!^[-+.]*$)[+-]?0*\d*\.?\d*$
          title: Unit Amount
      type: object
      required:
        - unit_amount
      title: Tier
      description: |-
        A per-unit rate up to and including `bound`.

        Each tier starts where the previous one ended. The first starts at
        zero. `bound` is None on the last tier if it's unbounded. Rates are
        in cents and may be fractional.

        Rates carry no precision bound: this schema reads stored rows, and a
        bound tightened later would stop them loading. `TierInput` holds the
        rules new rates must meet.
    MeterUnit:
      type: string
      enum:
        - scalar
        - token
        - custom
      title: MeterUnit
    CustomFieldTextProperties:
      properties:
        form_label:
          type: string
          minLength: 1
          title: Form Label
        form_help_text:
          type: string
          minLength: 1
          title: Form Help Text
        form_placeholder:
          type: string
          minLength: 1
          title: Form Placeholder
        textarea:
          type: boolean
          title: Textarea
        min_length:
          type: integer
          maximum: 2147483647
          minimum: 0
          title: Min Length
        max_length:
          type: integer
          maximum: 2147483647
          minimum: 0
          title: Max Length
      type: object
      title: CustomFieldTextProperties
    CustomFieldNumberProperties:
      properties:
        form_label:
          type: string
          minLength: 1
          title: Form Label
        form_help_text:
          type: string
          minLength: 1
          title: Form Help Text
        form_placeholder:
          type: string
          minLength: 1
          title: Form Placeholder
        ge:
          type: integer
          maximum: 2147483647
          minimum: -2147483648
          title: Ge
        le:
          type: integer
          maximum: 2147483647
          minimum: -2147483648
          title: Le
      type: object
      title: CustomFieldNumberProperties
    CustomFieldDateProperties:
      properties:
        form_label:
          type: string
          minLength: 1
          title: Form Label
        form_help_text:
          type: string
          minLength: 1
          title: Form Help Text
        form_placeholder:
          type: string
          minLength: 1
          title: Form Placeholder
        ge:
          type: integer
          maximum: 2147483647
          minimum: -2147483648
          title: Ge
        le:
          type: integer
          maximum: 2147483647
          minimum: -2147483648
          title: Le
      type: object
      title: CustomFieldDateProperties
    CustomFieldCheckboxProperties:
      properties:
        form_label:
          type: string
          minLength: 1
          title: Form Label
        form_help_text:
          type: string
          minLength: 1
          title: Form Help Text
        form_placeholder:
          type: string
          minLength: 1
          title: Form Placeholder
      type: object
      title: CustomFieldCheckboxProperties
    CustomFieldSelectProperties:
      properties:
        form_label:
          type: string
          minLength: 1
          title: Form Label
        form_help_text:
          type: string
          minLength: 1
          title: Form Help Text
        form_placeholder:
          type: string
          minLength: 1
          title: Form Placeholder
        options:
          items:
            $ref: '#/components/schemas/CustomFieldSelectOption'
          type: array
          minItems: 1
          title: Options
      type: object
      required:
        - options
      title: CustomFieldSelectProperties
    CustomFieldSelectOption:
      properties:
        value:
          type: string
          minLength: 1
          title: Value
        label:
          type: string
          minLength: 1
          title: Label
      type: object
      required:
        - value
        - label
      title: CustomFieldSelectOption
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
