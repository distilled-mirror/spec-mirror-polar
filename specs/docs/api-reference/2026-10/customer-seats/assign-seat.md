> ## Documentation Index
> Fetch the complete documentation index at: https://polar.sh/docs/llms.txt
> Use this file to discover all available pages before exploring further.

# Assign Seat

> **Scopes**: `customer_seats:write`



## OpenAPI

````yaml /openapi/2026-10.openapi.json post /v1/customer-seats
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
  /v1/customer-seats:
    post:
      tags:
        - customer-seats
        - public
        - mcp
        - cli
      summary: Assign Seat
      description: '**Scopes**: `customer_seats:write`'
      operationId: customer-seats:assign_seat
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/SeatAssign'
      responses:
        '200':
          description: Successful Response
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/CustomerSeat'
        '400':
          description: No available seats or customer already has a seat
        '401':
          description: Authentication required
        '403':
          description: Not permitted
        '404':
          description: Subscription, order, or customer not found
        '422':
          description: Validation Error
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/HTTPValidationError'
      security:
        - oidc:
            - customer_seats:write
        - pat:
            - customer_seats:write
        - oat:
            - customer_seats:write
      x-codeSamples:
        - lang: python
          source: |
            from polar.v2026_10 import Polar

            polar = Polar("polar_oat_xxx")

            response = polar.customer_seats.assign_seat(
                immediate_claim=False,
            )
            print(response)
        - lang: typescript
          source: |
            import { createPolar } from "@polar-sh/sdk/2026-10";

            const polar = createPolar({
              accessToken: "polar_oat_xxx",
            });

            const response = await polar.customerSeats.assignSeat(
              {
                "immediate_claim": false
              },
            );
            console.log(response);
components:
  schemas:
    SeatAssign:
      properties:
        subscription_id:
          anyOf:
            - type: string
              format: uuid
            - type: 'null'
          title: Subscription Id
          description: >-
            Subscription ID. Required if neither order_id nor checkout_id is
            provided.
        order_id:
          anyOf:
            - type: string
              format: uuid
            - type: 'null'
          title: Order Id
          description: >-
            Order ID for one-time purchases. Required if subscription_id is not
            provided.
        email:
          anyOf:
            - type: string
              format: email
            - type: 'null'
          title: Email
          description: Email of the customer to assign the seat to
        external_customer_id:
          anyOf:
            - type: string
            - type: 'null'
          title: External Customer Id
          description: External customer ID for the seat assignment
        customer_id:
          anyOf:
            - type: string
              format: uuid
            - type: 'null'
          title: Customer Id
          description: Customer ID for the seat assignment
        external_member_id:
          anyOf:
            - type: string
            - type: 'null'
          title: External Member Id
          description: >-
            External member ID for the seat assignment. Can be used alone
            (lookup existing member) or with email (create/validate member).
        member_id:
          anyOf:
            - type: string
              format: uuid
            - type: 'null'
          title: Member Id
          description: Member ID for the seat assignment.
        metadata:
          anyOf:
            - additionalProperties: true
              type: object
            - type: 'null'
          title: Metadata
          description: Additional metadata for the seat (max 10 keys, 1KB total)
        immediate_claim:
          type: boolean
          title: Immediate Claim
          description: >-
            If true, the seat will be immediately claimed without sending an
            invitation email. API-only feature.
          default: false
      type: object
      title: SeatAssign
    CustomerSeat:
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
          format: uuid
          title: Id
          description: The seat ID
        subscription_id:
          anyOf:
            - type: string
              format: uuid
            - type: 'null'
          title: Subscription Id
          description: The subscription ID (for recurring seats)
        order_id:
          anyOf:
            - type: string
              format: uuid
            - type: 'null'
          title: Order Id
          description: The order ID (for one-time purchase seats)
        status:
          $ref: '#/components/schemas/SeatStatus'
          description: Status of the seat
        customer_id:
          anyOf:
            - type: string
              format: uuid
            - type: 'null'
          title: Customer Id
          description: >-
            The customer ID. When member_model_enabled is true, this is the
            billing customer (purchaser). When false, this is the seat member
            customer.
        member_id:
          anyOf:
            - type: string
              format: uuid
            - type: 'null'
          title: Member Id
          description: The member ID of the seat occupant
        member:
          anyOf:
            - $ref: '#/components/schemas/Member'
            - type: 'null'
          description: The member associated with this seat
        email:
          anyOf:
            - type: string
            - type: 'null'
          title: Email
          description: Email of the seat member (set when member_model_enabled is true)
        customer_email:
          anyOf:
            - type: string
            - type: 'null'
          title: Customer Email
          description: The assigned customer email
        invitation_token_expires_at:
          anyOf:
            - type: string
              format: date-time
              examples:
                - '2026-01-01T00:00:00.000000Z'
            - type: 'null'
          title: Invitation Token Expires At
          description: When the invitation token expires
        claimed_at:
          anyOf:
            - type: string
              format: date-time
              examples:
                - '2026-01-01T00:00:00.000000Z'
            - type: 'null'
          title: Claimed At
          description: When the seat was claimed
        revoked_at:
          anyOf:
            - type: string
              format: date-time
              examples:
                - '2026-01-01T00:00:00.000000Z'
            - type: 'null'
          title: Revoked At
          description: When the seat was revoked
        seat_metadata:
          anyOf:
            - additionalProperties: true
              type: object
            - type: 'null'
          title: Seat Metadata
          description: Additional metadata for the seat
      type: object
      required:
        - created_at
        - modified_at
        - id
        - subscription_id
        - order_id
        - status
        - customer_id
        - member_id
        - member
        - email
        - customer_email
        - invitation_token_expires_at
        - claimed_at
        - revoked_at
        - seat_metadata
      title: CustomerSeat
    HTTPValidationError:
      properties:
        detail:
          items:
            $ref: '#/components/schemas/ValidationError'
          type: array
          title: Detail
      type: object
      title: HTTPValidationError
    SeatStatus:
      type: string
      enum:
        - pending
        - claimed
        - revoked
      title: SeatStatus
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
    MemberRole:
      type: string
      enum:
        - owner
        - billing_manager
        - member
      title: MemberRole
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
