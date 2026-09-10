> ## Documentation Index
> Fetch the complete documentation index at: https://polar.sh/docs/llms.txt
> Use this file to discover all available pages before exploring further.

# List Organizations

> List organizations.

**Scopes**: `organizations:read` `organizations:write`



## OpenAPI

````yaml /openapi/2026-04.openapi.json get /v1/organizations/
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
  /v1/organizations/:
    get:
      tags:
        - organizations
        - public
      summary: List Organizations
      description: |-
        List organizations.

        **Scopes**: `organizations:read` `organizations:write`
      operationId: organizations:list
      parameters:
        - name: slug
          in: query
          required: false
          schema:
            anyOf:
              - type: string
              - type: 'null'
            description: Filter by slug.
            title: Slug
          description: Filter by slug.
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
        - name: sorting
          in: query
          required: false
          schema:
            anyOf:
              - type: array
                items:
                  $ref: '#/components/schemas/OrganizationSortProperty'
              - type: 'null'
            description: >-
              Sorting criterion. Several criteria can be used simultaneously and
              will be applied in order. Add a minus sign `-` before the criteria
              name to sort by descending order.
            default:
              - created_at
            title: Sorting
          description: >-
            Sorting criterion. Several criteria can be used simultaneously and
            will be applied in order. Add a minus sign `-` before the criteria
            name to sort by descending order.
      responses:
        '200':
          description: Successful Response
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/ListResource_Organization_'
        '422':
          description: Validation Error
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/HTTPValidationError'
      security:
        - oidc:
            - organizations:read
            - organizations:write
        - pat:
            - organizations:read
            - organizations:write
        - oat:
            - organizations:read
            - organizations:write
      x-codeSamples:
        - lang: python
          source: |
            from polar.v2026_04 import Polar

            polar = Polar("polar_oat_xxx")

            for item in polar.organizations.iter_list(
                page=1,
                limit=10,
                sorting=['created_at'],
            ):
                print(item)
        - lang: typescript
          source: |
            import { createPolar } from "@polar-sh/sdk/2026-04";

            const polar = createPolar({
              accessToken: "polar_oat_xxx",
            });

            for await (const item of polar.organizations.iterList(
              {
                "page": 1,
                "limit": 10,
                "sorting": [
                  "created_at"
                ]
              },
            )) {
              console.log(item);
            }
components:
  schemas:
    OrganizationSortProperty:
      type: string
      enum:
        - created_at
        - '-created_at'
        - slug
        - '-slug'
        - name
        - '-name'
        - next_review_threshold
        - '-next_review_threshold'
        - days_in_status
        - '-days_in_status'
      title: OrganizationSortProperty
    ListResource_Organization_:
      properties:
        items:
          items:
            $ref: '#/components/schemas/Organization'
          type: array
          title: Items
        pagination:
          $ref: '#/components/schemas/Pagination'
      type: object
      required:
        - items
        - pagination
      title: ListResource[Organization]
    HTTPValidationError:
      properties:
        detail:
          items:
            $ref: '#/components/schemas/ValidationError'
          type: array
          title: Detail
      type: object
      title: HTTPValidationError
    Organization:
      properties:
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
        email:
          anyOf:
            - type: string
            - type: 'null'
          title: Email
          description: Public support email.
        website:
          anyOf:
            - type: string
            - type: 'null'
          title: Website
          description: Official website of the organization.
        socials:
          items:
            $ref: '#/components/schemas/OrganizationSocialLink'
          type: array
          title: Socials
          description: Links to social profiles.
        status:
          $ref: '#/components/schemas/OrganizationStatus'
          description: Current organization status
        details_submitted_at:
          anyOf:
            - type: string
              format: date-time
            - type: 'null'
          title: Details Submitted At
          description: When the business details were submitted for review.
        onboarding_resubmission_requested_at:
          anyOf:
            - type: string
              format: date-time
            - type: 'null'
          title: Onboarding Resubmission Requested At
          description: >-
            When Polar requested that the organization review and resubmit its
            onboarding information, if applicable.
        sso_enforced:
          type: boolean
          title: Sso Enforced
          description: >-
            Whether members must access this organization through its SSO
            connection.
        default_presentment_currency:
          type: string
          title: Default Presentment Currency
          description: >-
            Default presentment currency. Used as fallback in checkout and
            customer portal, if the customer's local currency is not available.
        default_tax_behavior:
          $ref: '#/components/schemas/TaxBehaviorOption'
          description: Default tax behavior applied on products.
        feature_settings:
          anyOf:
            - $ref: '#/components/schemas/OrganizationFeatureSettings'
            - type: 'null'
          description: Organization feature settings
        subscription_settings:
          $ref: '#/components/schemas/OrganizationSubscriptionSettings'
          description: Settings related to subscriptions management
        customer_email_settings:
          $ref: '#/components/schemas/OrganizationCustomerEmailSettings'
          description: Settings related to customer emails
        customer_portal_settings:
          $ref: '#/components/schemas/OrganizationCustomerPortalSettings'
          description: Settings related to the customer portal
        dispute_settings:
          $ref: '#/components/schemas/OrganizationDisputeSettings'
          description: Settings related to disputes
        embed_hosts:
          items:
            type: string
          type: array
          title: Embed Hosts
          description: >-
            Hosts allowed to embed this organization's checkout. An entry is a
            host and an optional port, without a scheme: HTTPS is always
            allowed, and HTTP too for local hosts — `localhost`, any
            `.localhost` or `.local` name, and loopback or private addresses.
            `*.example.com` matches any subdomain, but not `example.com` itself.
            An app origin such as `chrome-extension://abcdef` carries its
            scheme, having no host to match on.
        country:
          anyOf:
            - type: string
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
            - type: 'null'
          description: Two-letter country code (ISO 3166-1 alpha-2).
        account_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Account Id
          description: ID of the transactions account.
        payout_account_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Payout Account Id
          description: ID of the payout account.
        capabilities:
          $ref: '#/components/schemas/OrganizationCapabilities'
          description: Capabilities currently granted to the organization.
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
        - email
        - website
        - socials
        - status
        - details_submitted_at
        - onboarding_resubmission_requested_at
        - sso_enforced
        - default_presentment_currency
        - default_tax_behavior
        - feature_settings
        - subscription_settings
        - customer_email_settings
        - customer_portal_settings
        - dispute_settings
        - embed_hosts
        - account_id
        - payout_account_id
        - capabilities
      title: Organization
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
    SubscriptionProrationBehavior:
      type: string
      enum:
        - invoice
        - prorate
        - next_period
        - reset
      title: SubscriptionProrationBehavior
    OrganizationSocialLink:
      properties:
        platform:
          $ref: '#/components/schemas/OrganizationSocialPlatforms'
          description: The social platform of the URL
        url:
          type: string
          maxLength: 2083
          minLength: 1
          format: uri
          title: Url
          description: The URL to the organization profile
      type: object
      required:
        - platform
        - url
      title: OrganizationSocialLink
    OrganizationStatus:
      type: string
      enum:
        - created
        - review
        - snoozed
        - denied
        - active
        - blocked
        - offboarding
        - offboarded
      title: OrganizationStatus
    TaxBehaviorOption:
      type: string
      enum:
        - location
        - inclusive
        - exclusive
      title: TaxBehaviorOption
    OrganizationFeatureSettings:
      properties:
        issue_funding_enabled:
          type: boolean
          title: Issue Funding Enabled
          description: If this organization has issue funding enabled
          default: false
        seat_based_pricing_enabled:
          type: boolean
          title: Seat Based Pricing Enabled
          description: If this organization has seat-based pricing enabled
          default: false
        wallets_enabled:
          type: boolean
          title: Wallets Enabled
          description: If this organization has Wallets enabled
          default: false
        member_model_enabled:
          type: boolean
          title: Member Model Enabled
          description: If this organization has the Member model enabled
          default: false
        checkout_localization_enabled:
          type: boolean
          title: Checkout Localization Enabled
          description: If this organization has checkout localization enabled
          default: false
        overview_metrics:
          anyOf:
            - items:
                type: string
              type: array
            - type: 'null'
          title: Overview Metrics
          description: Ordered list of metric slugs shown on the dashboard overview.
        reset_proration_behavior_enabled:
          type: boolean
          title: Reset Proration Behavior Enabled
          description: If this organization has access to reset proration behavior.
          default: false
        off_session_charges_enabled:
          type: boolean
          title: Off Session Charges Enabled
          description: >-
            If this organization can create and finalize draft orders via the
            API (off-session charges against a saved payment method).
          default: false
        meter_cycling_enabled:
          type: boolean
          title: Meter Cycling Enabled
          description: >-
            If this organization can set a separate meter cycle on recurring
            products (a meter interval independent of the billing interval).
          default: false
        slack_benefit_enabled:
          type: boolean
          title: Slack Benefit Enabled
          description: Enables the slack shared channel benefit
          default: false
        preview_access_enabled:
          type: boolean
          title: Preview Access Enabled
          description: If this organization has preview access to new features enabled
          default: false
        disputes_enabled:
          type: boolean
          title: Disputes Enabled
          description: If this organization has the disputes dashboard enabled
          default: false
        sso_enabled:
          type: boolean
          title: Sso Enabled
          description: If this organization has single sign-on configuration enabled
          default: false
        dispute_auto_accept_enabled:
          type: boolean
          title: Dispute Auto Accept Enabled
          description: >-
            If this organization can set a threshold below which Polar concedes
            disputes on its behalf. Requires `disputes_enabled`.
          default: false
        compass_enabled:
          type: boolean
          title: Compass Enabled
          description: >-
            If this organization has the split product navigation (Billing /
            Compass / Customers) enabled in the dashboard
          default: false
        merchant_migration_enabled:
          type: boolean
          title: Merchant Migration Enabled
          description: >-
            If this organization can migrate its billing from another provider
            (e.g. Stripe) to Polar.
          default: false
      type: object
      title: OrganizationFeatureSettings
    OrganizationSubscriptionSettings:
      properties:
        allow_multiple_subscriptions:
          type: boolean
          title: Allow Multiple Subscriptions
        proration_behavior:
          type: string
          enum:
            - invoice
            - prorate
            - next_period
          title: PublicSubscriptionProrationBehavior
        benefit_revocation_grace_period:
          type: integer
          title: Benefit Revocation Grace Period
        prevent_trial_abuse:
          type: boolean
          title: Prevent Trial Abuse
        allow_customer_updates:
          type: boolean
          title: Allow Customer Updates
      type: object
      required:
        - allow_multiple_subscriptions
        - proration_behavior
        - benefit_revocation_grace_period
        - prevent_trial_abuse
        - allow_customer_updates
      title: OrganizationSubscriptionSettings
    OrganizationCustomerEmailSettings:
      properties:
        order_confirmation:
          type: boolean
          title: Order Confirmation
        payment_method_expiration_reminder:
          type: boolean
          title: Payment Method Expiration Reminder
        subscription_cancellation:
          type: boolean
          title: Subscription Cancellation
        subscription_confirmation:
          type: boolean
          title: Subscription Confirmation
        subscription_cycled:
          type: boolean
          title: Subscription Cycled
        subscription_cycled_after_trial:
          type: boolean
          title: Subscription Cycled After Trial
        subscription_past_due:
          type: boolean
          title: Subscription Past Due
        subscription_paused:
          type: boolean
          title: Subscription Paused
        subscription_resumed:
          type: boolean
          title: Subscription Resumed
        subscription_renewal_reminder:
          type: boolean
          title: Subscription Renewal Reminder
        subscription_revoked:
          type: boolean
          title: Subscription Revoked
        subscription_trial_conversion_reminder:
          type: boolean
          title: Subscription Trial Conversion Reminder
        subscription_uncanceled:
          type: boolean
          title: Subscription Uncanceled
        subscription_updated:
          type: boolean
          title: Subscription Updated
      type: object
      required:
        - order_confirmation
        - payment_method_expiration_reminder
        - subscription_cancellation
        - subscription_confirmation
        - subscription_cycled
        - subscription_cycled_after_trial
        - subscription_past_due
        - subscription_paused
        - subscription_resumed
        - subscription_renewal_reminder
        - subscription_revoked
        - subscription_trial_conversion_reminder
        - subscription_uncanceled
        - subscription_updated
      title: OrganizationCustomerEmailSettings
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
    OrganizationDisputeSettings:
      properties:
        auto_accept_below_amount:
          anyOf:
            - type: integer
            - type: 'null'
          title: Auto Accept Below Amount
      type: object
      required:
        - auto_accept_below_amount
      title: OrganizationDisputeSettings
      description: '`auto_accept_below_amount` is in Polar''s settlement currency (USD).'
    OrganizationCapabilities:
      properties:
        checkout_payments:
          type: boolean
          title: Checkout Payments
          description: Whether the organization can accept new checkout payments.
        subscription_renewals:
          type: boolean
          title: Subscription Renewals
          description: Whether the organization can process subscription renewals.
        payouts:
          type: boolean
          title: Payouts
          description: Whether the organization can withdraw its balance.
        refunds:
          type: boolean
          title: Refunds
          description: Whether the organization can issue refunds.
        api_access:
          type: boolean
          title: Api Access
          description: Whether the organization can access the API.
        dashboard_access:
          type: boolean
          title: Dashboard Access
          description: Whether the organization can access the dashboard.
      type: object
      required:
        - checkout_payments
        - subscription_renewals
        - payouts
        - refunds
        - api_access
        - dashboard_access
      title: OrganizationCapabilities
    OrganizationSocialPlatforms:
      type: string
      enum:
        - x
        - github
        - facebook
        - instagram
        - youtube
        - tiktok
        - linkedin
        - threads
        - discord
        - other
      title: OrganizationSocialPlatforms
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
