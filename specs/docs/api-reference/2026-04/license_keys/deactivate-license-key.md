> ## Documentation Index
> Fetch the complete documentation index at: https://polar.sh/docs/llms.txt
> Use this file to discover all available pages before exploring further.

# Deactivate License Key

> Deactivate a license key instance.

**Scopes**: `license_keys:write`



## OpenAPI

````yaml /openapi/2026-04.openapi.json post /v1/license-keys/deactivate
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
  /v1/license-keys/deactivate:
    post:
      tags:
        - license_keys
        - public
      summary: Deactivate License Key
      description: |-
        Deactivate a license key instance.

        **Scopes**: `license_keys:write`
      operationId: license_keys:deactivate
      requestBody:
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/LicenseKeyDeactivate'
        required: true
      responses:
        '204':
          description: License key activation deactivated.
        '404':
          description: >-
            License key or activation not found, or activation does not belong
            to the license key.
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
            - license_keys:write
        - pat:
            - license_keys:write
        - oat:
            - license_keys:write
      x-codeSamples:
        - lang: python
          source: |
            from polar.v2026_04 import Polar

            polar = Polar("polar_oat_xxx")

            polar.license_keys.deactivate(
                key='string',
                organization_id='00000000-0000-4000-8000-000000000000',
                activation_id='00000000-0000-4000-8000-000000000000',
            )
        - lang: typescript
          source: |
            import { createPolar } from "@polar-sh/sdk/2026-04";

            const polar = createPolar({
              accessToken: "polar_oat_xxx",
            });

            await polar.licenseKeys.deactivate(
              {
                "key": "string",
                "organization_id": "00000000-0000-4000-8000-000000000000",
                "activation_id": "00000000-0000-4000-8000-000000000000"
              },
            );
components:
  schemas:
    LicenseKeyDeactivate:
      properties:
        key:
          type: string
          title: Key
        organization_id:
          type: string
          format: uuid4
          title: Organization Id
        activation_id:
          type: string
          format: uuid4
          title: Activation Id
      type: object
      required:
        - key
        - organization_id
        - activation_id
      title: LicenseKeyDeactivate
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
