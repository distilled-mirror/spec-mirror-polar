> ## Documentation Index
> Fetch the complete documentation index at: https://polar.sh/docs/llms.txt
> Use this file to discover all available pages before exploring further.

# Get Metrics Limits

> Get the interval limits for the metrics endpoint.

**Scopes**: `metrics:read`



## OpenAPI

````yaml /openapi/2026-10.openapi.json get /v1/metrics/limits
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
  /v1/metrics/limits:
    get:
      tags:
        - metrics
        - public
        - mcp
        - cli
      summary: Get Metrics Limits
      description: |-
        Get the interval limits for the metrics endpoint.

        **Scopes**: `metrics:read`
      operationId: metrics:limits
      responses:
        '200':
          description: Successful Response
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/MetricsLimits'
      security:
        - oidc:
            - metrics:read
        - pat:
            - metrics:read
        - oat:
            - metrics:read
      x-codeSamples:
        - lang: python
          source: |
            from polar.v2026_10 import Polar

            polar = Polar("polar_oat_xxx")

            response = polar.metrics.limits()
            print(response)
        - lang: typescript
          source: |
            import { createPolar } from "@polar-sh/sdk/2026-10";

            const polar = createPolar({
              accessToken: "polar_oat_xxx",
            });

            const response = await polar.metrics.limits();
            console.log(response);
components:
  schemas:
    MetricsLimits:
      properties:
        min_date:
          type: string
          format: date
          title: Min Date
          description: Minimum date to get metrics.
        intervals:
          $ref: '#/components/schemas/MetricsIntervalsLimits'
          description: Limits for each interval.
      type: object
      required:
        - min_date
        - intervals
      title: MetricsLimits
      description: Date limits to get metrics.
    MetricsIntervalsLimits:
      properties:
        hour:
          $ref: '#/components/schemas/MetricsIntervalLimit'
          description: Limits for the hour interval.
        day:
          $ref: '#/components/schemas/MetricsIntervalLimit'
          description: Limits for the day interval.
        week:
          $ref: '#/components/schemas/MetricsIntervalLimit'
          description: Limits for the week interval.
        month:
          $ref: '#/components/schemas/MetricsIntervalLimit'
          description: Limits for the month interval.
        year:
          $ref: '#/components/schemas/MetricsIntervalLimit'
          description: Limits for the year interval.
      type: object
      required:
        - hour
        - day
        - week
        - month
        - year
      title: MetricsIntervalsLimits
      description: Date interval limits to get metrics for each interval.
    MetricsIntervalLimit:
      properties:
        min_days:
          type: integer
          title: Min Days
          description: Minimum number of days for this interval.
        max_days:
          type: integer
          title: Max Days
          description: Maximum number of days for this interval.
      type: object
      required:
        - min_days
        - max_days
      title: MetricsIntervalLimit
      description: Date interval limit to get metrics for a given interval.
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
