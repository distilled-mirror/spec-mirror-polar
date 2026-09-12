> ## Documentation Index
> Fetch the complete documentation index at: https://polar.sh/docs/llms.txt
> Use this file to discover all available pages before exploring further.

# Update Organization

> Update an organization.

**Scopes**: `organizations:write`



## OpenAPI

````yaml /openapi/2026-04.openapi.json patch /v1/organizations/{id}
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
  /v1/organizations/{id}:
    patch:
      tags:
        - organizations
        - public
      summary: Update Organization
      description: |-
        Update an organization.

        **Scopes**: `organizations:write`
      operationId: organizations:update
      parameters:
        - name: id
          in: path
          required: true
          schema:
            type: string
            format: uuid4
            examples:
              - 1dbfc517-0bbf-4301-9ba8-555ca42b9737
            description: The organization ID.
            title: Id
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/OrganizationUpdate'
      responses:
        '200':
          description: Organization updated.
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Organization'
        '403':
          description: >-
            You don't have the permission to update this organization, or
            dispute auto-accept isn't enabled for it.
          content:
            application/json:
              schema:
                anyOf:
                  - $ref: '#/components/schemas/NotPermitted'
                  - $ref: '#/components/schemas/DisputeAutoAcceptNotEnabled'
                title: Response 403 Organizations:Update
        '404':
          description: Organization not found.
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/ResourceNotFound'
        '409':
          description: Cannot enforce SSO without an enabled connection.
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/SSOEnforcementRequiresConnection'
        '422':
          description: Validation Error
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/HTTPValidationError'
      security:
        - oidc:
            - organizations:write
        - pat:
            - organizations:write
        - oat:
            - organizations:write
      x-codeSamples:
        - lang: python
          source: |
            from polar.v2026_04 import Polar

            polar = Polar("polar_oat_xxx")

            response = polar.organizations.update(
                '1dbfc517-0bbf-4301-9ba8-555ca42b9737',
            )
            print(response)
        - lang: typescript
          source: |
            import { createPolar } from "@polar-sh/sdk/2026-04";

            const polar = createPolar({
              accessToken: "polar_oat_xxx",
            });

            const response = await polar.organizations.update(
              "1dbfc517-0bbf-4301-9ba8-555ca42b9737",
              {},
            );
            console.log(response);
components:
  schemas:
    OrganizationUpdate:
      properties:
        name:
          anyOf:
            - type: string
              minLength: 3
            - type: 'null'
          title: Name
        avatar_url:
          anyOf:
            - type: string
              maxLength: 2083
              minLength: 1
              format: uri
            - type: 'null'
          title: Avatar Url
        email:
          anyOf:
            - type: string
              format: email
            - type: 'null'
          title: Email
          description: Public support email.
        website:
          anyOf:
            - type: string
              maxLength: 2083
              minLength: 1
              format: uri
            - type: 'null'
          title: Website
          description: Official website of the organization.
        socials:
          anyOf:
            - items:
                $ref: '#/components/schemas/OrganizationSocialLink'
              type: array
            - type: 'null'
          title: Socials
          description: Links to social profiles.
        details:
          anyOf:
            - $ref: '#/components/schemas/OrganizationDetails'
            - type: 'null'
          description: >-
            Additional, private, business details Polar needs about active
            organizations for compliance (KYC).
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
            - type: 'null'
          description: Two-letter country code (ISO 3166-1 alpha-2).
        feature_settings:
          anyOf:
            - $ref: '#/components/schemas/OrganizationFeatureSettingsUpdate'
            - type: 'null'
        subscription_settings:
          anyOf:
            - $ref: '#/components/schemas/OrganizationSubscriptionSettings'
            - type: 'null'
        customer_email_settings:
          anyOf:
            - $ref: '#/components/schemas/OrganizationCustomerEmailSettings'
            - type: 'null'
        customer_portal_settings:
          anyOf:
            - $ref: '#/components/schemas/OrganizationCustomerPortalSettings'
            - type: 'null'
        dispute_settings:
          anyOf:
            - $ref: '#/components/schemas/OrganizationDisputeSettingsUpdate'
            - type: 'null'
        embed_hosts:
          anyOf:
            - items:
                type: string
              type: array
              description: >-
                Hosts allowed to embed this organization's checkout. An entry is
                a host and an optional port, without a scheme: HTTPS is always
                allowed, and HTTP too for local hosts — `localhost`, any
                `.localhost` or `.local` name, and loopback or private
                addresses. `*.example.com` matches any subdomain, but not
                `example.com` itself. An app origin such as
                `chrome-extension://abcdef` carries its scheme, having no host
                to match on.
              examples:
                - - example.com
                  - '*.example.com'
                  - localhost:3000
            - type: 'null'
          title: Embed Hosts
        default_presentment_currency:
          anyOf:
            - $ref: '#/components/schemas/PresentmentCurrency'
            - type: 'null'
          description: Default presentment currency for the organization
        default_tax_behavior:
          anyOf:
            - $ref: '#/components/schemas/TaxBehaviorOption'
            - type: 'null'
          description: Default tax behavior applied on products.
        sso_enforced:
          anyOf:
            - type: boolean
            - type: 'null'
          title: Sso Enforced
          description: >-
            Whether members must access this organization through its SSO
            connection. Turning this on requires an active SSO session for this
            organization and at least one enabled SSO connection.
      type: object
      title: OrganizationUpdate
    Organization:
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
              examples:
                - '2026-01-01T00:00:00.000000Z'
            - type: 'null'
          title: Details Submitted At
          description: When the business details were submitted for review.
        onboarding_resubmission_requested_at:
          anyOf:
            - type: string
              format: date-time
              examples:
                - '2026-01-01T00:00:00.000000Z'
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
    NotPermitted:
      properties:
        error:
          type: string
          const: NotPermitted
          title: Error
          examples:
            - NotPermitted
        detail:
          type: string
          title: Detail
      type: object
      required:
        - error
        - detail
      title: NotPermitted
    DisputeAutoAcceptNotEnabled:
      properties:
        error:
          type: string
          const: DisputeAutoAcceptNotEnabled
          title: Error
          examples:
            - DisputeAutoAcceptNotEnabled
        detail:
          type: string
          title: Detail
      type: object
      required:
        - error
        - detail
      title: DisputeAutoAcceptNotEnabled
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
    SSOEnforcementRequiresConnection:
      properties:
        error:
          type: string
          const: SSOEnforcementRequiresConnection
          title: Error
          examples:
            - SSOEnforcementRequiresConnection
        detail:
          type: string
          title: Detail
      type: object
      required:
        - error
        - detail
      title: SSOEnforcementRequiresConnection
    HTTPValidationError:
      properties:
        detail:
          items:
            $ref: '#/components/schemas/ValidationError'
          type: array
          title: Detail
      type: object
      title: HTTPValidationError
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
    OrganizationDetails:
      properties:
        about:
          anyOf:
            - type: string
            - type: 'null'
          title: About
          description: Brief information about you and your business.
          deprecated: true
        product_description:
          anyOf:
            - type: string
            - type: 'null'
          title: Product Description
          description: Description of digital products being sold.
        selling_categories:
          items:
            type: string
          type: array
          title: Selling Categories
          description: Categories of products being sold.
        pricing_models:
          items:
            type: string
          type: array
          title: Pricing Models
          description: Pricing models used by the organization.
        intended_use:
          anyOf:
            - type: string
            - type: 'null'
          title: Intended Use
          description: How the organization will integrate and use Polar.
          deprecated: true
        customer_acquisition:
          items:
            type: string
          type: array
          title: Customer Acquisition
          description: Main customer acquisition channels.
          deprecated: true
        future_annual_revenue:
          anyOf:
            - type: integer
              minimum: 0
            - type: 'null'
          title: Future Annual Revenue
          description: Estimated revenue in the next 12 months
          deprecated: true
        switching:
          type: boolean
          title: Switching
          description: Switching from another platform?
          default: false
        switching_from:
          anyOf:
            - type: string
              enum:
                - paddle
                - lemon_squeezy
                - gumroad
                - stripe
                - other
            - type: 'null'
          title: Switching From
          description: Which platform the organization is migrating from.
        previous_annual_revenue:
          anyOf:
            - type: integer
              minimum: 0
            - type: 'null'
          title: Previous Annual Revenue
          description: Revenue from last year if applicable.
          deprecated: true
      type: object
      title: OrganizationDetails
    OrganizationFeatureSettingsUpdate:
      properties:
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
      type: object
      title: OrganizationFeatureSettingsUpdate
      description: |-
        Feature settings that organizations can update themselves.

        Other feature settings are managed by Polar staff: they're ignored if
        provided and keep their current value.
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
    OrganizationDisputeSettingsUpdate:
      properties:
        auto_accept_below_amount:
          anyOf:
            - type: integer
              maximum: 10000
              minimum: 1
            - type: 'null'
          title: Auto Accept Below Amount
          description: >-
            Concede disputes below this amount, in USD cents, without asking the
            organization. A dispute charged in another currency converts at the
            rate its payment settled at. `null` turns it off. The disputed
            amount and the processor's dispute fee are still deducted.
      type: object
      title: OrganizationDisputeSettingsUpdate
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
    SubscriptionProrationBehavior:
      type: string
      enum:
        - invoice
        - prorate
        - next_period
        - reset
      title: SubscriptionProrationBehavior
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
    OrganizationFeatureSettings:
      properties:
        issue_funding_enabled:
          type: boolean
          title: Issue Funding Enabled
          description: If this organization has issue funding enabled
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
        frame_ancestors_enforced:
          type: boolean
          title: Frame Ancestors Enforced
          description: >-
            If this organization's checkout tells the browser to refuse framing
            from any host outside its embed hosts.
          default: false
      type: object
      title: OrganizationFeatureSettings
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
