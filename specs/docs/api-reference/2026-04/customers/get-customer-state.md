> ## Documentation Index
> Fetch the complete documentation index at: https://polar.sh/docs/llms.txt
> Use this file to discover all available pages before exploring further.

# Get Customer State

> Get a customer state by ID.

The customer state includes information about
the customer's active subscriptions and benefits.

It's the ideal endpoint to use when you need to get a full overview
of a customer's status.

**Scopes**: `customers:read` `customers:write`



## OpenAPI

````yaml /openapi/2026-04.openapi.json get /v1/customers/{id}/state
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
  /v1/customers/{id}/state:
    get:
      tags:
        - customers
        - public
        - mcp
        - cli
      summary: Get Customer State
      description: |-
        Get a customer state by ID.

        The customer state includes information about
        the customer's active subscriptions and benefits.

        It's the ideal endpoint to use when you need to get a full overview
        of a customer's status.

        **Scopes**: `customers:read` `customers:write`
      operationId: customers:get_state
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
      responses:
        '200':
          description: Successful Response
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/CustomerState'
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
            from polar.v2026_04 import Polar

            polar = Polar("polar_oat_xxx")

            response = polar.customers.get_state(
                '00000000-0000-4000-8000-000000000000',
            )
            print(response)
        - lang: typescript
          source: |
            import { createPolar } from "@polar-sh/sdk/2026-04";

            const polar = createPolar({
              accessToken: "polar_oat_xxx",
            });

            const response = await polar.customers.getState(
              "00000000-0000-4000-8000-000000000000",
            );
            console.log(response);
components:
  schemas:
    CustomerState:
      oneOf:
        - $ref: '#/components/schemas/CustomerStateIndividual'
        - $ref: '#/components/schemas/CustomerStateTeam'
      discriminator:
        propertyName: type
        mapping:
          individual:
            $ref: '#/components/schemas/CustomerStateIndividual'
          team:
            $ref: '#/components/schemas/CustomerStateTeam'
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
    CustomerStateIndividual:
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
          type: string
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
          type: string
          const: individual
          title: Type
          description: The type of customer.
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
        active_subscriptions:
          items:
            $ref: '#/components/schemas/CustomerStateSubscription'
          type: array
          title: Active Subscriptions
          description: The customer's active subscriptions.
        granted_benefits:
          items:
            $ref: '#/components/schemas/CustomerStateBenefitGrant'
          type: array
          title: Granted Benefits
          description: The customer's active benefit grants.
        active_meters:
          items:
            $ref: '#/components/schemas/CustomerStateMeter'
          type: array
          title: Active Meters
          description: The customer's active meters.
      type: object
      required:
        - id
        - created_at
        - modified_at
        - metadata
        - email
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
        - active_subscriptions
        - granted_benefits
        - active_meters
      title: CustomerStateIndividual
      description: |-
        A customer along with additional state information:

        * Active subscriptions
        * Granted benefits
        * Active meters
    CustomerStateTeam:
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
          type: string
          const: team
          title: Type
          description: The type of customer. Team customers can have multiple members.
          examples:
            - team
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
        active_subscriptions:
          items:
            $ref: '#/components/schemas/CustomerStateSubscription'
          type: array
          title: Active Subscriptions
          description: The customer's active subscriptions.
        granted_benefits:
          items:
            $ref: '#/components/schemas/CustomerStateBenefitGrant'
          type: array
          title: Granted Benefits
          description: The customer's active benefit grants.
        active_meters:
          items:
            $ref: '#/components/schemas/CustomerStateMeter'
          type: array
          title: Active Meters
          description: The customer's active meters.
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
        - active_subscriptions
        - granted_benefits
        - active_meters
      title: CustomerStateTeam
      description: |-
        A team customer along with additional state information:

        * Active subscriptions
        * Granted benefits
        * Active meters
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
    MetadataOutputType:
      additionalProperties:
        anyOf:
          - type: string
          - type: integer
          - type: number
          - type: boolean
      type: object
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
    CustomerStateSubscription:
      properties:
        id:
          type: string
          format: uuid4
          title: Id
          description: The ID of the subscription.
          examples:
            - e5149aae-e521-42b9-b24c-abb3d71eea2e
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
        metadata:
          $ref: '#/components/schemas/MetadataOutputType'
        status:
          type: string
          enum:
            - active
            - trialing
          title: Status
          examples:
            - active
            - trialing
        amount:
          type: integer
          title: Amount
          description: The amount of the subscription.
          examples:
            - 1000
        currency:
          type: string
          title: Currency
          description: The currency of the subscription.
          examples:
            - usd
        recurring_interval:
          $ref: '#/components/schemas/RecurringInterval'
          description: The interval at which the subscription recurs.
        current_period_start:
          type: string
          format: date-time
          title: Current Period Start
          description: The start timestamp of the current billing period.
          examples:
            - '2025-02-03T13:37:00Z'
        current_period_end:
          type: string
          format: date-time
          title: Current Period End
          description: The end timestamp of the current billing period.
          examples:
            - '2025-03-03T13:37:00Z'
        trial_start:
          anyOf:
            - type: string
              format: date-time
              examples:
                - '2026-01-01T00:00:00.000000Z'
            - type: 'null'
          title: Trial Start
          description: The start timestamp of the trial period, if any.
          examples:
            - '2025-02-03T13:37:00Z'
        trial_end:
          anyOf:
            - type: string
              format: date-time
              examples:
                - '2026-01-01T00:00:00.000000Z'
            - type: 'null'
          title: Trial End
          description: The end timestamp of the trial period, if any.
          examples:
            - '2025-03-03T13:37:00Z'
        cancel_at_period_end:
          type: boolean
          title: Cancel At Period End
          description: >-
            Whether the subscription will be canceled at the end of the current
            period.
          examples:
            - false
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
          examples:
            - null
        started_at:
          anyOf:
            - type: string
              format: date-time
              examples:
                - '2026-01-01T00:00:00.000000Z'
            - type: 'null'
          title: Started At
          description: The timestamp when the subscription started.
          examples:
            - '2025-01-03T13:37:00Z'
        ends_at:
          anyOf:
            - type: string
              format: date-time
              examples:
                - '2026-01-01T00:00:00.000000Z'
            - type: 'null'
          title: Ends At
          description: The timestamp when the subscription will end.
          examples:
            - null
        product_id:
          type: string
          format: uuid4
          title: Product Id
          description: The ID of the subscribed product.
          examples:
            - d8dd2de1-21b7-4a41-8bc3-ce909c0cfe23
        discount_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Discount Id
          description: The ID of the applied discount, if any.
          examples:
            - null
        seats:
          anyOf:
            - type: integer
            - type: 'null'
          title: Seats
          description: >-
            The number of seats for seat-based subscriptions. None for non-seat
            subscriptions.
          examples:
            - null
        units:
          anyOf:
            - type: integer
            - type: 'null'
          title: Units
          description: >-
            The number of units for unit-based subscriptions. None for non-unit
            subscriptions.
          examples:
            - null
        meters:
          items:
            $ref: '#/components/schemas/CustomerStateSubscriptionMeter'
          type: array
          title: Meters
          description: List of meters associated with the subscription.
      type: object
      required:
        - id
        - created_at
        - modified_at
        - metadata
        - status
        - amount
        - currency
        - recurring_interval
        - current_period_start
        - current_period_end
        - trial_start
        - trial_end
        - cancel_at_period_end
        - canceled_at
        - started_at
        - ends_at
        - product_id
        - discount_id
        - seats
        - units
        - meters
      title: CustomerStateSubscription
      description: An active customer subscription.
    CustomerStateBenefitGrant:
      properties:
        id:
          type: string
          format: uuid4
          title: Id
          description: The ID of the grant.
          examples:
            - d322132c-a9d0-4e0d-b8d3-d81ad021a3a9
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
        granted_at:
          type: string
          format: date-time
          title: Granted At
          description: The timestamp when the benefit was granted.
          examples:
            - '2025-01-03T13:37:00Z'
        benefit_id:
          type: string
          format: uuid4
          title: Benefit Id
          description: The ID of the benefit concerned by this grant.
          examples:
            - 397a17aa-15cf-4cb4-9333-18040203cf98
        benefit_type:
          $ref: '#/components/schemas/BenefitType'
          description: The type of the benefit concerned by this grant.
          examples:
            - custom
        benefit_metadata:
          $ref: '#/components/schemas/MetadataOutputType'
          description: The metadata of the benefit concerned by this grant.
          examples:
            - key: value
        properties:
          anyOf:
            - $ref: '#/components/schemas/BenefitGrantDiscordProperties'
            - $ref: '#/components/schemas/BenefitGrantGitHubRepositoryProperties'
            - $ref: '#/components/schemas/BenefitGrantDownloadablesProperties'
            - $ref: '#/components/schemas/BenefitGrantLicenseKeysProperties'
            - $ref: '#/components/schemas/BenefitGrantCustomProperties'
            - $ref: '#/components/schemas/BenefitGrantFeatureFlagProperties'
            - $ref: '#/components/schemas/BenefitGrantSlackSharedChannelProperties'
          title: Properties
      type: object
      required:
        - id
        - created_at
        - modified_at
        - granted_at
        - benefit_id
        - benefit_type
        - benefit_metadata
        - properties
      title: CustomerStateBenefitGrant
      description: An active benefit grant for a customer.
    CustomerStateMeter:
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
        meter_id:
          type: string
          format: uuid4
          title: Meter Id
          description: The ID of the meter.
          examples:
            - d498a884-e2cd-4d3e-8002-f536468a8b22
        consumed_units:
          type: number
          title: Consumed Units
          description: The number of consumed units.
          examples:
            - 25
        credited_units:
          type: integer
          title: Credited Units
          description: The number of credited units.
          examples:
            - 100
        balance:
          type: number
          title: Balance
          description: >-
            The balance of the meter, i.e. the difference between credited and
            consumed units.
          examples:
            - 75
      type: object
      required:
        - id
        - created_at
        - modified_at
        - meter_id
        - consumed_units
        - credited_units
        - balance
      title: CustomerStateMeter
      description: An active meter for a customer, with latest consumed and credited units.
    RecurringInterval:
      type: string
      enum:
        - day
        - week
        - month
        - year
      title: RecurringInterval
    CustomerStateSubscriptionMeter:
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
        consumed_units:
          type: number
          title: Consumed Units
          description: The number of consumed units so far in this billing period.
          examples:
            - 25
        credited_units:
          type: integer
          title: Credited Units
          description: The number of credited units so far in this billing period.
          examples:
            - 100
        amount:
          type: integer
          title: Amount
          description: The amount due in cents so far in this billing period.
          examples:
            - 0
        meter_id:
          type: string
          format: uuid4
          title: Meter Id
          description: The ID of the meter.
          examples:
            - d498a884-e2cd-4d3e-8002-f536468a8b22
      type: object
      required:
        - created_at
        - modified_at
        - id
        - consumed_units
        - credited_units
        - amount
        - meter_id
      title: CustomerStateSubscriptionMeter
      description: Current consumption and spending for a subscription meter.
    BenefitType:
      type: string
      enum:
        - custom
        - discord
        - github_repository
        - downloadables
        - license_keys
        - meter_credit
        - feature_flag
        - slack_shared_channel
      title: BenefitType
    BenefitGrantDiscordProperties:
      properties:
        account_id:
          anyOf:
            - type: string
            - type: 'null'
          title: Account Id
        guild_id:
          type: string
          title: Guild Id
        role_id:
          type: string
          title: Role Id
        granted_account_id:
          type: string
          title: Granted Account Id
      type: object
      title: BenefitGrantDiscordProperties
    BenefitGrantGitHubRepositoryProperties:
      properties:
        account_id:
          anyOf:
            - type: string
            - type: 'null'
          title: Account Id
        repository_owner:
          type: string
          title: Repository Owner
        repository_name:
          type: string
          title: Repository Name
        permission:
          type: string
          enum:
            - pull
            - triage
            - push
            - maintain
            - admin
          title: Permission
        granted_account_id:
          type: string
          title: Granted Account Id
      type: object
      title: BenefitGrantGitHubRepositoryProperties
    BenefitGrantDownloadablesProperties:
      properties:
        files:
          items:
            type: string
          type: array
          title: Files
      type: object
      title: BenefitGrantDownloadablesProperties
    BenefitGrantLicenseKeysProperties:
      properties:
        user_provided_key:
          type: string
          title: User Provided Key
        license_key_id:
          type: string
          title: License Key Id
        display_key:
          type: string
          title: Display Key
      type: object
      title: BenefitGrantLicenseKeysProperties
    BenefitGrantCustomProperties:
      properties: {}
      type: object
      title: BenefitGrantCustomProperties
    BenefitGrantFeatureFlagProperties:
      properties: {}
      type: object
      title: BenefitGrantFeatureFlagProperties
    BenefitGrantSlackSharedChannelProperties:
      properties:
        invited_email:
          type: string
          title: Invited Email
        channel_id:
          type: string
          title: Channel Id
        channel_name:
          type: string
          title: Channel Name
        invite_id:
          type: string
          title: Invite Id
        invite_url:
          type: string
          title: Invite Url
        connected_team_id:
          type: string
          title: Connected Team Id
      type: object
      title: BenefitGrantSlackSharedChannelProperties
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
