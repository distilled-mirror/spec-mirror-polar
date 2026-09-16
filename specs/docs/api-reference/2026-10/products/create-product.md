> ## Documentation Index
> Fetch the complete documentation index at: https://polar.sh/docs/llms.txt
> Use this file to discover all available pages before exploring further.

# Create Product

> Create a product.

**Scopes**: `products:write`



## OpenAPI

````yaml /openapi/2026-10.openapi.json post /v1/products/
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
  - name: mcp
    description: Endpoints supported by Polar's MCP server.
  - name: cli
    description: Endpoints exposed as commands by Polar's CLI.
paths:
  /v1/products/:
    post:
      tags:
        - products
        - public
        - mcp
        - cli
      summary: Create Product
      description: |-
        Create a product.

        **Scopes**: `products:write`
      operationId: products:create
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/ProductCreate'
      responses:
        '201':
          description: Product created.
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Product'
        '422':
          description: Validation Error
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/HTTPValidationError'
      security:
        - oidc:
            - products:write
        - pat:
            - products:write
        - oat:
            - products:write
      x-codeSamples:
        - lang: python
          source: |
            from polar.v2026_10 import Polar

            polar = Polar("polar_oat_xxx")

            response = polar.products.create(
                name='string',
                prices=[{'amount_type': 'fixed', 'price_amount': 0}],
                recurring_interval='day',
                recurring_interval_count=1,
            )
            print(response)
        - lang: typescript
          source: |
            import { createPolar } from "@polar-sh/sdk/2026-10";

            const polar = createPolar({
              accessToken: "polar_oat_xxx",
            });

            const response = await polar.products.create(
              {
                "name": "string",
                "prices": [
                  {
                    "amount_type": "fixed",
                    "price_amount": 0
                  }
                ],
                "recurring_interval": "day",
                "recurring_interval_count": 1
              },
            );
            console.log(response);
components:
  schemas:
    ProductCreate:
      oneOf:
        - $ref: '#/components/schemas/ProductCreateRecurring'
        - $ref: '#/components/schemas/ProductCreateOneTime'
    Product:
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
        metadata:
          $ref: '#/components/schemas/MetadataOutputType'
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
            $ref: '#/components/schemas/Benefit'
            title: Benefit
          type: array
          title: Benefits
          description: List of benefits granted by the product.
        medias:
          items:
            $ref: '#/components/schemas/ProductMediaFileRead'
          type: array
          title: Medias
          description: List of medias associated to the product.
        attached_custom_fields:
          items:
            $ref: '#/components/schemas/AttachedCustomField'
          type: array
          title: Attached Custom Fields
          description: List of custom fields attached to the product.
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
        - metadata
        - prices
        - benefits
        - medias
        - attached_custom_fields
      title: Product
      description: A product.
    HTTPValidationError:
      properties:
        detail:
          items:
            $ref: '#/components/schemas/ValidationError'
          type: array
          title: Detail
      type: object
      title: HTTPValidationError
    ProductCreateRecurring:
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
          maxLength: 64
          minLength: 3
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
          default: public
        prices:
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
          title: ProductPriceCreateList
          description: >-
            List of available prices for this product. It may combine at most
            one fixed price with one seat-based price (billed as `fixed +
            seat_charge`), or contain a single custom or free price, plus any
            number of metered prices. A free price cannot be combined with other
            prices, and a custom price cannot be combined with a fixed or
            seat-based price. Metered prices are not supported on one-time
            purchase products.
        medias:
          anyOf:
            - items:
                type: string
                format: uuid4
              type: array
            - type: 'null'
          title: Medias
          description: >-
            List of file IDs. Each one must be on the same organization as the
            product, of type `product_media` and correctly uploaded.
        attached_custom_fields:
          items:
            $ref: '#/components/schemas/AttachedCustomFieldCreate'
          type: array
          title: Attached Custom Fields
          description: List of custom fields to attach.
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
            The ID of the organization owning the product. **Required unless you
            use an organization token.**
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
        recurring_interval:
          $ref: '#/components/schemas/RecurringInterval'
          description: The recurring interval of the product.
        recurring_interval_count:
          type: integer
          maximum: 999
          minimum: 1
          title: Recurring Interval Count
          description: >-
            Number of interval units of the subscription. If this is set to 1
            the charge will happen every interval (e.g. every month), if set to
            2 it will be every other month, and so on.
          default: 1
        meter_interval:
          anyOf:
            - $ref: '#/components/schemas/RecurringInterval'
            - type: 'null'
          description: >-
            Optional meter cycle, independent of the billing interval. When set,
            overage settlement, meter resets and meter-credit grants run on this
            cadence rather than the billing interval — e.g. yearly billing with
            monthly credits. It must evenly divide the billing interval. If
            `None`, metered concerns follow the billing interval. **Once set, it
            can't be changed.**
        meter_interval_count:
          anyOf:
            - type: integer
              maximum: 999
              minimum: 1
            - type: 'null'
          title: Meter Interval Count
          description: >-
            Number of meter interval units. Defaults to 1 when `meter_interval`
            is set. Ignored when `meter_interval` is `None`.
      type: object
      required:
        - name
        - prices
        - recurring_interval
      title: ProductCreateRecurring
    ProductCreateOneTime:
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
          maxLength: 64
          minLength: 3
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
          default: public
        prices:
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
          title: ProductPriceCreateList
          description: >-
            List of available prices for this product. It may combine at most
            one fixed price with one seat-based price (billed as `fixed +
            seat_charge`), or contain a single custom or free price, plus any
            number of metered prices. A free price cannot be combined with other
            prices, and a custom price cannot be combined with a fixed or
            seat-based price. Metered prices are not supported on one-time
            purchase products.
        medias:
          anyOf:
            - items:
                type: string
                format: uuid4
              type: array
            - type: 'null'
          title: Medias
          description: >-
            List of file IDs. Each one must be on the same organization as the
            product, of type `product_media` and correctly uploaded.
        attached_custom_fields:
          items:
            $ref: '#/components/schemas/AttachedCustomFieldCreate'
          type: array
          title: Attached Custom Fields
          description: List of custom fields to attach.
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
            The ID of the organization owning the product. **Required unless you
            use an organization token.**
        recurring_interval:
          type: 'null'
          title: Recurring Interval
          description: States that the product is a one-time purchase.
        recurring_interval_count:
          type: 'null'
          title: Recurring Interval Count
          description: One-time products don't have a recurring interval count.
      type: object
      required:
        - name
        - prices
      title: ProductCreateOneTime
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
    MetadataOutputType:
      additionalProperties:
        anyOf:
          - type: string
          - type: integer
          - type: number
          - type: boolean
      type: object
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
    Benefit:
      oneOf:
        - $ref: '#/components/schemas/BenefitCustom'
        - $ref: '#/components/schemas/BenefitDiscord'
        - $ref: '#/components/schemas/BenefitGitHubRepository'
        - $ref: '#/components/schemas/BenefitDownloadables'
        - $ref: '#/components/schemas/BenefitLicenseKeys'
        - $ref: '#/components/schemas/BenefitMeterCredit'
        - $ref: '#/components/schemas/BenefitFeatureFlag'
        - $ref: '#/components/schemas/BenefitSlackSharedChannel'
      discriminator:
        propertyName: type
        mapping:
          custom:
            $ref: '#/components/schemas/BenefitCustom'
          discord:
            $ref: '#/components/schemas/BenefitDiscord'
          downloadables:
            $ref: '#/components/schemas/BenefitDownloadables'
          feature_flag:
            $ref: '#/components/schemas/BenefitFeatureFlag'
          github_repository:
            $ref: '#/components/schemas/BenefitGitHubRepository'
          license_keys:
            $ref: '#/components/schemas/BenefitLicenseKeys'
          meter_credit:
            $ref: '#/components/schemas/BenefitMeterCredit'
          slack_shared_channel:
            $ref: '#/components/schemas/BenefitSlackSharedChannel'
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
    AttachedCustomFieldCreate:
      properties:
        custom_field_id:
          type: string
          format: uuid4
          title: Custom Field Id
          description: ID of the custom field to attach.
        required:
          type: boolean
          title: Required
          description: Whether the value is required for this custom field.
      type: object
      required:
        - custom_field_id
        - required
      title: AttachedCustomFieldCreate
      description: Schema to attach a custom field to a resource.
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
    BenefitCustom:
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
        metadata:
          $ref: '#/components/schemas/MetadataOutputType'
        visibility:
          $ref: '#/components/schemas/BenefitVisibility'
          description: The visibility of the benefit in the customer portal.
        properties:
          $ref: '#/components/schemas/BenefitCustomProperties'
        visibility_configurable:
          type: boolean
          title: Visibility Configurable
          readOnly: true
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
        - metadata
        - visibility
        - properties
        - visibility_configurable
      title: BenefitCustom
      description: |-
        A benefit of type `custom`.

        Use it to grant any kind of benefit that doesn't fit in the other types.
    BenefitDiscord:
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
        metadata:
          $ref: '#/components/schemas/MetadataOutputType'
        visibility:
          $ref: '#/components/schemas/BenefitVisibility'
          description: The visibility of the benefit in the customer portal.
        properties:
          $ref: '#/components/schemas/BenefitDiscordProperties'
        visibility_configurable:
          type: boolean
          title: Visibility Configurable
          readOnly: true
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
        - metadata
        - visibility
        - properties
        - visibility_configurable
      title: BenefitDiscord
      description: |-
        A benefit of type `discord`.

        Use it to automatically invite your backers to a Discord server.
    BenefitGitHubRepository:
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
        metadata:
          $ref: '#/components/schemas/MetadataOutputType'
        visibility:
          $ref: '#/components/schemas/BenefitVisibility'
          description: The visibility of the benefit in the customer portal.
        properties:
          $ref: '#/components/schemas/BenefitGitHubRepositoryProperties'
        visibility_configurable:
          type: boolean
          title: Visibility Configurable
          readOnly: true
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
        - metadata
        - visibility
        - properties
        - visibility_configurable
      title: BenefitGitHubRepository
      description: >-
        A benefit of type `github_repository`.


        Use it to automatically invite your backers to a private GitHub
        repository.
    BenefitDownloadables:
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
        metadata:
          $ref: '#/components/schemas/MetadataOutputType'
        visibility:
          $ref: '#/components/schemas/BenefitVisibility'
          description: The visibility of the benefit in the customer portal.
        properties:
          $ref: '#/components/schemas/BenefitDownloadablesProperties'
        visibility_configurable:
          type: boolean
          title: Visibility Configurable
          readOnly: true
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
        - metadata
        - visibility
        - properties
        - visibility_configurable
      title: BenefitDownloadables
    BenefitLicenseKeys:
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
        metadata:
          $ref: '#/components/schemas/MetadataOutputType'
        visibility:
          $ref: '#/components/schemas/BenefitVisibility'
          description: The visibility of the benefit in the customer portal.
        properties:
          $ref: '#/components/schemas/BenefitLicenseKeysProperties'
        visibility_configurable:
          type: boolean
          title: Visibility Configurable
          readOnly: true
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
        - metadata
        - visibility
        - properties
        - visibility_configurable
      title: BenefitLicenseKeys
    BenefitMeterCredit:
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
        metadata:
          $ref: '#/components/schemas/MetadataOutputType'
        visibility:
          $ref: '#/components/schemas/BenefitVisibility'
          description: The visibility of the benefit in the customer portal.
        properties:
          $ref: '#/components/schemas/BenefitMeterCreditProperties'
        visibility_configurable:
          type: boolean
          title: Visibility Configurable
          readOnly: true
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
        - metadata
        - visibility
        - properties
        - visibility_configurable
      title: BenefitMeterCredit
      description: |-
        A benefit of type `meter_unit`.

        Use it to grant a number of units on a specific meter.
    BenefitFeatureFlag:
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
        metadata:
          $ref: '#/components/schemas/MetadataOutputType'
        visibility:
          $ref: '#/components/schemas/BenefitVisibility'
          description: The visibility of the benefit in the customer portal.
        properties:
          $ref: '#/components/schemas/BenefitFeatureFlagProperties'
        visibility_configurable:
          type: boolean
          title: Visibility Configurable
          readOnly: true
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
        - metadata
        - visibility
        - properties
        - visibility_configurable
      title: BenefitFeatureFlag
      description: |-
        A benefit of type `feature_flag`.

        Use it to grant feature flags with key-value metadata
        that can be queried via the API and webhooks.
    BenefitSlackSharedChannel:
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
        metadata:
          $ref: '#/components/schemas/MetadataOutputType'
        visibility:
          $ref: '#/components/schemas/BenefitVisibility'
          description: The visibility of the benefit in the customer portal.
        properties:
          $ref: '#/components/schemas/BenefitSlackSharedChannelProperties'
        visibility_configurable:
          type: boolean
          title: Visibility Configurable
          readOnly: true
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
        - metadata
        - visibility
        - properties
        - visibility_configurable
      title: BenefitSlackSharedChannel
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
    BenefitVisibility:
      type: string
      enum:
        - draft
        - private
        - public
      title: Visibility
    BenefitCustomProperties:
      properties:
        note:
          anyOf:
            - anyOf:
                - type: string
                - type: 'null'
              description: >-
                Private note to be shared with customers who have this benefit
                granted.
            - type: 'null'
          title: Note
      type: object
      required:
        - note
      title: BenefitCustomProperties
      description: Properties for a benefit of type `custom`.
    BenefitDiscordProperties:
      properties:
        guild_id:
          type: string
          title: Guild Id
          description: The ID of the Discord server.
        role_id:
          type: string
          title: Role Id
          description: The ID of the Discord role to grant.
        kick_member:
          type: boolean
          title: Kick Member
          description: Whether to kick the member from the Discord server on revocation.
        guild_token:
          type: string
          title: Guild Token
          readOnly: true
      type: object
      required:
        - guild_id
        - role_id
        - kick_member
        - guild_token
      title: BenefitDiscordProperties
      description: Properties for a benefit of type `discord`.
    BenefitGitHubRepositoryProperties:
      properties:
        repository_owner:
          type: string
          title: Repository Owner
          description: The owner of the repository.
          examples:
            - polarsource
        repository_name:
          type: string
          title: Repository Name
          description: The name of the repository.
          examples:
            - private_repo
        permission:
          type: string
          enum:
            - pull
            - triage
            - push
            - maintain
            - admin
          title: Permission
          description: >-
            The permission level to grant. Read more about roles and their
            permissions on [GitHub
            documentation](https://docs.github.com/en/organizations/managing-user-access-to-your-organizations-repositories/managing-repository-roles/repository-roles-for-an-organization#permissions-for-each-role).
      type: object
      required:
        - repository_owner
        - repository_name
        - permission
      title: BenefitGitHubRepositoryProperties
      description: Properties for a benefit of type `github_repository`.
    BenefitDownloadablesProperties:
      properties:
        archived:
          additionalProperties:
            type: boolean
          propertyNames:
            format: uuid4
          type: object
          title: Archived
        files:
          items:
            type: string
            format: uuid4
          type: array
          title: Files
      type: object
      required:
        - archived
        - files
      title: BenefitDownloadablesProperties
    BenefitLicenseKeysProperties:
      properties:
        prefix:
          anyOf:
            - type: string
            - type: 'null'
          title: Prefix
        expires:
          anyOf:
            - $ref: '#/components/schemas/BenefitLicenseKeyExpirationProperties'
            - type: 'null'
        activations:
          anyOf:
            - $ref: '#/components/schemas/BenefitLicenseKeyActivationProperties'
            - type: 'null'
        limit_usage:
          anyOf:
            - type: integer
            - type: 'null'
          title: Limit Usage
      type: object
      required:
        - prefix
        - expires
        - activations
        - limit_usage
      title: BenefitLicenseKeysProperties
    BenefitMeterCreditProperties:
      properties:
        units:
          type: integer
          title: Units
        rollover:
          type: boolean
          title: Rollover
        meter_id:
          type: string
          format: uuid4
          title: Meter Id
      type: object
      required:
        - units
        - rollover
        - meter_id
      title: BenefitMeterCreditProperties
      description: Properties for a benefit of type `meter_unit`.
    BenefitFeatureFlagProperties:
      properties: {}
      type: object
      title: BenefitFeatureFlagProperties
      description: Properties for a benefit of type `feature_flag`.
    BenefitSlackSharedChannelProperties:
      properties:
        slack_integration_id:
          type: string
          format: uuid4
          title: Slack Integration Id
          description: Polar Slack integration linked to this benefit.
        channel_name_template:
          type: string
          maxLength: 80
          minLength: 1
          title: Channel Name Template
          description: >-
            Template for the channel name. Supports placeholders:
            {customer_name}, {customer_email_local}, and {metadata.<key>} for
            any value stored in customer user metadata.
        private:
          type: boolean
          title: Private
          description: Create the channel as private (recommended).
          default: true
        welcome_message:
          anyOf:
            - type: string
              maxLength: 4000
            - type: 'null'
          title: Welcome Message
          description: Optional message posted to the channel right after creation.
        archive_on_revoke:
          type: boolean
          title: Archive On Revoke
          description: Archive the channel when the benefit is revoked.
          default: true
        team_invitees:
          items:
            type: string
          type: array
          title: Team Invitees
          description: >-
            Slack user IDs from the merchant workspace to invite to every
            channel created for this benefit.
      type: object
      required:
        - slack_integration_id
        - channel_name_template
      title: BenefitSlackSharedChannelProperties
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
    BenefitLicenseKeyExpirationProperties:
      properties:
        ttl:
          type: integer
          exclusiveMinimum: 0
          title: Ttl
        timeframe:
          type: string
          enum:
            - year
            - month
            - day
          title: Timeframe
      type: object
      required:
        - ttl
        - timeframe
      title: BenefitLicenseKeyExpirationProperties
    BenefitLicenseKeyActivationProperties:
      properties:
        limit:
          type: integer
          title: Limit
        enable_customer_admin:
          type: boolean
          title: Enable Customer Admin
      type: object
      required:
        - limit
        - enable_customer_admin
      title: BenefitLicenseKeyActivationProperties
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
