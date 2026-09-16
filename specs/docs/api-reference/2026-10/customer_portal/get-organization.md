> ## Documentation Index
> Fetch the complete documentation index at: https://polar.sh/docs/llms.txt
> Use this file to discover all available pages before exploring further.

# Get Organization

> Get a customer portal's organization by slug.



## OpenAPI

````yaml /openapi/2026-10.openapi.json get /v1/customer-portal/organizations/{slug}
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
  /v1/customer-portal/organizations/{slug}:
    get:
      tags:
        - customer_portal
        - organizations
        - public
      summary: Get Organization
      description: Get a customer portal's organization by slug.
      operationId: customer_portal:organizations:get
      parameters:
        - name: slug
          in: path
          required: true
          schema:
            type: string
            description: The organization slug.
            title: Slug
          description: The organization slug.
      responses:
        '200':
          description: Successful Response
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/CustomerOrganizationData'
        '404':
          description: Organization not found.
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
      x-codeSamples:
        - lang: python
          source: |
            from polar.v2026_10 import Polar

            polar = Polar("polar_oat_xxx")

            response = polar.customer_portal.organizations.get(
                'string',
            )
            print(response)
        - lang: typescript
          source: |
            import { createPolar } from "@polar-sh/sdk/2026-10";

            const polar = createPolar({
              accessToken: "polar_oat_xxx",
            });

            const response = await polar.customerPortal.organizations.get(
              "string",
            );
            console.log(response);
components:
  schemas:
    CustomerOrganizationData:
      properties:
        organization:
          $ref: '#/components/schemas/CustomerOrganization'
        products:
          items:
            $ref: '#/components/schemas/CustomerProduct'
          type: array
          title: Products
      type: object
      required:
        - organization
        - products
      title: CustomerOrganizationData
      description: Schema of an organization and related data for customer portal.
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
    CustomerOrganization:
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
        name:
          type: string
          title: Name
          description: Organization name shown in checkout, customer portal, emails etc.
        slug:
          type: string
          title: Slug
          description: >-
            Unique organization slug in checkout, customer portal and credit
            card statements.
        avatar_url:
          anyOf:
            - type: string
            - type: 'null'
          title: Avatar Url
          description: Avatar URL shown in checkout, customer portal, emails etc.
        proration_behavior:
          $ref: '#/components/schemas/SubscriptionProrationBehavior'
          description: >-
            Proration behavior applied when customer updates their subscription
            from the portal.
        allow_customer_updates:
          type: boolean
          title: Allow Customer Updates
          description: >-
            Whether customers can update their subscriptions from the customer
            portal.
        customer_portal_settings:
          $ref: '#/components/schemas/OrganizationCustomerPortalSettings'
          description: Settings related to the customer portal
        organization_features:
          $ref: '#/components/schemas/CustomerOrganizationFeatureSettings'
          description: Feature flags for the customer portal.
      type: object
      required:
        - created_at
        - modified_at
        - id
        - name
        - slug
        - avatar_url
        - proration_behavior
        - allow_customer_updates
        - customer_portal_settings
      title: CustomerOrganization
    CustomerProduct:
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
          description: List of available prices for this product.
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
          description: The medias associated to the product.
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
      title: CustomerProduct
      description: Schema of a product for customer portal.
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
    SubscriptionProrationBehavior:
      type: string
      enum:
        - invoice
        - prorate
        - next_period
        - reset
      title: SubscriptionProrationBehavior
    OrganizationCustomerPortalSettings:
      properties:
        usage:
          $ref: '#/components/schemas/CustomerPortalUsageSettings'
        subscription:
          $ref: '#/components/schemas/CustomerPortalSubscriptionSettings'
        customer:
          $ref: '#/components/schemas/CustomerPortalCustomerSettings'
      type: object
      required:
        - usage
        - subscription
      title: OrganizationCustomerPortalSettings
    CustomerOrganizationFeatureSettings:
      properties:
        member_model_enabled:
          type: boolean
          title: Member Model Enabled
          description: Whether the member model is enabled for this organization.
          default: false
        checkout_localization_enabled:
          type: boolean
          title: Checkout Localization Enabled
          description: Whether localization is enabled for this organization.
          default: false
      type: object
      title: CustomerOrganizationFeatureSettings
      description: Feature flags exposed to the customer portal.
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
    CustomerPortalUsageSettings:
      properties:
        show:
          type: boolean
          title: Show
      type: object
      required:
        - show
      title: CustomerPortalUsageSettings
    CustomerPortalSubscriptionSettings:
      properties:
        update_seats:
          type: boolean
          title: Update Seats
        update_plan:
          type: boolean
          title: Update Plan
        update_units:
          type: boolean
          title: Update Units
        pause:
          type: boolean
          title: Pause
      type: object
      required:
        - update_seats
        - update_plan
      title: CustomerPortalSubscriptionSettings
    CustomerPortalCustomerSettings:
      properties:
        allow_email_change:
          type: boolean
          title: Allow Email Change
      type: object
      title: CustomerPortalCustomerSettings
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
    TaxBehaviorOption:
      type: string
      enum:
        - location
        - inclusive
        - exclusive
      title: TaxBehaviorOption
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

````
