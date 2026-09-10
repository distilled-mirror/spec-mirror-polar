> ## Documentation Index
> Fetch the complete documentation index at: https://polar.sh/docs/llms.txt
> Use this file to discover all available pages before exploring further.

# Introspect Token

> Get information about an access token.



## OpenAPI

````yaml /openapi/2026-10.openapi.json post /v1/oauth2/introspect
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
  /v1/oauth2/introspect:
    post:
      tags:
        - oauth2
        - public
      summary: Introspect Token
      description: Get information about an access token.
      operationId: oauth2:introspect_token
      requestBody:
        content:
          application/x-www-form-urlencoded:
            schema:
              $ref: '#/components/schemas/IntrospectTokenRequest'
        required: true
      responses:
        '200':
          description: Successful Response
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/IntrospectTokenResponse'
      x-codeSamples:
        - lang: python
          source: |
            from polar.v2026_10 import Polar

            polar = Polar("polar_oat_xxx")

            response = polar.oauth2.introspect_token()
            print(response)
        - lang: typescript
          source: |
            import { createPolar } from "@polar-sh/sdk/2026-10";

            const polar = createPolar({
              accessToken: "polar_oat_xxx",
            });

            const response = await polar.oauth2.introspectToken();
            console.log(response);
components:
  schemas:
    IntrospectTokenRequest:
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
      title: IntrospectTokenRequest
      type: object
    IntrospectTokenResponse:
      properties:
        active:
          type: boolean
          title: Active
        client_id:
          type: string
          title: Client Id
        token_type:
          type: string
          enum:
            - access_token
            - refresh_token
          title: Token Type
        scope:
          type: string
          title: Scope
        sub_type:
          $ref: '#/components/schemas/SubType'
        sub:
          type: string
          title: Sub
        organizations:
          items:
            type: string
          type: array
          title: Organizations
        aud:
          type: string
          title: Aud
        iss:
          type: string
          title: Iss
        exp:
          type: integer
          title: Exp
        iat:
          type: integer
          title: Iat
      type: object
      required:
        - active
        - client_id
        - token_type
        - scope
        - sub_type
        - sub
        - organizations
        - aud
        - iss
        - exp
        - iat
      title: IntrospectTokenResponse
    SubType:
      type: string
      enum:
        - user
        - organization
      title: SubType

````
