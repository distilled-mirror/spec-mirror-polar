> ## Documentation Index
> Fetch the complete documentation index at: https://polar.sh/docs/llms.txt
> Use this file to discover all available pages before exploring further.

# Get User Info

> Get information about the authenticated user.



## OpenAPI

````yaml /openapi/2026-10.openapi.json get /v1/oauth2/userinfo
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
  /v1/oauth2/userinfo:
    get:
      tags:
        - oauth2
        - public
      summary: Get User Info
      description: Get information about the authenticated user.
      operationId: oauth2:userinfo
      responses:
        '200':
          description: Successful Response
          content:
            application/json:
              schema:
                anyOf:
                  - $ref: '#/components/schemas/UserInfoUser'
                  - $ref: '#/components/schemas/UserInfoOrganization'
                title: Response Oauth2:Userinfo
      security:
        - oidc: []
      x-codeSamples:
        - lang: python
          source: |
            from polar.v2026_10 import Polar

            polar = Polar("polar_oat_xxx")

            response = polar.oauth2.userinfo()
            print(response)
        - lang: typescript
          source: |
            import { createPolar } from "@polar-sh/sdk/2026-10";

            const polar = createPolar({
              accessToken: "polar_oat_xxx",
            });

            const response = await polar.oauth2.userinfo();
            console.log(response);
components:
  schemas:
    UserInfoUser:
      properties:
        sub:
          type: string
          title: Sub
        name:
          anyOf:
            - type: string
            - type: 'null'
          title: Name
        email:
          anyOf:
            - type: string
            - type: 'null'
          title: Email
        email_verified:
          anyOf:
            - type: boolean
            - type: 'null'
          title: Email Verified
      type: object
      required:
        - sub
      title: UserInfoUser
    UserInfoOrganization:
      properties:
        sub:
          type: string
          title: Sub
        name:
          anyOf:
            - type: string
            - type: 'null'
          title: Name
      type: object
      required:
        - sub
      title: UserInfoOrganization
  securitySchemes:
    oidc:
      type: openIdConnect
      openIdConnectUrl: /.well-known/openid-configuration

````
