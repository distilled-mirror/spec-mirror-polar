> ## Documentation Index
> Fetch the complete documentation index at: https://polar.sh/docs/llms.txt
> Use this file to discover all available pages before exploring further.

# List Custom Fields

> List custom fields.

**Scopes**: `custom_fields:read` `custom_fields:write`



## OpenAPI

````yaml /openapi/2026-10.openapi.json get /v1/custom-fields/
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
  /v1/custom-fields/:
    get:
      tags:
        - custom-fields
        - public
        - mcp
        - cli
      summary: List Custom Fields
      description: |-
        List custom fields.

        **Scopes**: `custom_fields:read` `custom_fields:write`
      operationId: custom-fields:list
      parameters:
        - name: organization_id
          in: query
          required: false
          schema:
            anyOf:
              - type: string
                format: uuid4
                description: The organization ID.
                examples:
                  - 1dbfc517-0bbf-4301-9ba8-555ca42b9737
              - type: array
                items:
                  type: string
                  format: uuid4
                  description: The organization ID.
                  examples:
                    - 1dbfc517-0bbf-4301-9ba8-555ca42b9737
              - type: 'null'
            title: OrganizationID Filter
            description: Filter by organization ID.
          description: Filter by organization ID.
        - name: query
          in: query
          required: false
          schema:
            anyOf:
              - type: string
              - type: 'null'
            description: Filter by custom field name or slug.
            title: Query
          description: Filter by custom field name or slug.
        - name: type
          in: query
          required: false
          schema:
            anyOf:
              - $ref: '#/components/schemas/CustomFieldType'
              - type: array
                items:
                  $ref: '#/components/schemas/CustomFieldType'
              - type: 'null'
            title: CustomFieldType Filter
            description: Filter by custom field type.
          description: Filter by custom field type.
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
                  $ref: '#/components/schemas/CustomFieldSortProperty'
              - type: 'null'
            description: >-
              Sorting criterion. Several criteria can be used simultaneously and
              will be applied in order. Add a minus sign `-` before the criteria
              name to sort by descending order.
            default:
              - slug
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
                $ref: '#/components/schemas/ListResource_CustomField_'
        '422':
          description: Validation Error
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/HTTPValidationError'
      security:
        - oidc:
            - custom_fields:read
            - custom_fields:write
        - pat:
            - custom_fields:read
            - custom_fields:write
        - oat:
            - custom_fields:read
            - custom_fields:write
      x-codeSamples:
        - lang: python
          source: |
            from polar.v2026_10 import Polar

            polar = Polar("polar_oat_xxx")

            for item in polar.custom_fields.iter_list(
                page=1,
                limit=10,
                sorting=['slug'],
            ):
                print(item)
        - lang: typescript
          source: |
            import { createPolar } from "@polar-sh/sdk/2026-10";

            const polar = createPolar({
              accessToken: "polar_oat_xxx",
            });

            for await (const item of polar.customFields.iterList(
              {
                "page": 1,
                "limit": 10,
                "sorting": [
                  "slug"
                ]
              },
            )) {
              console.log(item);
            }
components:
  schemas:
    CustomFieldType:
      type: string
      enum:
        - text
        - number
        - date
        - checkbox
        - select
      title: CustomFieldType
    CustomFieldSortProperty:
      type: string
      enum:
        - created_at
        - '-created_at'
        - slug
        - '-slug'
        - name
        - '-name'
        - type
        - '-type'
      title: CustomFieldSortProperty
    ListResource_CustomField_:
      properties:
        items:
          items:
            $ref: '#/components/schemas/CustomField'
            title: CustomField
          type: array
          title: Items
        pagination:
          $ref: '#/components/schemas/Pagination'
      type: object
      required:
        - items
        - pagination
      title: ListResource[CustomField]
    HTTPValidationError:
      properties:
        detail:
          items:
            $ref: '#/components/schemas/ValidationError'
          type: array
          title: Detail
      type: object
      title: HTTPValidationError
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
    MetadataOutputType:
      additionalProperties:
        anyOf:
          - type: string
          - type: integer
          - type: number
          - type: boolean
      type: object
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
