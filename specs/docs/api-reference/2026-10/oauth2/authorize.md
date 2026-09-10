> ## Documentation Index
> Fetch the complete documentation index at: https://polar.sh/docs/llms.txt
> Use this file to discover all available pages before exploring further.

# Authorize



## OpenAPI

````yaml /openapi/2026-10.openapi.json get /v1/oauth2/authorize
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
  /v1/oauth2/authorize:
    get:
      tags:
        - oauth2
        - public
      summary: Authorize
      operationId: oauth2:authorize
      responses:
        '200':
          description: Successful Response
          content:
            application/json:
              schema:
                oneOf:
                  - $ref: '#/components/schemas/AuthorizeResponseUser'
                  - $ref: '#/components/schemas/AuthorizeResponseOrganization'
                title: Response Oauth2:Authorize
                discriminator:
                  propertyName: sub_type
                  mapping:
                    user:
                      $ref: '#/components/schemas/AuthorizeResponseUser'
                    organization:
                      $ref: '#/components/schemas/AuthorizeResponseOrganization'
      security:
        - oidc: []
        - pat: []
      x-codeSamples:
        - lang: python
          source: |
            from polar.v2026_10 import Polar

            polar = Polar("polar_oat_xxx")

            response = polar.oauth2.authorize()
            print(response)
        - lang: typescript
          source: |
            import { createPolar } from "@polar-sh/sdk/2026-10";

            const polar = createPolar({
              accessToken: "polar_oat_xxx",
            });

            const response = await polar.oauth2.authorize();
            console.log(response);
components:
  schemas:
    AuthorizeResponseUser:
      properties:
        client:
          $ref: '#/components/schemas/OAuth2ClientPublic'
        sub_type:
          type: string
          const: user
          title: Sub Type
        sub:
          anyOf:
            - $ref: '#/components/schemas/AuthorizeUser'
            - type: 'null'
        scopes:
          items:
            $ref: '#/components/schemas/Scope'
          type: array
          title: Scopes
        organizations:
          items:
            $ref: '#/components/schemas/AuthorizeOrganization'
          type: array
          title: Organizations
        requires_single_organization:
          type: boolean
          title: Requires Single Organization
          default: false
        scope_display_names:
          additionalProperties:
            type: string
          type: object
          title: Scope Display Names
          default:
            openid: OpenID
            profile: Read your profile
            email: Read your email address
            user:read: Read your user account
            user:write: Manage your user account
            organizations:read: Read your organizations
            organizations:write: Create or modify organizations
            custom_fields:read: Read custom fields
            custom_fields:write: Create or modify custom fields
            discounts:read: Read discounts
            discounts:write: Create or modify discounts
            checkout_links:read: Read checkout links
            checkout_links:write: Create or modify checkout links
            checkouts:read: Read checkout sessions
            checkouts:write: Create or modify checkout sessions
            transactions:read: Read transactions
            transactions:write: Create or modify transactions
            payouts:read: Read payouts
            payouts:write: Create or modify payouts
            products:read: Read products
            products:write: Create or modify products
            benefits:read: Read benefits
            benefits:write: Create or modify benefits
            events:read: Read events
            events:write: Create events
            meters:read: Read meters
            meters:write: Create or modify meters
            files:read: Read file uploads
            files:write: Create or modify file uploads
            subscriptions:read: Read subscriptions made on your organizations
            subscriptions:write: Create or modify subscriptions made on your organizations
            customers:read: Read customers
            customers:write: Create or modify customers
            members:read: Read members
            members:write: Create or modify members
            wallets:read: Read wallets
            wallets:write: Create or modify wallets
            disputes:read: Read disputes
            disputes:write: Create or modify disputes
            customer_meters:read: Read customer meters
            customer_sessions:write: Create or modify customer sessions
            member_sessions:write: Create or modify member sessions
            customer_seats:read: Read customer seats
            customer_seats:write: Create or modify customer seats
            orders:read: Read orders made on your organizations
            orders:write: Modify orders made on your organizations
            refunds:read: Read refunds made on your organizations
            refunds:write: Create or modify refunds
            payments:read: Read payments made on your organizations
            metrics:read: Read metrics
            metrics:write: Create or modify metric definitions
            webhooks:read: Read webhooks
            webhooks:write: Create or modify webhooks
            license_keys:read: Read license keys
            license_keys:write: Modify license keys
            customer_portal:read: Read your orders, subscriptions and benefits
            customer_portal:write: Create or modify your orders, subscriptions and benefits
            notifications:read: Read notifications
            notifications:write: Mark notifications as read
            notification_recipients:read: Read notification recipients
            notification_recipients:write: Create or modify notification recipients
            organization_access_tokens:read: Read organization access tokens
            organization_access_tokens:write: Create or modify organization access tokens
      type: object
      required:
        - client
        - sub_type
        - sub
        - scopes
        - organizations
      title: AuthorizeResponseUser
    AuthorizeResponseOrganization:
      properties:
        client:
          $ref: '#/components/schemas/OAuth2ClientPublic'
        sub_type:
          type: string
          const: organization
          title: Sub Type
        sub:
          anyOf:
            - $ref: '#/components/schemas/AuthorizeOrganization'
            - type: 'null'
        scopes:
          items:
            $ref: '#/components/schemas/Scope'
          type: array
          title: Scopes
        organizations:
          items:
            $ref: '#/components/schemas/AuthorizeOrganization'
          type: array
          title: Organizations
        requires_single_organization:
          type: boolean
          title: Requires Single Organization
          default: false
        scope_display_names:
          additionalProperties:
            type: string
          type: object
          title: Scope Display Names
          default:
            openid: OpenID
            profile: Read your profile
            email: Read your email address
            user:read: Read your user account
            user:write: Manage your user account
            organizations:read: Read your organizations
            organizations:write: Create or modify organizations
            custom_fields:read: Read custom fields
            custom_fields:write: Create or modify custom fields
            discounts:read: Read discounts
            discounts:write: Create or modify discounts
            checkout_links:read: Read checkout links
            checkout_links:write: Create or modify checkout links
            checkouts:read: Read checkout sessions
            checkouts:write: Create or modify checkout sessions
            transactions:read: Read transactions
            transactions:write: Create or modify transactions
            payouts:read: Read payouts
            payouts:write: Create or modify payouts
            products:read: Read products
            products:write: Create or modify products
            benefits:read: Read benefits
            benefits:write: Create or modify benefits
            events:read: Read events
            events:write: Create events
            meters:read: Read meters
            meters:write: Create or modify meters
            files:read: Read file uploads
            files:write: Create or modify file uploads
            subscriptions:read: Read subscriptions made on your organizations
            subscriptions:write: Create or modify subscriptions made on your organizations
            customers:read: Read customers
            customers:write: Create or modify customers
            members:read: Read members
            members:write: Create or modify members
            wallets:read: Read wallets
            wallets:write: Create or modify wallets
            disputes:read: Read disputes
            disputes:write: Create or modify disputes
            customer_meters:read: Read customer meters
            customer_sessions:write: Create or modify customer sessions
            member_sessions:write: Create or modify member sessions
            customer_seats:read: Read customer seats
            customer_seats:write: Create or modify customer seats
            orders:read: Read orders made on your organizations
            orders:write: Modify orders made on your organizations
            refunds:read: Read refunds made on your organizations
            refunds:write: Create or modify refunds
            payments:read: Read payments made on your organizations
            metrics:read: Read metrics
            metrics:write: Create or modify metric definitions
            webhooks:read: Read webhooks
            webhooks:write: Create or modify webhooks
            license_keys:read: Read license keys
            license_keys:write: Modify license keys
            customer_portal:read: Read your orders, subscriptions and benefits
            customer_portal:write: Create or modify your orders, subscriptions and benefits
            notifications:read: Read notifications
            notifications:write: Mark notifications as read
            notification_recipients:read: Read notification recipients
            notification_recipients:write: Create or modify notification recipients
            organization_access_tokens:read: Read organization access tokens
            organization_access_tokens:write: Create or modify organization access tokens
      type: object
      required:
        - client
        - sub_type
        - sub
        - scopes
        - organizations
      title: AuthorizeResponseOrganization
    OAuth2ClientPublic:
      properties:
        created_at:
          type: string
          format: date-time
          title: Created At
          description: Creation timestamp of the object.
        modified_at:
          anyOf:
            - type: string
              format: date-time
            - type: 'null'
          title: Modified At
          description: Last modification timestamp of the object.
        client_id:
          type: string
          title: Client Id
        client_name:
          anyOf:
            - type: string
            - type: 'null'
          title: Client Name
        client_uri:
          anyOf:
            - type: string
            - type: 'null'
          title: Client Uri
        logo_uri:
          anyOf:
            - type: string
            - type: 'null'
          title: Logo Uri
        tos_uri:
          anyOf:
            - type: string
            - type: 'null'
          title: Tos Uri
        policy_uri:
          anyOf:
            - type: string
            - type: 'null'
          title: Policy Uri
      type: object
      required:
        - created_at
        - modified_at
        - client_id
        - client_name
        - client_uri
        - logo_uri
        - tos_uri
        - policy_uri
      title: OAuth2ClientPublic
    AuthorizeUser:
      properties:
        id:
          type: string
          format: uuid4
          title: Id
        email:
          type: string
          format: email
          title: Email
        avatar_url:
          anyOf:
            - type: string
            - type: 'null'
          title: Avatar Url
      type: object
      required:
        - id
        - email
        - avatar_url
      title: AuthorizeUser
    Scope:
      type: string
      enum:
        - openid
        - profile
        - email
        - user:read
        - user:write
        - organizations:read
        - organizations:write
        - custom_fields:read
        - custom_fields:write
        - discounts:read
        - discounts:write
        - checkout_links:read
        - checkout_links:write
        - checkouts:read
        - checkouts:write
        - transactions:read
        - transactions:write
        - payouts:read
        - payouts:write
        - products:read
        - products:write
        - benefits:read
        - benefits:write
        - events:read
        - events:write
        - meters:read
        - meters:write
        - files:read
        - files:write
        - subscriptions:read
        - subscriptions:write
        - customers:read
        - customers:write
        - members:read
        - members:write
        - wallets:read
        - wallets:write
        - disputes:read
        - disputes:write
        - customer_meters:read
        - customer_sessions:write
        - member_sessions:write
        - customer_seats:read
        - customer_seats:write
        - orders:read
        - orders:write
        - refunds:read
        - refunds:write
        - payments:read
        - metrics:read
        - metrics:write
        - webhooks:read
        - webhooks:write
        - license_keys:read
        - license_keys:write
        - customer_portal:read
        - customer_portal:write
        - notifications:read
        - notifications:write
        - notification_recipients:read
        - notification_recipients:write
        - organization_access_tokens:read
        - organization_access_tokens:write
      title: Scope
      enumNames:
        benefits:read: Read benefits
        benefits:write: Create or modify benefits
        checkout_links:read: Read checkout links
        checkout_links:write: Create or modify checkout links
        checkouts:read: Read checkout sessions
        checkouts:write: Create or modify checkout sessions
        custom_fields:read: Read custom fields
        custom_fields:write: Create or modify custom fields
        customer_meters:read: Read customer meters
        customer_portal:read: Read your orders, subscriptions and benefits
        customer_portal:write: Create or modify your orders, subscriptions and benefits
        customer_seats:read: Read customer seats
        customer_seats:write: Create or modify customer seats
        customer_sessions:write: Create or modify customer sessions
        customers:read: Read customers
        customers:write: Create or modify customers
        discounts:read: Read discounts
        discounts:write: Create or modify discounts
        disputes:read: Read disputes
        disputes:write: Create or modify disputes
        email: Read your email address
        events:read: Read events
        events:write: Create events
        files:read: Read file uploads
        files:write: Create or modify file uploads
        license_keys:read: Read license keys
        license_keys:write: Modify license keys
        member_sessions:write: Create or modify member sessions
        members:read: Read members
        members:write: Create or modify members
        meters:read: Read meters
        meters:write: Create or modify meters
        metrics:read: Read metrics
        metrics:write: Create or modify metric definitions
        notification_recipients:read: Read notification recipients
        notification_recipients:write: Create or modify notification recipients
        notifications:read: Read notifications
        notifications:write: Mark notifications as read
        openid: OpenID
        orders:read: Read orders made on your organizations
        orders:write: Modify orders made on your organizations
        organization_access_tokens:read: Read organization access tokens
        organization_access_tokens:write: Create or modify organization access tokens
        organizations:read: Read your organizations
        organizations:write: Create or modify organizations
        payments:read: Read payments made on your organizations
        payouts:read: Read payouts
        payouts:write: Create or modify payouts
        products:read: Read products
        products:write: Create or modify products
        profile: Read your profile
        refunds:read: Read refunds made on your organizations
        refunds:write: Create or modify refunds
        subscriptions:read: Read subscriptions made on your organizations
        subscriptions:write: Create or modify subscriptions made on your organizations
        transactions:read: Read transactions
        transactions:write: Create or modify transactions
        user:read: Read your user account
        user:write: Manage your user account
        wallets:read: Read wallets
        wallets:write: Create or modify wallets
        webhooks:read: Read webhooks
        webhooks:write: Create or modify webhooks
    AuthorizeOrganization:
      properties:
        id:
          type: string
          format: uuid4
          title: Id
        slug:
          type: string
          title: Slug
        avatar_url:
          anyOf:
            - type: string
            - type: 'null'
          title: Avatar Url
      type: object
      required:
        - id
        - slug
        - avatar_url
      title: AuthorizeOrganization
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

````
