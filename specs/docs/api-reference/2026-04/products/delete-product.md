> ## Documentation Index
> Fetch the complete documentation index at: https://polar.sh/docs/llms.txt
> Use this file to discover all available pages before exploring further.

# Delete Product

> Delete a product.

Only products without orders, subscriptions, trials or discounts can be deleted.
Products that are in use can only be archived.

**Scopes**: `products:write`



## OpenAPI

````yaml /openapi/2026-04.openapi.json delete /v1/products/{id}
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
  /v1/products/{id}:
    delete:
      tags:
        - products
        - public
        - mcp
        - cli
      summary: Delete Product
      description: >-
        Delete a product.


        Only products without orders, subscriptions, trials or discounts can be
        deleted.

        Products that are in use can only be archived.


        **Scopes**: `products:write`
      operationId: products:delete
      parameters:
        - name: id
          in: path
          required: true
          schema:
            type: string
            format: uuid4
            description: The product ID.
            title: Id
      responses:
        '204':
          description: Product deleted.
        '403':
          description: You don't have the permission to delete this product.
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/NotPermitted'
        '404':
          description: Product not found.
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/ResourceNotFound'
        '409':
          description: Product is in use and cannot be deleted.
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/ProductNotDeletable'
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
            from polar.v2026_04 import Polar

            polar = Polar("polar_oat_xxx")

            polar.products.delete(
                '00000000-0000-4000-8000-000000000000',
            )
        - lang: typescript
          source: |
            import { createPolar } from "@polar-sh/sdk/2026-04";

            const polar = createPolar({
              accessToken: "polar_oat_xxx",
            });

            await polar.products.delete(
              "00000000-0000-4000-8000-000000000000",
            );
components:
  schemas:
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
    ProductNotDeletable:
      properties:
        error:
          type: string
          const: ProductNotDeletable
          title: Error
          examples:
            - ProductNotDeletable
        detail:
          type: string
          title: Detail
      type: object
      required:
        - error
        - detail
      title: ProductNotDeletable
    HTTPValidationError:
      properties:
        detail:
          items:
            $ref: '#/components/schemas/ValidationError'
          type: array
          title: Detail
      type: object
      title: HTTPValidationError
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
