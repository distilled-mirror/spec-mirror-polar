> ## Documentation Index
> Fetch the complete documentation index at: https://polar.sh/docs/llms.txt
> Use this file to discover all available pages before exploring further.

# List Downloadables

> **Scopes**: `customer_portal:read` `customer_portal:write`



## OpenAPI

````yaml /openapi/2026-10.openapi.json get /v1/customer-portal/downloadables/
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
  /v1/customer-portal/downloadables/:
    get:
      tags:
        - customer_portal
        - downloadables
        - public
      summary: List Downloadables
      description: '**Scopes**: `customer_portal:read` `customer_portal:write`'
      operationId: customer_portal:downloadables:list
      parameters:
        - name: benefit_id
          in: query
          required: false
          schema:
            anyOf:
              - type: string
                format: uuid4
                description: The benefit ID.
              - type: array
                items:
                  type: string
                  format: uuid4
                  description: The benefit ID.
              - type: 'null'
            title: BenefitID Filter
            description: Filter by benefit ID.
          description: Filter by benefit ID.
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
      responses:
        '200':
          description: Successful Response
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/ListResource_DownloadableRead_'
        '422':
          description: Validation Error
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/HTTPValidationError'
      security:
        - customer_session:
            - customer_portal:read
            - customer_portal:write
        - member_session:
            - customer_portal:read
            - customer_portal:write
      x-codeSamples:
        - lang: python
          source: |
            from polar.v2026_10 import Polar

            polar = Polar("polar_oat_xxx")

            for item in polar.customer_portal.downloadables.iter_list(
                page=1,
                limit=10,
            ):
                print(item)
        - lang: typescript
          source: >
            import { createPolar } from "@polar-sh/sdk/2026-10";


            const polar = createPolar({
              accessToken: "polar_oat_xxx",
            });


            for await (const item of
            polar.customerPortal.downloadables.iterList(
              {
                "page": 1,
                "limit": 10
              },
            )) {
              console.log(item);
            }
components:
  schemas:
    ListResource_DownloadableRead_:
      properties:
        items:
          items:
            $ref: '#/components/schemas/DownloadableRead'
          type: array
          title: Items
        pagination:
          $ref: '#/components/schemas/Pagination'
      type: object
      required:
        - items
        - pagination
      title: ListResource[DownloadableRead]
    HTTPValidationError:
      properties:
        detail:
          items:
            $ref: '#/components/schemas/ValidationError'
          type: array
          title: Detail
      type: object
      title: HTTPValidationError
    DownloadableRead:
      properties:
        id:
          type: string
          format: uuid4
          title: Id
        benefit_id:
          type: string
          format: uuid4
          title: Benefit Id
        file:
          $ref: '#/components/schemas/FileDownload'
      type: object
      required:
        - id
        - benefit_id
        - file
      title: DownloadableRead
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
    FileDownload:
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
            - type: 'null'
          title: Last Modified At
        download:
          $ref: '#/components/schemas/S3DownloadURL'
        version:
          anyOf:
            - type: string
            - type: 'null'
          title: Version
        is_uploaded:
          type: boolean
          title: Is Uploaded
        service:
          $ref: '#/components/schemas/FileServiceTypes'
        size_readable:
          type: string
          title: Size Readable
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
        - download
        - version
        - is_uploaded
        - service
        - size_readable
      title: FileDownload
    S3DownloadURL:
      properties:
        url:
          type: string
          title: Url
        headers:
          additionalProperties:
            type: string
          type: object
          title: Headers
          default: {}
        expires_at:
          type: string
          format: date-time
          title: Expires At
      type: object
      required:
        - url
        - expires_at
      title: S3DownloadURL
    FileServiceTypes:
      type: string
      enum:
        - downloadable
        - product_media
        - organization_avatar
        - support_case_attachment
      title: FileServiceTypes
  securitySchemes:
    customer_session:
      type: http
      description: >-
        Customer session tokens are specific tokens that are used to
        authenticate customers on your organization. You can create those
        sessions programmatically using the [Create Customer Session
        endpoint](/api-reference/customer-sessions/create-customer-session).
      scheme: bearer
    member_session:
      type: http
      description: >-
        Member session tokens are specific tokens that are used to authenticate
        members on your organization. You can create those sessions
        programmatically using the [Create Member Session
        endpoint](/api-reference/customer-sessions/create-customer-session).
      scheme: bearer

````
