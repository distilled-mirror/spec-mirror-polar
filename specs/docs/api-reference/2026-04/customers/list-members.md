> ## Documentation Index
> Fetch the complete documentation index at: https://polar.sh/docs/llms.txt
> Use this file to discover all available pages before exploring further.

# List Members

> List the members of a customer.

**Scopes**: `members:read` `members:write`



## OpenAPI

````yaml /openapi/2026-04.openapi.json get /v1/customers/{id}/members
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
  /v1/customers/{id}/members:
    get:
      tags:
        - customers
        - members
        - public
      summary: List Members
      description: |-
        List the members of a customer.

        **Scopes**: `members:read` `members:write`
      operationId: customers:members:list
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
        - name: role
          in: query
          required: false
          schema:
            anyOf:
              - $ref: '#/components/schemas/MemberRole'
              - type: 'null'
            description: Filter by member role.
            title: Role
          description: Filter by member role.
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
                  $ref: '#/components/schemas/MemberSortProperty'
              - type: 'null'
            description: >-
              Sorting criterion. Several criteria can be used simultaneously and
              will be applied in order. Add a minus sign `-` before the criteria
              name to sort by descending order.
            default:
              - '-created_at'
            title: Sorting
          description: >-
            Sorting criterion. Several criteria can be used simultaneously and
            will be applied in order. Add a minus sign `-` before the criteria
            name to sort by descending order.
      responses:
        '200':
          description: Members retrieved.
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/ListResource_Member_'
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
            - members:read
            - members:write
        - pat:
            - members:read
            - members:write
        - oat:
            - members:read
            - members:write
      x-codeSamples:
        - lang: python
          source: |
            from polar.v2026_04 import Polar

            polar = Polar("polar_oat_xxx")

            for item in polar.customers.members.iter_list(
                '00000000-0000-4000-8000-000000000000',
                page=1,
                limit=10,
                sorting=['-created_at'],
            ):
                print(item)
        - lang: typescript
          source: |
            import { createPolar } from "@polar-sh/sdk/2026-04";

            const polar = createPolar({
              accessToken: "polar_oat_xxx",
            });

            for await (const item of polar.customers.members.iterList(
              "00000000-0000-4000-8000-000000000000",
              {
                "page": 1,
                "limit": 10,
                "sorting": [
                  "-created_at"
                ]
              },
            )) {
              console.log(item);
            }
components:
  schemas:
    MemberRole:
      type: string
      enum:
        - owner
        - billing_manager
        - member
      title: MemberRole
    MemberSortProperty:
      type: string
      enum:
        - created_at
        - '-created_at'
      title: MemberSortProperty
    ListResource_Member_:
      properties:
        items:
          items:
            $ref: '#/components/schemas/Member'
          type: array
          title: Items
        pagination:
          $ref: '#/components/schemas/Pagination'
      type: object
      required:
        - items
        - pagination
      title: ListResource[Member]
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
    Member:
      properties:
        id:
          type: string
          format: uuid4
          title: Id
          description: The ID of the member.
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
        customer_id:
          type: string
          format: uuid4
          title: Customer Id
          description: The ID of the customer this member belongs to.
        email:
          type: string
          title: Email
          description: The email address of the member.
          examples:
            - member@example.com
        name:
          anyOf:
            - type: string
            - type: 'null'
          title: Name
          description: The name of the member.
          examples:
            - Jane Doe
        external_id:
          anyOf:
            - type: string
            - type: 'null'
          title: External Id
          description: >-
            The ID of the member in your system. This must be unique within the
            customer. 
          examples:
            - usr_1337
        role:
          $ref: '#/components/schemas/MemberRole'
          description: The role of the member within the customer.
          examples:
            - owner
      type: object
      required:
        - id
        - created_at
        - modified_at
        - customer_id
        - email
        - name
        - external_id
        - role
      title: Member
      description: A member of a customer.
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
