> ## Documentation Index
> Fetch the complete documentation index at: https://polar.sh/docs/llms.txt
> Use this file to discover all available pages before exploring further.

# Revoke Token

> Revoke an access token or a refresh token.



## OpenAPI

````yaml /openapi/2026-10.openapi.json post /v1/oauth2/revoke
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
  /v1/oauth2/revoke:
    post:
      tags:
        - oauth2
        - public
      summary: Revoke Token
      description: Revoke an access token or a refresh token.
      operationId: oauth2:revoke_token
      requestBody:
        content:
          application/x-www-form-urlencoded:
            schema:
              $ref: '#/components/schemas/RevokeTokenRequest'
        required: true
      responses:
        '200':
          description: Successful Response
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/RevokeTokenResponse'
      x-codeSamples:
        - lang: python
          source: |
            from polar.v2026_10 import Polar

            polar = Polar("polar_oat_xxx")

            response = polar.oauth2.revoke_token()
            print(response)
        - lang: typescript
          source: |
            import { createPolar } from "@polar-sh/sdk/2026-10";

            const polar = createPolar({
              accessToken: "polar_oat_xxx",
            });

            const response = await polar.oauth2.revokeToken();
            console.log(response);
components:
  schemas:
    RevokeTokenRequest:
      properties:
        token:
          type: string
          title: Token
        token_type_hint:
          anyOf:
            - enum:
                - access_token
                - refresh_token
              type: string
            - type: 'null'
          default: null
          title: Token Type Hint
        client_id:
          type: string
          title: Client Id
        client_secret:
          type: string
          title: Client Secret
      required:
        - token
        - client_id
        - client_secret
      title: RevokeTokenRequest
      type: object
    RevokeTokenResponse:
      properties: {}
      type: object
      title: RevokeTokenResponse

````
