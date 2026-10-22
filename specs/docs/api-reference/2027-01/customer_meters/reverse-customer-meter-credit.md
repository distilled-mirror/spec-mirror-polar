> ## Documentation Index
> Fetch the complete documentation index at: https://polar.sh/docs/llms.txt
> Use this file to discover all available pages before exploring further.

# Reverse Customer Meter Credit

> Reverse a manual credit, removing the units the customer hasn't used yet.

Works once per credit, within 7 days and in the current billing period.
Returns a `meter.credited` event with negative units and
`parent_id` set to the reversed credit.

**Scopes**: `customers:write`



## OpenAPI

````yaml /openapi/2027-01.openapi.json post /v1/customer-meters/credits/{id}/reverse
openapi: 3.1.0
info:
  title: Polar API
  summary: Polar HTTP and Webhooks API
  description: Read the docs at https://polar.sh/docs/api-reference
  version: 2027-01
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
  /v1/customer-meters/credits/{id}/reverse:
    post:
      tags:
        - customer_meters
        - public
        - mcp
        - cli
      summary: Reverse Customer Meter Credit
      description: >-
        Reverse a manual credit, removing the units the customer hasn't used
        yet.


        Works once per credit, within 7 days and in the current billing period.

        Returns a `meter.credited` event with negative units and

        `parent_id` set to the reversed credit.


        **Scopes**: `customers:write`
      operationId: customer_meters:reverse_credit
      parameters:
        - name: id
          in: path
          required: true
          schema:
            type: string
            format: uuid4
            description: >-
              The ID of the `meter.credited` event returned when the credit was
              granted.
            title: Id
          description: >-
            The ID of the `meter.credited` event returned when the credit was
            granted.
      responses:
        '201':
          description: Credit reversed.
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/MeterCreditEvent'
        '403':
          description: You don't have permission to manage customers.
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/NotPermitted'
        '404':
          description: Manual credit not found.
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/ResourceNotFound'
        '409':
          description: >-
            The credit can't be reversed, or the customer meter is being
            updated: retry shortly.
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/CustomerMeterCreditNotReversible'
        '422':
          description: Validation Error
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/HTTPValidationError'
      security:
        - oidc:
            - customers:write
        - pat:
            - customers:write
        - oat:
            - customers:write
      x-codeSamples:
        - lang: python
          source: |
            from polar.v2027_01 import Polar

            polar = Polar("polar_oat_xxx")

            response = polar.customer_meters.reverse_credit(
                '00000000-0000-4000-8000-000000000000',
            )
            print(response)
        - lang: typescript
          source: |
            import { createPolar } from "@polar-sh/sdk/2027-01";

            const polar = createPolar({
              accessToken: "polar_oat_xxx",
            });

            const response = await polar.customerMeters.reverseCredit(
              "00000000-0000-4000-8000-000000000000",
            );
            console.log(response);
components:
  schemas:
    MeterCreditEvent:
      properties:
        id:
          type: string
          format: uuid4
          title: Id
          description: The ID of the object.
        timestamp:
          type: string
          format: date-time
          title: Timestamp
          description: The timestamp of the event.
          examples:
            - '2026-01-01T00:00:00.000000Z'
        organization_id:
          type: string
          format: uuid4
          title: Organization Id
          description: The ID of the organization owning the event.
          examples:
            - 1dbfc517-0bbf-4301-9ba8-555ca42b9737
        customer_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Customer Id
          description: >-
            ID of the customer in your Polar organization associated with the
            event.
        customer:
          anyOf:
            - $ref: '#/components/schemas/Customer'
            - type: 'null'
          description: The customer associated with the event.
        external_customer_id:
          anyOf:
            - type: string
            - type: 'null'
          title: External Customer Id
          description: ID of the customer in your system associated with the event.
        member_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Member Id
          description: >-
            ID of the member within the customer's organization who performed
            the action inside B2B.
        external_member_id:
          anyOf:
            - type: string
            - type: 'null'
          title: External Member Id
          description: >-
            ID of the member in your system within the customer's organization
            who performed the action inside B2B.
        child_count:
          type: integer
          title: Child Count
          description: Number of direct child events linked to this event.
          default: 0
        parent_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Parent Id
          description: The ID of the parent event.
        label:
          type: string
          title: Label
          description: Human readable label of the event type.
        source:
          type: string
          const: system
          title: Source
          description: >-
            The source of the event. `system` events are created by Polar.
            `user` events are the one you create through our ingestion API.
        name:
          type: string
          const: meter.credited
          title: Name
          description: The name of the event.
        metadata:
          $ref: '#/components/schemas/MeterCreditedMetadata'
      type: object
      required:
        - id
        - timestamp
        - organization_id
        - customer_id
        - customer
        - external_customer_id
        - label
        - source
        - name
        - metadata
      title: MeterCreditEvent
      description: An event created by Polar when credits are added to a customer meter.
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
    CustomerMeterCreditNotReversible:
      properties:
        error:
          type: string
          const: CustomerMeterCreditNotReversible
          title: Error
          examples:
            - CustomerMeterCreditNotReversible
        detail:
          type: string
          title: Detail
      type: object
      required:
        - error
        - detail
      title: CustomerMeterCreditNotReversible
    HTTPValidationError:
      properties:
        detail:
          items:
            $ref: '#/components/schemas/ValidationError'
          type: array
          title: Detail
      type: object
      title: HTTPValidationError
    Customer:
      oneOf:
        - $ref: '#/components/schemas/CustomerIndividual'
        - $ref: '#/components/schemas/CustomerTeam'
      discriminator:
        propertyName: type
        mapping:
          individual: '#/components/schemas/CustomerIndividual'
          team: '#/components/schemas/CustomerTeam'
    MeterCreditedMetadata:
      properties:
        meter_id:
          type: string
          title: Meter Id
        units:
          type: integer
          title: Units
        rollover:
          type: boolean
          title: Rollover
        reason:
          type: string
          enum:
            - benefit
            - manual
          title: Reason
          default: benefit
      type: object
      required:
        - meter_id
        - units
        - rollover
      title: MeterCreditedMetadata
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
    CustomerIndividual:
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
      title: CustomerIndividual
      description: A customer in an organization.
    CustomerTeam:
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
      title: CustomerTeam
      description: A team customer in an organization.
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

This documentation is built and hosted on [Mintlify](https://mintlify.com), a developer documentation platform.
