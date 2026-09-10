> ## Documentation Index
> Fetch the complete documentation index at: https://polar.sh/docs/llms.txt
> Use this file to discover all available pages before exploring further.

# Update Meter

> Update a meter.

**Scopes**: `meters:write`



## OpenAPI

````yaml /openapi/2026-10.openapi.json patch /v1/meters/{id}
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
  /v1/meters/{id}:
    patch:
      tags:
        - meters
        - public
      summary: Update Meter
      description: |-
        Update a meter.

        **Scopes**: `meters:write`
      operationId: meters:update
      parameters:
        - name: id
          in: path
          required: true
          schema:
            type: string
            format: uuid4
            description: The meter ID.
            title: Id
          description: The meter ID.
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/MeterUpdate'
      responses:
        '200':
          description: Meter updated.
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Meter'
        '404':
          description: Meter not found.
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
            - meters:write
        - pat:
            - meters:write
        - oat:
            - meters:write
      x-codeSamples:
        - lang: python
          source: |
            from polar.v2026_10 import Polar

            polar = Polar("polar_oat_xxx")

            response = polar.meters.update(
                '00000000-0000-4000-8000-000000000000',
            )
            print(response)
        - lang: typescript
          source: |
            import { createPolar } from "@polar-sh/sdk/2026-10";

            const polar = createPolar({
              accessToken: "polar_oat_xxx",
            });

            const response = await polar.meters.update(
              "00000000-0000-4000-8000-000000000000",
              {},
            );
            console.log(response);
components:
  schemas:
    MeterUpdate:
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
          anyOf:
            - type: string
              minLength: 3
            - type: 'null'
          title: Name
          description: >-
            The name of the meter. Will be shown on customer's invoices and
            usage.
        unit:
          anyOf:
            - $ref: '#/components/schemas/MeterUnit'
            - type: 'null'
          description: The unit of the meter.
        custom_label:
          anyOf:
            - type: string
            - type: 'null'
          title: Custom Label
          description: The label for the custom unit. Required when unit is 'custom'.
        custom_multiplier:
          anyOf:
            - type: integer
              exclusiveMinimum: 0
            - type: 'null'
          title: Custom Multiplier
          description: >-
            The multiplier to convert from base unit to display scale. Required
            when unit is 'custom'.
        filter:
          anyOf:
            - $ref: '#/components/schemas/Filter'
              description: >-
                The filter to apply on events that'll be used to calculate the
                meter.
            - type: 'null'
          description: >-
            The filter to apply on events that'll be used to calculate the
            meter.
        aggregation:
          anyOf:
            - oneOf:
                - $ref: '#/components/schemas/CountAggregation'
                - $ref: '#/components/schemas/PropertyAggregation'
                - $ref: '#/components/schemas/UniqueAggregation'
              discriminator:
                propertyName: func
                mapping:
                  avg:
                    $ref: '#/components/schemas/PropertyAggregation'
                  count:
                    $ref: '#/components/schemas/CountAggregation'
                  max:
                    $ref: '#/components/schemas/PropertyAggregation'
                  min:
                    $ref: '#/components/schemas/PropertyAggregation'
                  sum:
                    $ref: '#/components/schemas/PropertyAggregation'
                  unique:
                    $ref: '#/components/schemas/UniqueAggregation'
            - type: 'null'
          title: Aggregation
          description: >-
            The aggregation to apply on the filtered events to calculate the
            meter.
        is_archived:
          anyOf:
            - type: boolean
            - type: 'null'
          title: Is Archived
          description: >-
            Whether the meter is archived. Archived meters are no longer used
            for billing.
      type: object
      title: MeterUpdate
    Meter:
      properties:
        metadata:
          $ref: '#/components/schemas/MetadataOutputType'
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
          description: >-
            The name of the meter. Will be shown on customer's invoices and
            usage.
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
        filter:
          $ref: '#/components/schemas/Filter'
          description: >-
            The filter to apply on events that'll be used to calculate the
            meter.
        aggregation:
          oneOf:
            - $ref: '#/components/schemas/CountAggregation'
            - $ref: '#/components/schemas/PropertyAggregation'
            - $ref: '#/components/schemas/UniqueAggregation'
          title: Aggregation
          description: >-
            The aggregation to apply on the filtered events to calculate the
            meter.
          discriminator:
            propertyName: func
            mapping:
              avg:
                $ref: '#/components/schemas/PropertyAggregation'
              count:
                $ref: '#/components/schemas/CountAggregation'
              max:
                $ref: '#/components/schemas/PropertyAggregation'
              min:
                $ref: '#/components/schemas/PropertyAggregation'
              sum:
                $ref: '#/components/schemas/PropertyAggregation'
              unique:
                $ref: '#/components/schemas/UniqueAggregation'
        organization_id:
          type: string
          format: uuid4
          title: Organization Id
          description: The ID of the organization owning the meter.
        archived_at:
          anyOf:
            - type: string
              format: date-time
            - type: 'null'
          title: Archived At
          description: Whether the meter is archived and the time it was archived.
      type: object
      required:
        - metadata
        - created_at
        - modified_at
        - id
        - name
        - unit
        - filter
        - aggregation
        - organization_id
      title: Meter
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
    MeterUnit:
      type: string
      enum:
        - scalar
        - token
        - custom
      title: MeterUnit
    Filter:
      properties:
        conjunction:
          $ref: '#/components/schemas/FilterConjunction'
        clauses:
          items:
            anyOf:
              - $ref: '#/components/schemas/FilterClause'
              - $ref: '#/components/schemas/Filter'
          type: array
          title: Clauses
      type: object
      required:
        - conjunction
        - clauses
      title: Filter
    CountAggregation:
      properties:
        func:
          type: string
          const: count
          title: Func
          default: count
      type: object
      title: CountAggregation
    PropertyAggregation:
      properties:
        func:
          type: string
          enum:
            - sum
            - max
            - min
            - avg
          title: Func
        property:
          type: string
          title: Property
      type: object
      required:
        - func
        - property
      title: PropertyAggregation
    UniqueAggregation:
      properties:
        func:
          type: string
          const: unique
          title: Func
          default: unique
        property:
          type: string
          title: Property
      type: object
      required:
        - property
      title: UniqueAggregation
    MetadataOutputType:
      additionalProperties:
        anyOf:
          - type: string
          - type: integer
          - type: number
          - type: boolean
      type: object
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
    FilterConjunction:
      type: string
      enum:
        - and
        - or
      title: FilterConjunction
    FilterClause:
      properties:
        property:
          type: string
          title: Property
        operator:
          $ref: '#/components/schemas/FilterOperator'
        value:
          anyOf:
            - type: string
              maxLength: 1000
            - type: integer
              maximum: 2147483647
              minimum: -2147483648
            - type: boolean
          title: Value
      type: object
      required:
        - property
        - operator
        - value
      title: FilterClause
    FilterOperator:
      type: string
      enum:
        - eq
        - ne
        - gt
        - gte
        - lt
        - lte
        - like
        - not_like
      title: FilterOperator
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
