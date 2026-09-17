> ## Documentation Index
> Fetch the complete documentation index at: https://polar.sh/docs/llms.txt
> Use this file to discover all available pages before exploring further.

# Get Event

> Get an event by ID.

**Scopes**: `events:read` `events:write`



## OpenAPI

````yaml /openapi/2026-10.openapi.json get /v1/events/{id}
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
  /v1/events/{id}:
    get:
      tags:
        - events
        - public
        - mcp
        - cli
      summary: Get Event
      description: |-
        Get an event by ID.

        **Scopes**: `events:read` `events:write`
      operationId: events:get
      parameters:
        - name: id
          in: path
          required: true
          schema:
            type: string
            format: uuid4
            description: The event ID.
            title: Id
          description: The event ID.
      responses:
        '200':
          description: Successful Response
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Event'
        '404':
          description: Event not found.
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
            - events:read
            - events:write
        - pat:
            - events:read
            - events:write
        - oat:
            - events:read
            - events:write
      x-codeSamples:
        - lang: python
          source: |
            from polar.v2026_10 import Polar

            polar = Polar("polar_oat_xxx")

            response = polar.events.get(
                '00000000-0000-4000-8000-000000000000',
            )
            print(response)
        - lang: typescript
          source: |
            import { createPolar } from "@polar-sh/sdk/2026-10";

            const polar = createPolar({
              accessToken: "polar_oat_xxx",
            });

            const response = await polar.events.get(
              "00000000-0000-4000-8000-000000000000",
            );
            console.log(response);
components:
  schemas:
    Event:
      oneOf:
        - $ref: '#/components/schemas/SystemEvent'
        - $ref: '#/components/schemas/UserEvent'
      discriminator:
        propertyName: source
        mapping:
          system:
            $ref: '#/components/schemas/SystemEvent'
          user:
            $ref: '#/components/schemas/UserEvent'
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
    SystemEvent:
      oneOf:
        - $ref: '#/components/schemas/MeterCreditEvent'
        - $ref: '#/components/schemas/MeterResetEvent'
        - $ref: '#/components/schemas/BenefitGrantedEvent'
        - $ref: '#/components/schemas/BenefitCycledEvent'
        - $ref: '#/components/schemas/BenefitUpdatedEvent'
        - $ref: '#/components/schemas/BenefitRevokedEvent'
        - $ref: '#/components/schemas/SubscriptionCreatedEvent'
        - $ref: '#/components/schemas/SubscriptionUpdatedEvent'
        - $ref: '#/components/schemas/SubscriptionCycledEvent'
        - $ref: '#/components/schemas/SubscriptionCanceledEvent'
        - $ref: '#/components/schemas/SubscriptionRevokedEvent'
        - $ref: '#/components/schemas/SubscriptionPastDueEvent'
        - $ref: '#/components/schemas/SubscriptionReactivatedEvent'
        - $ref: '#/components/schemas/SubscriptionReinstatedEvent'
        - $ref: '#/components/schemas/SubscriptionPausedEvent'
        - $ref: '#/components/schemas/SubscriptionResumedEvent'
        - $ref: '#/components/schemas/SubscriptionMigratedEvent'
        - $ref: '#/components/schemas/SubscriptionUncanceledEvent'
        - $ref: '#/components/schemas/SubscriptionProductUpdatedEvent'
        - $ref: '#/components/schemas/SubscriptionSeatsUpdatedEvent'
        - $ref: '#/components/schemas/SubscriptionUnitsUpdatedEvent'
        - $ref: '#/components/schemas/SubscriptionBillingPeriodUpdatedEvent'
        - $ref: '#/components/schemas/SubscriptionUpdateClearedEvent'
        - $ref: '#/components/schemas/OrderPaidEvent'
        - $ref: '#/components/schemas/OrderRefundedEvent'
        - $ref: '#/components/schemas/OrderVoidedEvent'
        - $ref: '#/components/schemas/OrderUnvoidedEvent'
        - $ref: '#/components/schemas/CheckoutCreatedEvent'
        - $ref: '#/components/schemas/CustomerCreatedEvent'
        - $ref: '#/components/schemas/CustomerUpdatedEvent'
        - $ref: '#/components/schemas/CustomerDeletedEvent'
        - $ref: '#/components/schemas/BalanceOrderEvent'
        - $ref: '#/components/schemas/BalanceCreditOrderEvent'
        - $ref: '#/components/schemas/BalanceRefundEvent'
        - $ref: '#/components/schemas/BalanceRefundReversalEvent'
        - $ref: '#/components/schemas/BalanceDisputeEvent'
        - $ref: '#/components/schemas/BalanceDisputeReversalEvent'
      discriminator:
        propertyName: name
        mapping:
          balance.credit_order:
            $ref: '#/components/schemas/BalanceCreditOrderEvent'
          balance.dispute:
            $ref: '#/components/schemas/BalanceDisputeEvent'
          balance.dispute_reversal:
            $ref: '#/components/schemas/BalanceDisputeReversalEvent'
          balance.order:
            $ref: '#/components/schemas/BalanceOrderEvent'
          balance.refund:
            $ref: '#/components/schemas/BalanceRefundEvent'
          balance.refund_reversal:
            $ref: '#/components/schemas/BalanceRefundReversalEvent'
          benefit.cycled:
            $ref: '#/components/schemas/BenefitCycledEvent'
          benefit.granted:
            $ref: '#/components/schemas/BenefitGrantedEvent'
          benefit.revoked:
            $ref: '#/components/schemas/BenefitRevokedEvent'
          benefit.updated:
            $ref: '#/components/schemas/BenefitUpdatedEvent'
          checkout.created:
            $ref: '#/components/schemas/CheckoutCreatedEvent'
          customer.created:
            $ref: '#/components/schemas/CustomerCreatedEvent'
          customer.deleted:
            $ref: '#/components/schemas/CustomerDeletedEvent'
          customer.updated:
            $ref: '#/components/schemas/CustomerUpdatedEvent'
          meter.credited:
            $ref: '#/components/schemas/MeterCreditEvent'
          meter.reset:
            $ref: '#/components/schemas/MeterResetEvent'
          order.paid:
            $ref: '#/components/schemas/OrderPaidEvent'
          order.refunded:
            $ref: '#/components/schemas/OrderRefundedEvent'
          order.unvoided:
            $ref: '#/components/schemas/OrderUnvoidedEvent'
          order.voided:
            $ref: '#/components/schemas/OrderVoidedEvent'
          subscription.billing_period_updated:
            $ref: '#/components/schemas/SubscriptionBillingPeriodUpdatedEvent'
          subscription.canceled:
            $ref: '#/components/schemas/SubscriptionCanceledEvent'
          subscription.created:
            $ref: '#/components/schemas/SubscriptionCreatedEvent'
          subscription.cycled:
            $ref: '#/components/schemas/SubscriptionCycledEvent'
          subscription.migrated:
            $ref: '#/components/schemas/SubscriptionMigratedEvent'
          subscription.past_due:
            $ref: '#/components/schemas/SubscriptionPastDueEvent'
          subscription.paused:
            $ref: '#/components/schemas/SubscriptionPausedEvent'
          subscription.product_updated:
            $ref: '#/components/schemas/SubscriptionProductUpdatedEvent'
          subscription.reactivated:
            $ref: '#/components/schemas/SubscriptionReactivatedEvent'
          subscription.reinstated:
            $ref: '#/components/schemas/SubscriptionReinstatedEvent'
          subscription.resumed:
            $ref: '#/components/schemas/SubscriptionResumedEvent'
          subscription.revoked:
            $ref: '#/components/schemas/SubscriptionRevokedEvent'
          subscription.seats_updated:
            $ref: '#/components/schemas/SubscriptionSeatsUpdatedEvent'
          subscription.uncanceled:
            $ref: '#/components/schemas/SubscriptionUncanceledEvent'
          subscription.units_updated:
            $ref: '#/components/schemas/SubscriptionUnitsUpdatedEvent'
          subscription.update_cleared:
            $ref: '#/components/schemas/SubscriptionUpdateClearedEvent'
          subscription.updated:
            $ref: '#/components/schemas/SubscriptionUpdatedEvent'
    UserEvent:
      properties:
        id:
          type: string
          format: uuid4
          title: Id
          description: The ID of the object.
        timestamp:
          type: string
          format: date-time
          title: Timestamp
          description: The timestamp of the event.
          examples:
            - '2026-01-01T00:00:00.000000Z'
        organization_id:
          type: string
          format: uuid4
          title: Organization Id
          description: The ID of the organization owning the event.
          examples:
            - 1dbfc517-0bbf-4301-9ba8-555ca42b9737
        customer_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Customer Id
          description: >-
            ID of the customer in your Polar organization associated with the
            event.
        customer:
          anyOf:
            - $ref: '#/components/schemas/Customer'
            - type: 'null'
          description: The customer associated with the event.
        external_customer_id:
          anyOf:
            - type: string
            - type: 'null'
          title: External Customer Id
          description: ID of the customer in your system associated with the event.
        member_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Member Id
          description: >-
            ID of the member within the customer's organization who performed
            the action inside B2B.
        external_member_id:
          anyOf:
            - type: string
            - type: 'null'
          title: External Member Id
          description: >-
            ID of the member in your system within the customer's organization
            who performed the action inside B2B.
        child_count:
          type: integer
          title: Child Count
          description: Number of direct child events linked to this event.
          default: 0
        parent_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Parent Id
          description: The ID of the parent event.
        label:
          type: string
          title: Label
          description: Human readable label of the event type.
        name:
          type: string
          title: Name
          description: The name of the event.
        source:
          type: string
          const: user
          title: Source
          description: >-
            The source of the event. `system` events are created by Polar.
            `user` events are the one you create through our ingestion API.
        metadata:
          $ref: '#/components/schemas/EventMetadataOutput'
      type: object
      required:
        - id
        - timestamp
        - organization_id
        - customer_id
        - customer
        - external_customer_id
        - label
        - name
        - source
        - metadata
      title: UserEvent
      description: An event you created through the ingestion API.
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
    MeterCreditEvent:
      properties:
        id:
          type: string
          format: uuid4
          title: Id
          description: The ID of the object.
        timestamp:
          type: string
          format: date-time
          title: Timestamp
          description: The timestamp of the event.
          examples:
            - '2026-01-01T00:00:00.000000Z'
        organization_id:
          type: string
          format: uuid4
          title: Organization Id
          description: The ID of the organization owning the event.
          examples:
            - 1dbfc517-0bbf-4301-9ba8-555ca42b9737
        customer_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Customer Id
          description: >-
            ID of the customer in your Polar organization associated with the
            event.
        customer:
          anyOf:
            - $ref: '#/components/schemas/Customer'
            - type: 'null'
          description: The customer associated with the event.
        external_customer_id:
          anyOf:
            - type: string
            - type: 'null'
          title: External Customer Id
          description: ID of the customer in your system associated with the event.
        member_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Member Id
          description: >-
            ID of the member within the customer's organization who performed
            the action inside B2B.
        external_member_id:
          anyOf:
            - type: string
            - type: 'null'
          title: External Member Id
          description: >-
            ID of the member in your system within the customer's organization
            who performed the action inside B2B.
        child_count:
          type: integer
          title: Child Count
          description: Number of direct child events linked to this event.
          default: 0
        parent_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Parent Id
          description: The ID of the parent event.
        label:
          type: string
          title: Label
          description: Human readable label of the event type.
        source:
          type: string
          const: system
          title: Source
          description: >-
            The source of the event. `system` events are created by Polar.
            `user` events are the one you create through our ingestion API.
        name:
          type: string
          const: meter.credited
          title: Name
          description: The name of the event.
        metadata:
          $ref: '#/components/schemas/MeterCreditedMetadata'
      type: object
      required:
        - id
        - timestamp
        - organization_id
        - customer_id
        - customer
        - external_customer_id
        - label
        - source
        - name
        - metadata
      title: MeterCreditEvent
      description: An event created by Polar when credits are added to a customer meter.
    MeterResetEvent:
      properties:
        id:
          type: string
          format: uuid4
          title: Id
          description: The ID of the object.
        timestamp:
          type: string
          format: date-time
          title: Timestamp
          description: The timestamp of the event.
          examples:
            - '2026-01-01T00:00:00.000000Z'
        organization_id:
          type: string
          format: uuid4
          title: Organization Id
          description: The ID of the organization owning the event.
          examples:
            - 1dbfc517-0bbf-4301-9ba8-555ca42b9737
        customer_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Customer Id
          description: >-
            ID of the customer in your Polar organization associated with the
            event.
        customer:
          anyOf:
            - $ref: '#/components/schemas/Customer'
            - type: 'null'
          description: The customer associated with the event.
        external_customer_id:
          anyOf:
            - type: string
            - type: 'null'
          title: External Customer Id
          description: ID of the customer in your system associated with the event.
        member_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Member Id
          description: >-
            ID of the member within the customer's organization who performed
            the action inside B2B.
        external_member_id:
          anyOf:
            - type: string
            - type: 'null'
          title: External Member Id
          description: >-
            ID of the member in your system within the customer's organization
            who performed the action inside B2B.
        child_count:
          type: integer
          title: Child Count
          description: Number of direct child events linked to this event.
          default: 0
        parent_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Parent Id
          description: The ID of the parent event.
        label:
          type: string
          title: Label
          description: Human readable label of the event type.
        source:
          type: string
          const: system
          title: Source
          description: >-
            The source of the event. `system` events are created by Polar.
            `user` events are the one you create through our ingestion API.
        name:
          type: string
          const: meter.reset
          title: Name
          description: The name of the event.
        metadata:
          $ref: '#/components/schemas/MeterResetMetadata'
      type: object
      required:
        - id
        - timestamp
        - organization_id
        - customer_id
        - customer
        - external_customer_id
        - label
        - source
        - name
        - metadata
      title: MeterResetEvent
      description: An event created by Polar when a customer meter is reset.
    BenefitGrantedEvent:
      properties:
        id:
          type: string
          format: uuid4
          title: Id
          description: The ID of the object.
        timestamp:
          type: string
          format: date-time
          title: Timestamp
          description: The timestamp of the event.
          examples:
            - '2026-01-01T00:00:00.000000Z'
        organization_id:
          type: string
          format: uuid4
          title: Organization Id
          description: The ID of the organization owning the event.
          examples:
            - 1dbfc517-0bbf-4301-9ba8-555ca42b9737
        customer_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Customer Id
          description: >-
            ID of the customer in your Polar organization associated with the
            event.
        customer:
          anyOf:
            - $ref: '#/components/schemas/Customer'
            - type: 'null'
          description: The customer associated with the event.
        external_customer_id:
          anyOf:
            - type: string
            - type: 'null'
          title: External Customer Id
          description: ID of the customer in your system associated with the event.
        member_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Member Id
          description: >-
            ID of the member within the customer's organization who performed
            the action inside B2B.
        external_member_id:
          anyOf:
            - type: string
            - type: 'null'
          title: External Member Id
          description: >-
            ID of the member in your system within the customer's organization
            who performed the action inside B2B.
        child_count:
          type: integer
          title: Child Count
          description: Number of direct child events linked to this event.
          default: 0
        parent_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Parent Id
          description: The ID of the parent event.
        label:
          type: string
          title: Label
          description: Human readable label of the event type.
        source:
          type: string
          const: system
          title: Source
          description: >-
            The source of the event. `system` events are created by Polar.
            `user` events are the one you create through our ingestion API.
        name:
          type: string
          const: benefit.granted
          title: Name
          description: The name of the event.
        metadata:
          $ref: '#/components/schemas/BenefitGrantMetadata'
      type: object
      required:
        - id
        - timestamp
        - organization_id
        - customer_id
        - customer
        - external_customer_id
        - label
        - source
        - name
        - metadata
      title: BenefitGrantedEvent
      description: An event created by Polar when a benefit is granted to a customer.
    BenefitCycledEvent:
      properties:
        id:
          type: string
          format: uuid4
          title: Id
          description: The ID of the object.
        timestamp:
          type: string
          format: date-time
          title: Timestamp
          description: The timestamp of the event.
          examples:
            - '2026-01-01T00:00:00.000000Z'
        organization_id:
          type: string
          format: uuid4
          title: Organization Id
          description: The ID of the organization owning the event.
          examples:
            - 1dbfc517-0bbf-4301-9ba8-555ca42b9737
        customer_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Customer Id
          description: >-
            ID of the customer in your Polar organization associated with the
            event.
        customer:
          anyOf:
            - $ref: '#/components/schemas/Customer'
            - type: 'null'
          description: The customer associated with the event.
        external_customer_id:
          anyOf:
            - type: string
            - type: 'null'
          title: External Customer Id
          description: ID of the customer in your system associated with the event.
        member_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Member Id
          description: >-
            ID of the member within the customer's organization who performed
            the action inside B2B.
        external_member_id:
          anyOf:
            - type: string
            - type: 'null'
          title: External Member Id
          description: >-
            ID of the member in your system within the customer's organization
            who performed the action inside B2B.
        child_count:
          type: integer
          title: Child Count
          description: Number of direct child events linked to this event.
          default: 0
        parent_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Parent Id
          description: The ID of the parent event.
        label:
          type: string
          title: Label
          description: Human readable label of the event type.
        source:
          type: string
          const: system
          title: Source
          description: >-
            The source of the event. `system` events are created by Polar.
            `user` events are the one you create through our ingestion API.
        name:
          type: string
          const: benefit.cycled
          title: Name
          description: The name of the event.
        metadata:
          $ref: '#/components/schemas/BenefitGrantMetadata'
      type: object
      required:
        - id
        - timestamp
        - organization_id
        - customer_id
        - customer
        - external_customer_id
        - label
        - source
        - name
        - metadata
      title: BenefitCycledEvent
      description: An event created by Polar when a benefit is cycled.
    BenefitUpdatedEvent:
      properties:
        id:
          type: string
          format: uuid4
          title: Id
          description: The ID of the object.
        timestamp:
          type: string
          format: date-time
          title: Timestamp
          description: The timestamp of the event.
          examples:
            - '2026-01-01T00:00:00.000000Z'
        organization_id:
          type: string
          format: uuid4
          title: Organization Id
          description: The ID of the organization owning the event.
          examples:
            - 1dbfc517-0bbf-4301-9ba8-555ca42b9737
        customer_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Customer Id
          description: >-
            ID of the customer in your Polar organization associated with the
            event.
        customer:
          anyOf:
            - $ref: '#/components/schemas/Customer'
            - type: 'null'
          description: The customer associated with the event.
        external_customer_id:
          anyOf:
            - type: string
            - type: 'null'
          title: External Customer Id
          description: ID of the customer in your system associated with the event.
        member_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Member Id
          description: >-
            ID of the member within the customer's organization who performed
            the action inside B2B.
        external_member_id:
          anyOf:
            - type: string
            - type: 'null'
          title: External Member Id
          description: >-
            ID of the member in your system within the customer's organization
            who performed the action inside B2B.
        child_count:
          type: integer
          title: Child Count
          description: Number of direct child events linked to this event.
          default: 0
        parent_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Parent Id
          description: The ID of the parent event.
        label:
          type: string
          title: Label
          description: Human readable label of the event type.
        source:
          type: string
          const: system
          title: Source
          description: >-
            The source of the event. `system` events are created by Polar.
            `user` events are the one you create through our ingestion API.
        name:
          type: string
          const: benefit.updated
          title: Name
          description: The name of the event.
        metadata:
          $ref: '#/components/schemas/BenefitGrantMetadata'
      type: object
      required:
        - id
        - timestamp
        - organization_id
        - customer_id
        - customer
        - external_customer_id
        - label
        - source
        - name
        - metadata
      title: BenefitUpdatedEvent
      description: An event created by Polar when a benefit is updated.
    BenefitRevokedEvent:
      properties:
        id:
          type: string
          format: uuid4
          title: Id
          description: The ID of the object.
        timestamp:
          type: string
          format: date-time
          title: Timestamp
          description: The timestamp of the event.
          examples:
            - '2026-01-01T00:00:00.000000Z'
        organization_id:
          type: string
          format: uuid4
          title: Organization Id
          description: The ID of the organization owning the event.
          examples:
            - 1dbfc517-0bbf-4301-9ba8-555ca42b9737
        customer_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Customer Id
          description: >-
            ID of the customer in your Polar organization associated with the
            event.
        customer:
          anyOf:
            - $ref: '#/components/schemas/Customer'
            - type: 'null'
          description: The customer associated with the event.
        external_customer_id:
          anyOf:
            - type: string
            - type: 'null'
          title: External Customer Id
          description: ID of the customer in your system associated with the event.
        member_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Member Id
          description: >-
            ID of the member within the customer's organization who performed
            the action inside B2B.
        external_member_id:
          anyOf:
            - type: string
            - type: 'null'
          title: External Member Id
          description: >-
            ID of the member in your system within the customer's organization
            who performed the action inside B2B.
        child_count:
          type: integer
          title: Child Count
          description: Number of direct child events linked to this event.
          default: 0
        parent_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Parent Id
          description: The ID of the parent event.
        label:
          type: string
          title: Label
          description: Human readable label of the event type.
        source:
          type: string
          const: system
          title: Source
          description: >-
            The source of the event. `system` events are created by Polar.
            `user` events are the one you create through our ingestion API.
        name:
          type: string
          const: benefit.revoked
          title: Name
          description: The name of the event.
        metadata:
          $ref: '#/components/schemas/BenefitGrantMetadata'
      type: object
      required:
        - id
        - timestamp
        - organization_id
        - customer_id
        - customer
        - external_customer_id
        - label
        - source
        - name
        - metadata
      title: BenefitRevokedEvent
      description: An event created by Polar when a benefit is revoked from a customer.
    SubscriptionCreatedEvent:
      properties:
        id:
          type: string
          format: uuid4
          title: Id
          description: The ID of the object.
        timestamp:
          type: string
          format: date-time
          title: Timestamp
          description: The timestamp of the event.
          examples:
            - '2026-01-01T00:00:00.000000Z'
        organization_id:
          type: string
          format: uuid4
          title: Organization Id
          description: The ID of the organization owning the event.
          examples:
            - 1dbfc517-0bbf-4301-9ba8-555ca42b9737
        customer_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Customer Id
          description: >-
            ID of the customer in your Polar organization associated with the
            event.
        customer:
          anyOf:
            - $ref: '#/components/schemas/Customer'
            - type: 'null'
          description: The customer associated with the event.
        external_customer_id:
          anyOf:
            - type: string
            - type: 'null'
          title: External Customer Id
          description: ID of the customer in your system associated with the event.
        member_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Member Id
          description: >-
            ID of the member within the customer's organization who performed
            the action inside B2B.
        external_member_id:
          anyOf:
            - type: string
            - type: 'null'
          title: External Member Id
          description: >-
            ID of the member in your system within the customer's organization
            who performed the action inside B2B.
        child_count:
          type: integer
          title: Child Count
          description: Number of direct child events linked to this event.
          default: 0
        parent_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Parent Id
          description: The ID of the parent event.
        label:
          type: string
          title: Label
          description: Human readable label of the event type.
        source:
          type: string
          const: system
          title: Source
          description: >-
            The source of the event. `system` events are created by Polar.
            `user` events are the one you create through our ingestion API.
        name:
          type: string
          const: subscription.created
          title: Name
          description: The name of the event.
        metadata:
          $ref: '#/components/schemas/SubscriptionCreatedMetadata'
      type: object
      required:
        - id
        - timestamp
        - organization_id
        - customer_id
        - customer
        - external_customer_id
        - label
        - source
        - name
        - metadata
      title: SubscriptionCreatedEvent
      description: An event created by Polar when a subscription is created.
    SubscriptionUpdatedEvent:
      properties:
        id:
          type: string
          format: uuid4
          title: Id
          description: The ID of the object.
        timestamp:
          type: string
          format: date-time
          title: Timestamp
          description: The timestamp of the event.
          examples:
            - '2026-01-01T00:00:00.000000Z'
        organization_id:
          type: string
          format: uuid4
          title: Organization Id
          description: The ID of the organization owning the event.
          examples:
            - 1dbfc517-0bbf-4301-9ba8-555ca42b9737
        customer_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Customer Id
          description: >-
            ID of the customer in your Polar organization associated with the
            event.
        customer:
          anyOf:
            - $ref: '#/components/schemas/Customer'
            - type: 'null'
          description: The customer associated with the event.
        external_customer_id:
          anyOf:
            - type: string
            - type: 'null'
          title: External Customer Id
          description: ID of the customer in your system associated with the event.
        member_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Member Id
          description: >-
            ID of the member within the customer's organization who performed
            the action inside B2B.
        external_member_id:
          anyOf:
            - type: string
            - type: 'null'
          title: External Member Id
          description: >-
            ID of the member in your system within the customer's organization
            who performed the action inside B2B.
        child_count:
          type: integer
          title: Child Count
          description: Number of direct child events linked to this event.
          default: 0
        parent_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Parent Id
          description: The ID of the parent event.
        label:
          type: string
          title: Label
          description: Human readable label of the event type.
        source:
          type: string
          const: system
          title: Source
          description: >-
            The source of the event. `system` events are created by Polar.
            `user` events are the one you create through our ingestion API.
        name:
          type: string
          const: subscription.updated
          title: Name
          description: The name of the event.
        metadata:
          $ref: '#/components/schemas/SubscriptionUpdatedMetadata'
      type: object
      required:
        - id
        - timestamp
        - organization_id
        - customer_id
        - customer
        - external_customer_id
        - label
        - source
        - name
        - metadata
      title: SubscriptionUpdatedEvent
      description: An event created by Polar when a subscription is updated.
    SubscriptionCycledEvent:
      properties:
        id:
          type: string
          format: uuid4
          title: Id
          description: The ID of the object.
        timestamp:
          type: string
          format: date-time
          title: Timestamp
          description: The timestamp of the event.
          examples:
            - '2026-01-01T00:00:00.000000Z'
        organization_id:
          type: string
          format: uuid4
          title: Organization Id
          description: The ID of the organization owning the event.
          examples:
            - 1dbfc517-0bbf-4301-9ba8-555ca42b9737
        customer_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Customer Id
          description: >-
            ID of the customer in your Polar organization associated with the
            event.
        customer:
          anyOf:
            - $ref: '#/components/schemas/Customer'
            - type: 'null'
          description: The customer associated with the event.
        external_customer_id:
          anyOf:
            - type: string
            - type: 'null'
          title: External Customer Id
          description: ID of the customer in your system associated with the event.
        member_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Member Id
          description: >-
            ID of the member within the customer's organization who performed
            the action inside B2B.
        external_member_id:
          anyOf:
            - type: string
            - type: 'null'
          title: External Member Id
          description: >-
            ID of the member in your system within the customer's organization
            who performed the action inside B2B.
        child_count:
          type: integer
          title: Child Count
          description: Number of direct child events linked to this event.
          default: 0
        parent_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Parent Id
          description: The ID of the parent event.
        label:
          type: string
          title: Label
          description: Human readable label of the event type.
        source:
          type: string
          const: system
          title: Source
          description: >-
            The source of the event. `system` events are created by Polar.
            `user` events are the one you create through our ingestion API.
        name:
          type: string
          const: subscription.cycled
          title: Name
          description: The name of the event.
        metadata:
          $ref: '#/components/schemas/SubscriptionCycledMetadata'
      type: object
      required:
        - id
        - timestamp
        - organization_id
        - customer_id
        - customer
        - external_customer_id
        - label
        - source
        - name
        - metadata
      title: SubscriptionCycledEvent
      description: An event created by Polar when a subscription is cycled.
    SubscriptionCanceledEvent:
      properties:
        id:
          type: string
          format: uuid4
          title: Id
          description: The ID of the object.
        timestamp:
          type: string
          format: date-time
          title: Timestamp
          description: The timestamp of the event.
          examples:
            - '2026-01-01T00:00:00.000000Z'
        organization_id:
          type: string
          format: uuid4
          title: Organization Id
          description: The ID of the organization owning the event.
          examples:
            - 1dbfc517-0bbf-4301-9ba8-555ca42b9737
        customer_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Customer Id
          description: >-
            ID of the customer in your Polar organization associated with the
            event.
        customer:
          anyOf:
            - $ref: '#/components/schemas/Customer'
            - type: 'null'
          description: The customer associated with the event.
        external_customer_id:
          anyOf:
            - type: string
            - type: 'null'
          title: External Customer Id
          description: ID of the customer in your system associated with the event.
        member_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Member Id
          description: >-
            ID of the member within the customer's organization who performed
            the action inside B2B.
        external_member_id:
          anyOf:
            - type: string
            - type: 'null'
          title: External Member Id
          description: >-
            ID of the member in your system within the customer's organization
            who performed the action inside B2B.
        child_count:
          type: integer
          title: Child Count
          description: Number of direct child events linked to this event.
          default: 0
        parent_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Parent Id
          description: The ID of the parent event.
        label:
          type: string
          title: Label
          description: Human readable label of the event type.
        source:
          type: string
          const: system
          title: Source
          description: >-
            The source of the event. `system` events are created by Polar.
            `user` events are the one you create through our ingestion API.
        name:
          type: string
          const: subscription.canceled
          title: Name
          description: The name of the event.
        metadata:
          $ref: '#/components/schemas/SubscriptionCanceledMetadata'
      type: object
      required:
        - id
        - timestamp
        - organization_id
        - customer_id
        - customer
        - external_customer_id
        - label
        - source
        - name
        - metadata
      title: SubscriptionCanceledEvent
      description: An event created by Polar when a subscription is canceled.
    SubscriptionRevokedEvent:
      properties:
        id:
          type: string
          format: uuid4
          title: Id
          description: The ID of the object.
        timestamp:
          type: string
          format: date-time
          title: Timestamp
          description: The timestamp of the event.
          examples:
            - '2026-01-01T00:00:00.000000Z'
        organization_id:
          type: string
          format: uuid4
          title: Organization Id
          description: The ID of the organization owning the event.
          examples:
            - 1dbfc517-0bbf-4301-9ba8-555ca42b9737
        customer_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Customer Id
          description: >-
            ID of the customer in your Polar organization associated with the
            event.
        customer:
          anyOf:
            - $ref: '#/components/schemas/Customer'
            - type: 'null'
          description: The customer associated with the event.
        external_customer_id:
          anyOf:
            - type: string
            - type: 'null'
          title: External Customer Id
          description: ID of the customer in your system associated with the event.
        member_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Member Id
          description: >-
            ID of the member within the customer's organization who performed
            the action inside B2B.
        external_member_id:
          anyOf:
            - type: string
            - type: 'null'
          title: External Member Id
          description: >-
            ID of the member in your system within the customer's organization
            who performed the action inside B2B.
        child_count:
          type: integer
          title: Child Count
          description: Number of direct child events linked to this event.
          default: 0
        parent_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Parent Id
          description: The ID of the parent event.
        label:
          type: string
          title: Label
          description: Human readable label of the event type.
        source:
          type: string
          const: system
          title: Source
          description: >-
            The source of the event. `system` events are created by Polar.
            `user` events are the one you create through our ingestion API.
        name:
          type: string
          const: subscription.revoked
          title: Name
          description: The name of the event.
        metadata:
          $ref: '#/components/schemas/SubscriptionRevokedMetadata'
      type: object
      required:
        - id
        - timestamp
        - organization_id
        - customer_id
        - customer
        - external_customer_id
        - label
        - source
        - name
        - metadata
      title: SubscriptionRevokedEvent
      description: >-
        An event created by Polar when a subscription is revoked from a
        customer.
    SubscriptionPastDueEvent:
      properties:
        id:
          type: string
          format: uuid4
          title: Id
          description: The ID of the object.
        timestamp:
          type: string
          format: date-time
          title: Timestamp
          description: The timestamp of the event.
          examples:
            - '2026-01-01T00:00:00.000000Z'
        organization_id:
          type: string
          format: uuid4
          title: Organization Id
          description: The ID of the organization owning the event.
          examples:
            - 1dbfc517-0bbf-4301-9ba8-555ca42b9737
        customer_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Customer Id
          description: >-
            ID of the customer in your Polar organization associated with the
            event.
        customer:
          anyOf:
            - $ref: '#/components/schemas/Customer'
            - type: 'null'
          description: The customer associated with the event.
        external_customer_id:
          anyOf:
            - type: string
            - type: 'null'
          title: External Customer Id
          description: ID of the customer in your system associated with the event.
        member_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Member Id
          description: >-
            ID of the member within the customer's organization who performed
            the action inside B2B.
        external_member_id:
          anyOf:
            - type: string
            - type: 'null'
          title: External Member Id
          description: >-
            ID of the member in your system within the customer's organization
            who performed the action inside B2B.
        child_count:
          type: integer
          title: Child Count
          description: Number of direct child events linked to this event.
          default: 0
        parent_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Parent Id
          description: The ID of the parent event.
        label:
          type: string
          title: Label
          description: Human readable label of the event type.
        source:
          type: string
          const: system
          title: Source
          description: >-
            The source of the event. `system` events are created by Polar.
            `user` events are the one you create through our ingestion API.
        name:
          type: string
          const: subscription.past_due
          title: Name
          description: The name of the event.
        metadata:
          $ref: '#/components/schemas/SubscriptionPastDueMetadata'
      type: object
      required:
        - id
        - timestamp
        - organization_id
        - customer_id
        - customer
        - external_customer_id
        - label
        - source
        - name
        - metadata
      title: SubscriptionPastDueEvent
      description: An event created by Polar when a subscription becomes past due.
    SubscriptionReactivatedEvent:
      properties:
        id:
          type: string
          format: uuid4
          title: Id
          description: The ID of the object.
        timestamp:
          type: string
          format: date-time
          title: Timestamp
          description: The timestamp of the event.
          examples:
            - '2026-01-01T00:00:00.000000Z'
        organization_id:
          type: string
          format: uuid4
          title: Organization Id
          description: The ID of the organization owning the event.
          examples:
            - 1dbfc517-0bbf-4301-9ba8-555ca42b9737
        customer_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Customer Id
          description: >-
            ID of the customer in your Polar organization associated with the
            event.
        customer:
          anyOf:
            - $ref: '#/components/schemas/Customer'
            - type: 'null'
          description: The customer associated with the event.
        external_customer_id:
          anyOf:
            - type: string
            - type: 'null'
          title: External Customer Id
          description: ID of the customer in your system associated with the event.
        member_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Member Id
          description: >-
            ID of the member within the customer's organization who performed
            the action inside B2B.
        external_member_id:
          anyOf:
            - type: string
            - type: 'null'
          title: External Member Id
          description: >-
            ID of the member in your system within the customer's organization
            who performed the action inside B2B.
        child_count:
          type: integer
          title: Child Count
          description: Number of direct child events linked to this event.
          default: 0
        parent_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Parent Id
          description: The ID of the parent event.
        label:
          type: string
          title: Label
          description: Human readable label of the event type.
        source:
          type: string
          const: system
          title: Source
          description: >-
            The source of the event. `system` events are created by Polar.
            `user` events are the one you create through our ingestion API.
        name:
          type: string
          const: subscription.reactivated
          title: Name
          description: The name of the event.
        metadata:
          $ref: '#/components/schemas/SubscriptionReactivatedMetadata'
      type: object
      required:
        - id
        - timestamp
        - organization_id
        - customer_id
        - customer
        - external_customer_id
        - label
        - source
        - name
        - metadata
      title: SubscriptionReactivatedEvent
      description: An event created by Polar when a past due subscription is recovered.
    SubscriptionReinstatedEvent:
      properties:
        id:
          type: string
          format: uuid4
          title: Id
          description: The ID of the object.
        timestamp:
          type: string
          format: date-time
          title: Timestamp
          description: The timestamp of the event.
          examples:
            - '2026-01-01T00:00:00.000000Z'
        organization_id:
          type: string
          format: uuid4
          title: Organization Id
          description: The ID of the organization owning the event.
          examples:
            - 1dbfc517-0bbf-4301-9ba8-555ca42b9737
        customer_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Customer Id
          description: >-
            ID of the customer in your Polar organization associated with the
            event.
        customer:
          anyOf:
            - $ref: '#/components/schemas/Customer'
            - type: 'null'
          description: The customer associated with the event.
        external_customer_id:
          anyOf:
            - type: string
            - type: 'null'
          title: External Customer Id
          description: ID of the customer in your system associated with the event.
        member_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Member Id
          description: >-
            ID of the member within the customer's organization who performed
            the action inside B2B.
        external_member_id:
          anyOf:
            - type: string
            - type: 'null'
          title: External Member Id
          description: >-
            ID of the member in your system within the customer's organization
            who performed the action inside B2B.
        child_count:
          type: integer
          title: Child Count
          description: Number of direct child events linked to this event.
          default: 0
        parent_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Parent Id
          description: The ID of the parent event.
        label:
          type: string
          title: Label
          description: Human readable label of the event type.
        source:
          type: string
          const: system
          title: Source
          description: >-
            The source of the event. `system` events are created by Polar.
            `user` events are the one you create through our ingestion API.
        name:
          type: string
          const: subscription.reinstated
          title: Name
          description: The name of the event.
        metadata:
          $ref: '#/components/schemas/SubscriptionReinstatedMetadata'
      type: object
      required:
        - id
        - timestamp
        - organization_id
        - customer_id
        - customer
        - external_customer_id
        - label
        - source
        - name
        - metadata
      title: SubscriptionReinstatedEvent
      description: An event created by Polar when a canceled subscription is reinstated.
    SubscriptionPausedEvent:
      properties:
        id:
          type: string
          format: uuid4
          title: Id
          description: The ID of the object.
        timestamp:
          type: string
          format: date-time
          title: Timestamp
          description: The timestamp of the event.
          examples:
            - '2026-01-01T00:00:00.000000Z'
        organization_id:
          type: string
          format: uuid4
          title: Organization Id
          description: The ID of the organization owning the event.
          examples:
            - 1dbfc517-0bbf-4301-9ba8-555ca42b9737
        customer_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Customer Id
          description: >-
            ID of the customer in your Polar organization associated with the
            event.
        customer:
          anyOf:
            - $ref: '#/components/schemas/Customer'
            - type: 'null'
          description: The customer associated with the event.
        external_customer_id:
          anyOf:
            - type: string
            - type: 'null'
          title: External Customer Id
          description: ID of the customer in your system associated with the event.
        member_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Member Id
          description: >-
            ID of the member within the customer's organization who performed
            the action inside B2B.
        external_member_id:
          anyOf:
            - type: string
            - type: 'null'
          title: External Member Id
          description: >-
            ID of the member in your system within the customer's organization
            who performed the action inside B2B.
        child_count:
          type: integer
          title: Child Count
          description: Number of direct child events linked to this event.
          default: 0
        parent_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Parent Id
          description: The ID of the parent event.
        label:
          type: string
          title: Label
          description: Human readable label of the event type.
        source:
          type: string
          const: system
          title: Source
          description: >-
            The source of the event. `system` events are created by Polar.
            `user` events are the one you create through our ingestion API.
        name:
          type: string
          const: subscription.paused
          title: Name
          description: The name of the event.
        metadata:
          $ref: '#/components/schemas/SubscriptionPausedMetadata'
      type: object
      required:
        - id
        - timestamp
        - organization_id
        - customer_id
        - customer
        - external_customer_id
        - label
        - source
        - name
        - metadata
      title: SubscriptionPausedEvent
      description: An event created by Polar when a subscription is paused.
    SubscriptionResumedEvent:
      properties:
        id:
          type: string
          format: uuid4
          title: Id
          description: The ID of the object.
        timestamp:
          type: string
          format: date-time
          title: Timestamp
          description: The timestamp of the event.
          examples:
            - '2026-01-01T00:00:00.000000Z'
        organization_id:
          type: string
          format: uuid4
          title: Organization Id
          description: The ID of the organization owning the event.
          examples:
            - 1dbfc517-0bbf-4301-9ba8-555ca42b9737
        customer_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Customer Id
          description: >-
            ID of the customer in your Polar organization associated with the
            event.
        customer:
          anyOf:
            - $ref: '#/components/schemas/Customer'
            - type: 'null'
          description: The customer associated with the event.
        external_customer_id:
          anyOf:
            - type: string
            - type: 'null'
          title: External Customer Id
          description: ID of the customer in your system associated with the event.
        member_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Member Id
          description: >-
            ID of the member within the customer's organization who performed
            the action inside B2B.
        external_member_id:
          anyOf:
            - type: string
            - type: 'null'
          title: External Member Id
          description: >-
            ID of the member in your system within the customer's organization
            who performed the action inside B2B.
        child_count:
          type: integer
          title: Child Count
          description: Number of direct child events linked to this event.
          default: 0
        parent_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Parent Id
          description: The ID of the parent event.
        label:
          type: string
          title: Label
          description: Human readable label of the event type.
        source:
          type: string
          const: system
          title: Source
          description: >-
            The source of the event. `system` events are created by Polar.
            `user` events are the one you create through our ingestion API.
        name:
          type: string
          const: subscription.resumed
          title: Name
          description: The name of the event.
        metadata:
          $ref: '#/components/schemas/SubscriptionResumedMetadata'
      type: object
      required:
        - id
        - timestamp
        - organization_id
        - customer_id
        - customer
        - external_customer_id
        - label
        - source
        - name
        - metadata
      title: SubscriptionResumedEvent
      description: An event created by Polar when a paused subscription is resumed.
    SubscriptionMigratedEvent:
      properties:
        id:
          type: string
          format: uuid4
          title: Id
          description: The ID of the object.
        timestamp:
          type: string
          format: date-time
          title: Timestamp
          description: The timestamp of the event.
          examples:
            - '2026-01-01T00:00:00.000000Z'
        organization_id:
          type: string
          format: uuid4
          title: Organization Id
          description: The ID of the organization owning the event.
          examples:
            - 1dbfc517-0bbf-4301-9ba8-555ca42b9737
        customer_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Customer Id
          description: >-
            ID of the customer in your Polar organization associated with the
            event.
        customer:
          anyOf:
            - $ref: '#/components/schemas/Customer'
            - type: 'null'
          description: The customer associated with the event.
        external_customer_id:
          anyOf:
            - type: string
            - type: 'null'
          title: External Customer Id
          description: ID of the customer in your system associated with the event.
        member_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Member Id
          description: >-
            ID of the member within the customer's organization who performed
            the action inside B2B.
        external_member_id:
          anyOf:
            - type: string
            - type: 'null'
          title: External Member Id
          description: >-
            ID of the member in your system within the customer's organization
            who performed the action inside B2B.
        child_count:
          type: integer
          title: Child Count
          description: Number of direct child events linked to this event.
          default: 0
        parent_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Parent Id
          description: The ID of the parent event.
        label:
          type: string
          title: Label
          description: Human readable label of the event type.
        source:
          type: string
          const: system
          title: Source
          description: >-
            The source of the event. `system` events are created by Polar.
            `user` events are the one you create through our ingestion API.
        name:
          type: string
          const: subscription.migrated
          title: Name
          description: The name of the event.
        metadata:
          $ref: '#/components/schemas/SubscriptionMigratedMetadata'
      type: object
      required:
        - id
        - timestamp
        - organization_id
        - customer_id
        - customer
        - external_customer_id
        - label
        - source
        - name
        - metadata
      title: SubscriptionMigratedEvent
      description: An event created by Polar when a subscription is migrated to Polar.
    SubscriptionUncanceledEvent:
      properties:
        id:
          type: string
          format: uuid4
          title: Id
          description: The ID of the object.
        timestamp:
          type: string
          format: date-time
          title: Timestamp
          description: The timestamp of the event.
          examples:
            - '2026-01-01T00:00:00.000000Z'
        organization_id:
          type: string
          format: uuid4
          title: Organization Id
          description: The ID of the organization owning the event.
          examples:
            - 1dbfc517-0bbf-4301-9ba8-555ca42b9737
        customer_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Customer Id
          description: >-
            ID of the customer in your Polar organization associated with the
            event.
        customer:
          anyOf:
            - $ref: '#/components/schemas/Customer'
            - type: 'null'
          description: The customer associated with the event.
        external_customer_id:
          anyOf:
            - type: string
            - type: 'null'
          title: External Customer Id
          description: ID of the customer in your system associated with the event.
        member_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Member Id
          description: >-
            ID of the member within the customer's organization who performed
            the action inside B2B.
        external_member_id:
          anyOf:
            - type: string
            - type: 'null'
          title: External Member Id
          description: >-
            ID of the member in your system within the customer's organization
            who performed the action inside B2B.
        child_count:
          type: integer
          title: Child Count
          description: Number of direct child events linked to this event.
          default: 0
        parent_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Parent Id
          description: The ID of the parent event.
        label:
          type: string
          title: Label
          description: Human readable label of the event type.
        source:
          type: string
          const: system
          title: Source
          description: >-
            The source of the event. `system` events are created by Polar.
            `user` events are the one you create through our ingestion API.
        name:
          type: string
          const: subscription.uncanceled
          title: Name
          description: The name of the event.
        metadata:
          $ref: '#/components/schemas/SubscriptionUncanceledMetadata'
      type: object
      required:
        - id
        - timestamp
        - organization_id
        - customer_id
        - customer
        - external_customer_id
        - label
        - source
        - name
        - metadata
      title: SubscriptionUncanceledEvent
      description: An event created by Polar when a subscription cancellation is reversed.
    SubscriptionProductUpdatedEvent:
      properties:
        id:
          type: string
          format: uuid4
          title: Id
          description: The ID of the object.
        timestamp:
          type: string
          format: date-time
          title: Timestamp
          description: The timestamp of the event.
          examples:
            - '2026-01-01T00:00:00.000000Z'
        organization_id:
          type: string
          format: uuid4
          title: Organization Id
          description: The ID of the organization owning the event.
          examples:
            - 1dbfc517-0bbf-4301-9ba8-555ca42b9737
        customer_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Customer Id
          description: >-
            ID of the customer in your Polar organization associated with the
            event.
        customer:
          anyOf:
            - $ref: '#/components/schemas/Customer'
            - type: 'null'
          description: The customer associated with the event.
        external_customer_id:
          anyOf:
            - type: string
            - type: 'null'
          title: External Customer Id
          description: ID of the customer in your system associated with the event.
        member_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Member Id
          description: >-
            ID of the member within the customer's organization who performed
            the action inside B2B.
        external_member_id:
          anyOf:
            - type: string
            - type: 'null'
          title: External Member Id
          description: >-
            ID of the member in your system within the customer's organization
            who performed the action inside B2B.
        child_count:
          type: integer
          title: Child Count
          description: Number of direct child events linked to this event.
          default: 0
        parent_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Parent Id
          description: The ID of the parent event.
        label:
          type: string
          title: Label
          description: Human readable label of the event type.
        source:
          type: string
          const: system
          title: Source
          description: >-
            The source of the event. `system` events are created by Polar.
            `user` events are the one you create through our ingestion API.
        name:
          type: string
          const: subscription.product_updated
          title: Name
          description: The name of the event.
        metadata:
          $ref: '#/components/schemas/SubscriptionProductUpdatedMetadata'
      type: object
      required:
        - id
        - timestamp
        - organization_id
        - customer_id
        - customer
        - external_customer_id
        - label
        - source
        - name
        - metadata
      title: SubscriptionProductUpdatedEvent
      description: An event created by Polar when a subscription changes the product.
    SubscriptionSeatsUpdatedEvent:
      properties:
        id:
          type: string
          format: uuid4
          title: Id
          description: The ID of the object.
        timestamp:
          type: string
          format: date-time
          title: Timestamp
          description: The timestamp of the event.
          examples:
            - '2026-01-01T00:00:00.000000Z'
        organization_id:
          type: string
          format: uuid4
          title: Organization Id
          description: The ID of the organization owning the event.
          examples:
            - 1dbfc517-0bbf-4301-9ba8-555ca42b9737
        customer_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Customer Id
          description: >-
            ID of the customer in your Polar organization associated with the
            event.
        customer:
          anyOf:
            - $ref: '#/components/schemas/Customer'
            - type: 'null'
          description: The customer associated with the event.
        external_customer_id:
          anyOf:
            - type: string
            - type: 'null'
          title: External Customer Id
          description: ID of the customer in your system associated with the event.
        member_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Member Id
          description: >-
            ID of the member within the customer's organization who performed
            the action inside B2B.
        external_member_id:
          anyOf:
            - type: string
            - type: 'null'
          title: External Member Id
          description: >-
            ID of the member in your system within the customer's organization
            who performed the action inside B2B.
        child_count:
          type: integer
          title: Child Count
          description: Number of direct child events linked to this event.
          default: 0
        parent_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Parent Id
          description: The ID of the parent event.
        label:
          type: string
          title: Label
          description: Human readable label of the event type.
        source:
          type: string
          const: system
          title: Source
          description: >-
            The source of the event. `system` events are created by Polar.
            `user` events are the one you create through our ingestion API.
        name:
          type: string
          const: subscription.seats_updated
          title: Name
          description: The name of the event.
        metadata:
          $ref: '#/components/schemas/SubscriptionSeatsUpdatedMetadata'
      type: object
      required:
        - id
        - timestamp
        - organization_id
        - customer_id
        - customer
        - external_customer_id
        - label
        - source
        - name
        - metadata
      title: SubscriptionSeatsUpdatedEvent
      description: An event created by Polar when a the seats on a subscription is changed.
    SubscriptionUnitsUpdatedEvent:
      properties:
        id:
          type: string
          format: uuid4
          title: Id
          description: The ID of the object.
        timestamp:
          type: string
          format: date-time
          title: Timestamp
          description: The timestamp of the event.
          examples:
            - '2026-01-01T00:00:00.000000Z'
        organization_id:
          type: string
          format: uuid4
          title: Organization Id
          description: The ID of the organization owning the event.
          examples:
            - 1dbfc517-0bbf-4301-9ba8-555ca42b9737
        customer_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Customer Id
          description: >-
            ID of the customer in your Polar organization associated with the
            event.
        customer:
          anyOf:
            - $ref: '#/components/schemas/Customer'
            - type: 'null'
          description: The customer associated with the event.
        external_customer_id:
          anyOf:
            - type: string
            - type: 'null'
          title: External Customer Id
          description: ID of the customer in your system associated with the event.
        member_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Member Id
          description: >-
            ID of the member within the customer's organization who performed
            the action inside B2B.
        external_member_id:
          anyOf:
            - type: string
            - type: 'null'
          title: External Member Id
          description: >-
            ID of the member in your system within the customer's organization
            who performed the action inside B2B.
        child_count:
          type: integer
          title: Child Count
          description: Number of direct child events linked to this event.
          default: 0
        parent_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Parent Id
          description: The ID of the parent event.
        label:
          type: string
          title: Label
          description: Human readable label of the event type.
        source:
          type: string
          const: system
          title: Source
          description: >-
            The source of the event. `system` events are created by Polar.
            `user` events are the one you create through our ingestion API.
        name:
          type: string
          const: subscription.units_updated
          title: Name
          description: The name of the event.
        metadata:
          $ref: '#/components/schemas/SubscriptionUnitsUpdatedMetadata'
      type: object
      required:
        - id
        - timestamp
        - organization_id
        - customer_id
        - customer
        - external_customer_id
        - label
        - source
        - name
        - metadata
      title: SubscriptionUnitsUpdatedEvent
      description: An event created by Polar when the units on a subscription are changed.
    SubscriptionBillingPeriodUpdatedEvent:
      properties:
        id:
          type: string
          format: uuid4
          title: Id
          description: The ID of the object.
        timestamp:
          type: string
          format: date-time
          title: Timestamp
          description: The timestamp of the event.
          examples:
            - '2026-01-01T00:00:00.000000Z'
        organization_id:
          type: string
          format: uuid4
          title: Organization Id
          description: The ID of the organization owning the event.
          examples:
            - 1dbfc517-0bbf-4301-9ba8-555ca42b9737
        customer_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Customer Id
          description: >-
            ID of the customer in your Polar organization associated with the
            event.
        customer:
          anyOf:
            - $ref: '#/components/schemas/Customer'
            - type: 'null'
          description: The customer associated with the event.
        external_customer_id:
          anyOf:
            - type: string
            - type: 'null'
          title: External Customer Id
          description: ID of the customer in your system associated with the event.
        member_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Member Id
          description: >-
            ID of the member within the customer's organization who performed
            the action inside B2B.
        external_member_id:
          anyOf:
            - type: string
            - type: 'null'
          title: External Member Id
          description: >-
            ID of the member in your system within the customer's organization
            who performed the action inside B2B.
        child_count:
          type: integer
          title: Child Count
          description: Number of direct child events linked to this event.
          default: 0
        parent_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Parent Id
          description: The ID of the parent event.
        label:
          type: string
          title: Label
          description: Human readable label of the event type.
        source:
          type: string
          const: system
          title: Source
          description: >-
            The source of the event. `system` events are created by Polar.
            `user` events are the one you create through our ingestion API.
        name:
          type: string
          const: subscription.billing_period_updated
          title: Name
          description: The name of the event.
        metadata:
          $ref: '#/components/schemas/SubscriptionBillingPeriodUpdatedMetadata'
      type: object
      required:
        - id
        - timestamp
        - organization_id
        - customer_id
        - customer
        - external_customer_id
        - label
        - source
        - name
        - metadata
      title: SubscriptionBillingPeriodUpdatedEvent
      description: An event created by Polar when a subscription billing period is updated.
    SubscriptionUpdateClearedEvent:
      properties:
        id:
          type: string
          format: uuid4
          title: Id
          description: The ID of the object.
        timestamp:
          type: string
          format: date-time
          title: Timestamp
          description: The timestamp of the event.
          examples:
            - '2026-01-01T00:00:00.000000Z'
        organization_id:
          type: string
          format: uuid4
          title: Organization Id
          description: The ID of the organization owning the event.
          examples:
            - 1dbfc517-0bbf-4301-9ba8-555ca42b9737
        customer_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Customer Id
          description: >-
            ID of the customer in your Polar organization associated with the
            event.
        customer:
          anyOf:
            - $ref: '#/components/schemas/Customer'
            - type: 'null'
          description: The customer associated with the event.
        external_customer_id:
          anyOf:
            - type: string
            - type: 'null'
          title: External Customer Id
          description: ID of the customer in your system associated with the event.
        member_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Member Id
          description: >-
            ID of the member within the customer's organization who performed
            the action inside B2B.
        external_member_id:
          anyOf:
            - type: string
            - type: 'null'
          title: External Member Id
          description: >-
            ID of the member in your system within the customer's organization
            who performed the action inside B2B.
        child_count:
          type: integer
          title: Child Count
          description: Number of direct child events linked to this event.
          default: 0
        parent_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Parent Id
          description: The ID of the parent event.
        label:
          type: string
          title: Label
          description: Human readable label of the event type.
        source:
          type: string
          const: system
          title: Source
          description: >-
            The source of the event. `system` events are created by Polar.
            `user` events are the one you create through our ingestion API.
        name:
          type: string
          const: subscription.update_cleared
          title: Name
          description: The name of the event.
        metadata:
          $ref: '#/components/schemas/SubscriptionUpdateClearedMetadata'
      type: object
      required:
        - id
        - timestamp
        - organization_id
        - customer_id
        - customer
        - external_customer_id
        - label
        - source
        - name
        - metadata
      title: SubscriptionUpdateClearedEvent
      description: >-
        An event created by Polar when a pending subscription update is cleared
        without being applied.
    OrderPaidEvent:
      properties:
        id:
          type: string
          format: uuid4
          title: Id
          description: The ID of the object.
        timestamp:
          type: string
          format: date-time
          title: Timestamp
          description: The timestamp of the event.
          examples:
            - '2026-01-01T00:00:00.000000Z'
        organization_id:
          type: string
          format: uuid4
          title: Organization Id
          description: The ID of the organization owning the event.
          examples:
            - 1dbfc517-0bbf-4301-9ba8-555ca42b9737
        customer_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Customer Id
          description: >-
            ID of the customer in your Polar organization associated with the
            event.
        customer:
          anyOf:
            - $ref: '#/components/schemas/Customer'
            - type: 'null'
          description: The customer associated with the event.
        external_customer_id:
          anyOf:
            - type: string
            - type: 'null'
          title: External Customer Id
          description: ID of the customer in your system associated with the event.
        member_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Member Id
          description: >-
            ID of the member within the customer's organization who performed
            the action inside B2B.
        external_member_id:
          anyOf:
            - type: string
            - type: 'null'
          title: External Member Id
          description: >-
            ID of the member in your system within the customer's organization
            who performed the action inside B2B.
        child_count:
          type: integer
          title: Child Count
          description: Number of direct child events linked to this event.
          default: 0
        parent_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Parent Id
          description: The ID of the parent event.
        label:
          type: string
          title: Label
          description: Human readable label of the event type.
        source:
          type: string
          const: system
          title: Source
          description: >-
            The source of the event. `system` events are created by Polar.
            `user` events are the one you create through our ingestion API.
        name:
          type: string
          const: order.paid
          title: Name
          description: The name of the event.
        metadata:
          $ref: '#/components/schemas/OrderPaidMetadata'
      type: object
      required:
        - id
        - timestamp
        - organization_id
        - customer_id
        - customer
        - external_customer_id
        - label
        - source
        - name
        - metadata
      title: OrderPaidEvent
      description: An event created by Polar when an order is paid.
    OrderRefundedEvent:
      properties:
        id:
          type: string
          format: uuid4
          title: Id
          description: The ID of the object.
        timestamp:
          type: string
          format: date-time
          title: Timestamp
          description: The timestamp of the event.
          examples:
            - '2026-01-01T00:00:00.000000Z'
        organization_id:
          type: string
          format: uuid4
          title: Organization Id
          description: The ID of the organization owning the event.
          examples:
            - 1dbfc517-0bbf-4301-9ba8-555ca42b9737
        customer_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Customer Id
          description: >-
            ID of the customer in your Polar organization associated with the
            event.
        customer:
          anyOf:
            - $ref: '#/components/schemas/Customer'
            - type: 'null'
          description: The customer associated with the event.
        external_customer_id:
          anyOf:
            - type: string
            - type: 'null'
          title: External Customer Id
          description: ID of the customer in your system associated with the event.
        member_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Member Id
          description: >-
            ID of the member within the customer's organization who performed
            the action inside B2B.
        external_member_id:
          anyOf:
            - type: string
            - type: 'null'
          title: External Member Id
          description: >-
            ID of the member in your system within the customer's organization
            who performed the action inside B2B.
        child_count:
          type: integer
          title: Child Count
          description: Number of direct child events linked to this event.
          default: 0
        parent_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Parent Id
          description: The ID of the parent event.
        label:
          type: string
          title: Label
          description: Human readable label of the event type.
        source:
          type: string
          const: system
          title: Source
          description: >-
            The source of the event. `system` events are created by Polar.
            `user` events are the one you create through our ingestion API.
        name:
          type: string
          const: order.refunded
          title: Name
          description: The name of the event.
        metadata:
          $ref: '#/components/schemas/OrderRefundedMetadata'
      type: object
      required:
        - id
        - timestamp
        - organization_id
        - customer_id
        - customer
        - external_customer_id
        - label
        - source
        - name
        - metadata
      title: OrderRefundedEvent
      description: An event created by Polar when an order is refunded.
    OrderVoidedEvent:
      properties:
        id:
          type: string
          format: uuid4
          title: Id
          description: The ID of the object.
        timestamp:
          type: string
          format: date-time
          title: Timestamp
          description: The timestamp of the event.
          examples:
            - '2026-01-01T00:00:00.000000Z'
        organization_id:
          type: string
          format: uuid4
          title: Organization Id
          description: The ID of the organization owning the event.
          examples:
            - 1dbfc517-0bbf-4301-9ba8-555ca42b9737
        customer_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Customer Id
          description: >-
            ID of the customer in your Polar organization associated with the
            event.
        customer:
          anyOf:
            - $ref: '#/components/schemas/Customer'
            - type: 'null'
          description: The customer associated with the event.
        external_customer_id:
          anyOf:
            - type: string
            - type: 'null'
          title: External Customer Id
          description: ID of the customer in your system associated with the event.
        member_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Member Id
          description: >-
            ID of the member within the customer's organization who performed
            the action inside B2B.
        external_member_id:
          anyOf:
            - type: string
            - type: 'null'
          title: External Member Id
          description: >-
            ID of the member in your system within the customer's organization
            who performed the action inside B2B.
        child_count:
          type: integer
          title: Child Count
          description: Number of direct child events linked to this event.
          default: 0
        parent_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Parent Id
          description: The ID of the parent event.
        label:
          type: string
          title: Label
          description: Human readable label of the event type.
        source:
          type: string
          const: system
          title: Source
          description: >-
            The source of the event. `system` events are created by Polar.
            `user` events are the one you create through our ingestion API.
        name:
          type: string
          const: order.voided
          title: Name
          description: The name of the event.
        metadata:
          $ref: '#/components/schemas/OrderVoidedMetadata'
      type: object
      required:
        - id
        - timestamp
        - organization_id
        - customer_id
        - customer
        - external_customer_id
        - label
        - source
        - name
        - metadata
      title: OrderVoidedEvent
      description: An event created by Polar when an order is voided.
    OrderUnvoidedEvent:
      properties:
        id:
          type: string
          format: uuid4
          title: Id
          description: The ID of the object.
        timestamp:
          type: string
          format: date-time
          title: Timestamp
          description: The timestamp of the event.
          examples:
            - '2026-01-01T00:00:00.000000Z'
        organization_id:
          type: string
          format: uuid4
          title: Organization Id
          description: The ID of the organization owning the event.
          examples:
            - 1dbfc517-0bbf-4301-9ba8-555ca42b9737
        customer_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Customer Id
          description: >-
            ID of the customer in your Polar organization associated with the
            event.
        customer:
          anyOf:
            - $ref: '#/components/schemas/Customer'
            - type: 'null'
          description: The customer associated with the event.
        external_customer_id:
          anyOf:
            - type: string
            - type: 'null'
          title: External Customer Id
          description: ID of the customer in your system associated with the event.
        member_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Member Id
          description: >-
            ID of the member within the customer's organization who performed
            the action inside B2B.
        external_member_id:
          anyOf:
            - type: string
            - type: 'null'
          title: External Member Id
          description: >-
            ID of the member in your system within the customer's organization
            who performed the action inside B2B.
        child_count:
          type: integer
          title: Child Count
          description: Number of direct child events linked to this event.
          default: 0
        parent_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Parent Id
          description: The ID of the parent event.
        label:
          type: string
          title: Label
          description: Human readable label of the event type.
        source:
          type: string
          const: system
          title: Source
          description: >-
            The source of the event. `system` events are created by Polar.
            `user` events are the one you create through our ingestion API.
        name:
          type: string
          const: order.unvoided
          title: Name
          description: The name of the event.
        metadata:
          $ref: '#/components/schemas/OrderUnvoidedMetadata'
      type: object
      required:
        - id
        - timestamp
        - organization_id
        - customer_id
        - customer
        - external_customer_id
        - label
        - source
        - name
        - metadata
      title: OrderUnvoidedEvent
      description: An event created by Polar when an order is unvoided.
    CheckoutCreatedEvent:
      properties:
        id:
          type: string
          format: uuid4
          title: Id
          description: The ID of the object.
        timestamp:
          type: string
          format: date-time
          title: Timestamp
          description: The timestamp of the event.
          examples:
            - '2026-01-01T00:00:00.000000Z'
        organization_id:
          type: string
          format: uuid4
          title: Organization Id
          description: The ID of the organization owning the event.
          examples:
            - 1dbfc517-0bbf-4301-9ba8-555ca42b9737
        customer_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Customer Id
          description: >-
            ID of the customer in your Polar organization associated with the
            event.
        customer:
          anyOf:
            - $ref: '#/components/schemas/Customer'
            - type: 'null'
          description: The customer associated with the event.
        external_customer_id:
          anyOf:
            - type: string
            - type: 'null'
          title: External Customer Id
          description: ID of the customer in your system associated with the event.
        member_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Member Id
          description: >-
            ID of the member within the customer's organization who performed
            the action inside B2B.
        external_member_id:
          anyOf:
            - type: string
            - type: 'null'
          title: External Member Id
          description: >-
            ID of the member in your system within the customer's organization
            who performed the action inside B2B.
        child_count:
          type: integer
          title: Child Count
          description: Number of direct child events linked to this event.
          default: 0
        parent_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Parent Id
          description: The ID of the parent event.
        label:
          type: string
          title: Label
          description: Human readable label of the event type.
        source:
          type: string
          const: system
          title: Source
          description: >-
            The source of the event. `system` events are created by Polar.
            `user` events are the one you create through our ingestion API.
        name:
          type: string
          const: checkout.created
          title: Name
          description: The name of the event.
        metadata:
          $ref: '#/components/schemas/CheckoutCreatedMetadata'
      type: object
      required:
        - id
        - timestamp
        - organization_id
        - customer_id
        - customer
        - external_customer_id
        - label
        - source
        - name
        - metadata
      title: CheckoutCreatedEvent
      description: An event created by Polar when a checkout is created.
    CustomerCreatedEvent:
      properties:
        id:
          type: string
          format: uuid4
          title: Id
          description: The ID of the object.
        timestamp:
          type: string
          format: date-time
          title: Timestamp
          description: The timestamp of the event.
          examples:
            - '2026-01-01T00:00:00.000000Z'
        organization_id:
          type: string
          format: uuid4
          title: Organization Id
          description: The ID of the organization owning the event.
          examples:
            - 1dbfc517-0bbf-4301-9ba8-555ca42b9737
        customer_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Customer Id
          description: >-
            ID of the customer in your Polar organization associated with the
            event.
        customer:
          anyOf:
            - $ref: '#/components/schemas/Customer'
            - type: 'null'
          description: The customer associated with the event.
        external_customer_id:
          anyOf:
            - type: string
            - type: 'null'
          title: External Customer Id
          description: ID of the customer in your system associated with the event.
        member_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Member Id
          description: >-
            ID of the member within the customer's organization who performed
            the action inside B2B.
        external_member_id:
          anyOf:
            - type: string
            - type: 'null'
          title: External Member Id
          description: >-
            ID of the member in your system within the customer's organization
            who performed the action inside B2B.
        child_count:
          type: integer
          title: Child Count
          description: Number of direct child events linked to this event.
          default: 0
        parent_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Parent Id
          description: The ID of the parent event.
        label:
          type: string
          title: Label
          description: Human readable label of the event type.
        source:
          type: string
          const: system
          title: Source
          description: >-
            The source of the event. `system` events are created by Polar.
            `user` events are the one you create through our ingestion API.
        name:
          type: string
          const: customer.created
          title: Name
          description: The name of the event.
        metadata:
          $ref: '#/components/schemas/CustomerCreatedMetadata'
      type: object
      required:
        - id
        - timestamp
        - organization_id
        - customer_id
        - customer
        - external_customer_id
        - label
        - source
        - name
        - metadata
      title: CustomerCreatedEvent
      description: An event created by Polar when a customer is created.
    CustomerUpdatedEvent:
      properties:
        id:
          type: string
          format: uuid4
          title: Id
          description: The ID of the object.
        timestamp:
          type: string
          format: date-time
          title: Timestamp
          description: The timestamp of the event.
          examples:
            - '2026-01-01T00:00:00.000000Z'
        organization_id:
          type: string
          format: uuid4
          title: Organization Id
          description: The ID of the organization owning the event.
          examples:
            - 1dbfc517-0bbf-4301-9ba8-555ca42b9737
        customer_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Customer Id
          description: >-
            ID of the customer in your Polar organization associated with the
            event.
        customer:
          anyOf:
            - $ref: '#/components/schemas/Customer'
            - type: 'null'
          description: The customer associated with the event.
        external_customer_id:
          anyOf:
            - type: string
            - type: 'null'
          title: External Customer Id
          description: ID of the customer in your system associated with the event.
        member_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Member Id
          description: >-
            ID of the member within the customer's organization who performed
            the action inside B2B.
        external_member_id:
          anyOf:
            - type: string
            - type: 'null'
          title: External Member Id
          description: >-
            ID of the member in your system within the customer's organization
            who performed the action inside B2B.
        child_count:
          type: integer
          title: Child Count
          description: Number of direct child events linked to this event.
          default: 0
        parent_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Parent Id
          description: The ID of the parent event.
        label:
          type: string
          title: Label
          description: Human readable label of the event type.
        source:
          type: string
          const: system
          title: Source
          description: >-
            The source of the event. `system` events are created by Polar.
            `user` events are the one you create through our ingestion API.
        name:
          type: string
          const: customer.updated
          title: Name
          description: The name of the event.
        metadata:
          $ref: '#/components/schemas/CustomerUpdatedMetadata'
      type: object
      required:
        - id
        - timestamp
        - organization_id
        - customer_id
        - customer
        - external_customer_id
        - label
        - source
        - name
        - metadata
      title: CustomerUpdatedEvent
      description: An event created by Polar when a customer is updated.
    CustomerDeletedEvent:
      properties:
        id:
          type: string
          format: uuid4
          title: Id
          description: The ID of the object.
        timestamp:
          type: string
          format: date-time
          title: Timestamp
          description: The timestamp of the event.
          examples:
            - '2026-01-01T00:00:00.000000Z'
        organization_id:
          type: string
          format: uuid4
          title: Organization Id
          description: The ID of the organization owning the event.
          examples:
            - 1dbfc517-0bbf-4301-9ba8-555ca42b9737
        customer_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Customer Id
          description: >-
            ID of the customer in your Polar organization associated with the
            event.
        customer:
          anyOf:
            - $ref: '#/components/schemas/Customer'
            - type: 'null'
          description: The customer associated with the event.
        external_customer_id:
          anyOf:
            - type: string
            - type: 'null'
          title: External Customer Id
          description: ID of the customer in your system associated with the event.
        member_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Member Id
          description: >-
            ID of the member within the customer's organization who performed
            the action inside B2B.
        external_member_id:
          anyOf:
            - type: string
            - type: 'null'
          title: External Member Id
          description: >-
            ID of the member in your system within the customer's organization
            who performed the action inside B2B.
        child_count:
          type: integer
          title: Child Count
          description: Number of direct child events linked to this event.
          default: 0
        parent_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Parent Id
          description: The ID of the parent event.
        label:
          type: string
          title: Label
          description: Human readable label of the event type.
        source:
          type: string
          const: system
          title: Source
          description: >-
            The source of the event. `system` events are created by Polar.
            `user` events are the one you create through our ingestion API.
        name:
          type: string
          const: customer.deleted
          title: Name
          description: The name of the event.
        metadata:
          $ref: '#/components/schemas/CustomerDeletedMetadata'
      type: object
      required:
        - id
        - timestamp
        - organization_id
        - customer_id
        - customer
        - external_customer_id
        - label
        - source
        - name
        - metadata
      title: CustomerDeletedEvent
      description: An event created by Polar when a customer is deleted.
    BalanceOrderEvent:
      properties:
        id:
          type: string
          format: uuid4
          title: Id
          description: The ID of the object.
        timestamp:
          type: string
          format: date-time
          title: Timestamp
          description: The timestamp of the event.
          examples:
            - '2026-01-01T00:00:00.000000Z'
        organization_id:
          type: string
          format: uuid4
          title: Organization Id
          description: The ID of the organization owning the event.
          examples:
            - 1dbfc517-0bbf-4301-9ba8-555ca42b9737
        customer_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Customer Id
          description: >-
            ID of the customer in your Polar organization associated with the
            event.
        customer:
          anyOf:
            - $ref: '#/components/schemas/Customer'
            - type: 'null'
          description: The customer associated with the event.
        external_customer_id:
          anyOf:
            - type: string
            - type: 'null'
          title: External Customer Id
          description: ID of the customer in your system associated with the event.
        member_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Member Id
          description: >-
            ID of the member within the customer's organization who performed
            the action inside B2B.
        external_member_id:
          anyOf:
            - type: string
            - type: 'null'
          title: External Member Id
          description: >-
            ID of the member in your system within the customer's organization
            who performed the action inside B2B.
        child_count:
          type: integer
          title: Child Count
          description: Number of direct child events linked to this event.
          default: 0
        parent_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Parent Id
          description: The ID of the parent event.
        label:
          type: string
          title: Label
          description: Human readable label of the event type.
        source:
          type: string
          const: system
          title: Source
          description: >-
            The source of the event. `system` events are created by Polar.
            `user` events are the one you create through our ingestion API.
        name:
          type: string
          const: balance.order
          title: Name
          description: The name of the event.
        metadata:
          $ref: '#/components/schemas/BalanceOrderMetadata'
      type: object
      required:
        - id
        - timestamp
        - organization_id
        - customer_id
        - customer
        - external_customer_id
        - label
        - source
        - name
        - metadata
      title: BalanceOrderEvent
      description: An event created by Polar when an order is paid.
    BalanceCreditOrderEvent:
      properties:
        id:
          type: string
          format: uuid4
          title: Id
          description: The ID of the object.
        timestamp:
          type: string
          format: date-time
          title: Timestamp
          description: The timestamp of the event.
          examples:
            - '2026-01-01T00:00:00.000000Z'
        organization_id:
          type: string
          format: uuid4
          title: Organization Id
          description: The ID of the organization owning the event.
          examples:
            - 1dbfc517-0bbf-4301-9ba8-555ca42b9737
        customer_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Customer Id
          description: >-
            ID of the customer in your Polar organization associated with the
            event.
        customer:
          anyOf:
            - $ref: '#/components/schemas/Customer'
            - type: 'null'
          description: The customer associated with the event.
        external_customer_id:
          anyOf:
            - type: string
            - type: 'null'
          title: External Customer Id
          description: ID of the customer in your system associated with the event.
        member_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Member Id
          description: >-
            ID of the member within the customer's organization who performed
            the action inside B2B.
        external_member_id:
          anyOf:
            - type: string
            - type: 'null'
          title: External Member Id
          description: >-
            ID of the member in your system within the customer's organization
            who performed the action inside B2B.
        child_count:
          type: integer
          title: Child Count
          description: Number of direct child events linked to this event.
          default: 0
        parent_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Parent Id
          description: The ID of the parent event.
        label:
          type: string
          title: Label
          description: Human readable label of the event type.
        source:
          type: string
          const: system
          title: Source
          description: >-
            The source of the event. `system` events are created by Polar.
            `user` events are the one you create through our ingestion API.
        name:
          type: string
          const: balance.credit_order
          title: Name
          description: The name of the event.
        metadata:
          $ref: '#/components/schemas/BalanceCreditOrderMetadata'
      type: object
      required:
        - id
        - timestamp
        - organization_id
        - customer_id
        - customer
        - external_customer_id
        - label
        - source
        - name
        - metadata
      title: BalanceCreditOrderEvent
      description: An event created by Polar when an order is paid via customer balance.
    BalanceRefundEvent:
      properties:
        id:
          type: string
          format: uuid4
          title: Id
          description: The ID of the object.
        timestamp:
          type: string
          format: date-time
          title: Timestamp
          description: The timestamp of the event.
          examples:
            - '2026-01-01T00:00:00.000000Z'
        organization_id:
          type: string
          format: uuid4
          title: Organization Id
          description: The ID of the organization owning the event.
          examples:
            - 1dbfc517-0bbf-4301-9ba8-555ca42b9737
        customer_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Customer Id
          description: >-
            ID of the customer in your Polar organization associated with the
            event.
        customer:
          anyOf:
            - $ref: '#/components/schemas/Customer'
            - type: 'null'
          description: The customer associated with the event.
        external_customer_id:
          anyOf:
            - type: string
            - type: 'null'
          title: External Customer Id
          description: ID of the customer in your system associated with the event.
        member_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Member Id
          description: >-
            ID of the member within the customer's organization who performed
            the action inside B2B.
        external_member_id:
          anyOf:
            - type: string
            - type: 'null'
          title: External Member Id
          description: >-
            ID of the member in your system within the customer's organization
            who performed the action inside B2B.
        child_count:
          type: integer
          title: Child Count
          description: Number of direct child events linked to this event.
          default: 0
        parent_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Parent Id
          description: The ID of the parent event.
        label:
          type: string
          title: Label
          description: Human readable label of the event type.
        source:
          type: string
          const: system
          title: Source
          description: >-
            The source of the event. `system` events are created by Polar.
            `user` events are the one you create through our ingestion API.
        name:
          type: string
          const: balance.refund
          title: Name
          description: The name of the event.
        metadata:
          $ref: '#/components/schemas/BalanceRefundMetadata'
      type: object
      required:
        - id
        - timestamp
        - organization_id
        - customer_id
        - customer
        - external_customer_id
        - label
        - source
        - name
        - metadata
      title: BalanceRefundEvent
      description: An event created by Polar when an order is refunded.
    BalanceRefundReversalEvent:
      properties:
        id:
          type: string
          format: uuid4
          title: Id
          description: The ID of the object.
        timestamp:
          type: string
          format: date-time
          title: Timestamp
          description: The timestamp of the event.
          examples:
            - '2026-01-01T00:00:00.000000Z'
        organization_id:
          type: string
          format: uuid4
          title: Organization Id
          description: The ID of the organization owning the event.
          examples:
            - 1dbfc517-0bbf-4301-9ba8-555ca42b9737
        customer_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Customer Id
          description: >-
            ID of the customer in your Polar organization associated with the
            event.
        customer:
          anyOf:
            - $ref: '#/components/schemas/Customer'
            - type: 'null'
          description: The customer associated with the event.
        external_customer_id:
          anyOf:
            - type: string
            - type: 'null'
          title: External Customer Id
          description: ID of the customer in your system associated with the event.
        member_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Member Id
          description: >-
            ID of the member within the customer's organization who performed
            the action inside B2B.
        external_member_id:
          anyOf:
            - type: string
            - type: 'null'
          title: External Member Id
          description: >-
            ID of the member in your system within the customer's organization
            who performed the action inside B2B.
        child_count:
          type: integer
          title: Child Count
          description: Number of direct child events linked to this event.
          default: 0
        parent_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Parent Id
          description: The ID of the parent event.
        label:
          type: string
          title: Label
          description: Human readable label of the event type.
        source:
          type: string
          const: system
          title: Source
          description: >-
            The source of the event. `system` events are created by Polar.
            `user` events are the one you create through our ingestion API.
        name:
          type: string
          const: balance.refund_reversal
          title: Name
          description: The name of the event.
        metadata:
          $ref: '#/components/schemas/BalanceRefundMetadata'
      type: object
      required:
        - id
        - timestamp
        - organization_id
        - customer_id
        - customer
        - external_customer_id
        - label
        - source
        - name
        - metadata
      title: BalanceRefundReversalEvent
      description: An event created by Polar when a refund is reverted.
    BalanceDisputeEvent:
      properties:
        id:
          type: string
          format: uuid4
          title: Id
          description: The ID of the object.
        timestamp:
          type: string
          format: date-time
          title: Timestamp
          description: The timestamp of the event.
          examples:
            - '2026-01-01T00:00:00.000000Z'
        organization_id:
          type: string
          format: uuid4
          title: Organization Id
          description: The ID of the organization owning the event.
          examples:
            - 1dbfc517-0bbf-4301-9ba8-555ca42b9737
        customer_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Customer Id
          description: >-
            ID of the customer in your Polar organization associated with the
            event.
        customer:
          anyOf:
            - $ref: '#/components/schemas/Customer'
            - type: 'null'
          description: The customer associated with the event.
        external_customer_id:
          anyOf:
            - type: string
            - type: 'null'
          title: External Customer Id
          description: ID of the customer in your system associated with the event.
        member_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Member Id
          description: >-
            ID of the member within the customer's organization who performed
            the action inside B2B.
        external_member_id:
          anyOf:
            - type: string
            - type: 'null'
          title: External Member Id
          description: >-
            ID of the member in your system within the customer's organization
            who performed the action inside B2B.
        child_count:
          type: integer
          title: Child Count
          description: Number of direct child events linked to this event.
          default: 0
        parent_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Parent Id
          description: The ID of the parent event.
        label:
          type: string
          title: Label
          description: Human readable label of the event type.
        source:
          type: string
          const: system
          title: Source
          description: >-
            The source of the event. `system` events are created by Polar.
            `user` events are the one you create through our ingestion API.
        name:
          type: string
          const: balance.dispute
          title: Name
          description: The name of the event.
        metadata:
          $ref: '#/components/schemas/BalanceDisputeMetadata'
      type: object
      required:
        - id
        - timestamp
        - organization_id
        - customer_id
        - customer
        - external_customer_id
        - label
        - source
        - name
        - metadata
      title: BalanceDisputeEvent
      description: An event created by Polar when an order is disputed.
    BalanceDisputeReversalEvent:
      properties:
        id:
          type: string
          format: uuid4
          title: Id
          description: The ID of the object.
        timestamp:
          type: string
          format: date-time
          title: Timestamp
          description: The timestamp of the event.
          examples:
            - '2026-01-01T00:00:00.000000Z'
        organization_id:
          type: string
          format: uuid4
          title: Organization Id
          description: The ID of the organization owning the event.
          examples:
            - 1dbfc517-0bbf-4301-9ba8-555ca42b9737
        customer_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Customer Id
          description: >-
            ID of the customer in your Polar organization associated with the
            event.
        customer:
          anyOf:
            - $ref: '#/components/schemas/Customer'
            - type: 'null'
          description: The customer associated with the event.
        external_customer_id:
          anyOf:
            - type: string
            - type: 'null'
          title: External Customer Id
          description: ID of the customer in your system associated with the event.
        member_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Member Id
          description: >-
            ID of the member within the customer's organization who performed
            the action inside B2B.
        external_member_id:
          anyOf:
            - type: string
            - type: 'null'
          title: External Member Id
          description: >-
            ID of the member in your system within the customer's organization
            who performed the action inside B2B.
        child_count:
          type: integer
          title: Child Count
          description: Number of direct child events linked to this event.
          default: 0
        parent_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Parent Id
          description: The ID of the parent event.
        label:
          type: string
          title: Label
          description: Human readable label of the event type.
        source:
          type: string
          const: system
          title: Source
          description: >-
            The source of the event. `system` events are created by Polar.
            `user` events are the one you create through our ingestion API.
        name:
          type: string
          const: balance.dispute_reversal
          title: Name
          description: The name of the event.
        metadata:
          $ref: '#/components/schemas/BalanceDisputeMetadata'
      type: object
      required:
        - id
        - timestamp
        - organization_id
        - customer_id
        - customer
        - external_customer_id
        - label
        - source
        - name
        - metadata
      title: BalanceDisputeReversalEvent
      description: >-
        An event created by Polar when a dispute is won and funds are
        reinstated.
    Customer:
      oneOf:
        - $ref: '#/components/schemas/CustomerIndividual'
        - $ref: '#/components/schemas/CustomerTeam'
      discriminator:
        propertyName: type
        mapping:
          individual:
            $ref: '#/components/schemas/CustomerIndividual'
          team:
            $ref: '#/components/schemas/CustomerTeam'
    EventMetadataOutput:
      properties:
        _cost:
          $ref: '#/components/schemas/CostMetadata-Output'
        _llm:
          $ref: '#/components/schemas/LLMMetadata'
      additionalProperties:
        anyOf:
          - type: string
          - type: integer
          - type: number
          - type: boolean
      type: object
      title: EventMetadataOutput
    MeterCreditedMetadata:
      properties:
        meter_id:
          type: string
          title: Meter Id
        units:
          type: integer
          title: Units
        rollover:
          type: boolean
          title: Rollover
      type: object
      required:
        - meter_id
        - units
        - rollover
      title: MeterCreditedMetadata
    MeterResetMetadata:
      properties:
        meter_id:
          type: string
          title: Meter Id
      type: object
      required:
        - meter_id
      title: MeterResetMetadata
    BenefitGrantMetadata:
      properties:
        benefit_id:
          type: string
          title: Benefit Id
        benefit_grant_id:
          type: string
          title: Benefit Grant Id
        benefit_type:
          $ref: '#/components/schemas/BenefitType'
        member_id:
          type: string
          title: Member Id
      type: object
      required:
        - benefit_id
        - benefit_grant_id
        - benefit_type
      title: BenefitGrantMetadata
    SubscriptionCreatedMetadata:
      properties:
        subscription_id:
          type: string
          title: Subscription Id
        product_id:
          type: string
          title: Product Id
        amount:
          type: integer
          title: Amount
        currency:
          type: string
          title: Currency
        recurring_interval:
          type: string
          title: Recurring Interval
        recurring_interval_count:
          type: integer
          title: Recurring Interval Count
        started_at:
          type: string
          title: Started At
      type: object
      required:
        - subscription_id
        - product_id
        - amount
        - currency
        - recurring_interval
        - recurring_interval_count
        - started_at
      title: SubscriptionCreatedMetadata
    SubscriptionUpdatedMetadata:
      properties:
        product_id:
          type: string
          title: Product Id
        proration_behavior:
          $ref: '#/components/schemas/SubscriptionProrationBehavior'
        discount_id:
          anyOf:
            - type: string
            - type: 'null'
          title: Discount Id
        trial_end:
          type: string
          title: Trial End
        seats:
          type: integer
          title: Seats
        units:
          type: integer
          title: Units
        billing_period_end:
          type: string
          title: Billing Period End
        subscription_id:
          type: string
          title: Subscription Id
      type: object
      required:
        - subscription_id
      title: SubscriptionUpdatedMetadata
    SubscriptionCycledMetadata:
      properties:
        subscription_id:
          type: string
          title: Subscription Id
        product_id:
          type: string
          title: Product Id
        amount:
          type: integer
          title: Amount
        currency:
          type: string
          title: Currency
        recurring_interval:
          type: string
          title: Recurring Interval
        recurring_interval_count:
          type: integer
          title: Recurring Interval Count
      type: object
      required:
        - subscription_id
      title: SubscriptionCycledMetadata
    SubscriptionCanceledMetadata:
      properties:
        subscription_id:
          type: string
          title: Subscription Id
        product_id:
          type: string
          title: Product Id
        amount:
          type: integer
          title: Amount
        currency:
          type: string
          title: Currency
        recurring_interval:
          type: string
          title: Recurring Interval
        recurring_interval_count:
          type: integer
          title: Recurring Interval Count
        customer_cancellation_reason:
          type: string
          title: Customer Cancellation Reason
        customer_cancellation_comment:
          type: string
          title: Customer Cancellation Comment
        canceled_at:
          type: string
          title: Canceled At
        ends_at:
          type: string
          title: Ends At
        cancel_at_period_end:
          type: boolean
          title: Cancel At Period End
      type: object
      required:
        - subscription_id
        - amount
        - currency
        - recurring_interval
        - recurring_interval_count
        - canceled_at
      title: SubscriptionCanceledMetadata
    SubscriptionRevokedMetadata:
      properties:
        subscription_id:
          type: string
          title: Subscription Id
        product_id:
          type: string
          title: Product Id
        amount:
          type: integer
          title: Amount
        currency:
          type: string
          title: Currency
        recurring_interval:
          type: string
          title: Recurring Interval
        recurring_interval_count:
          type: integer
          title: Recurring Interval Count
      type: object
      required:
        - subscription_id
      title: SubscriptionRevokedMetadata
    SubscriptionPastDueMetadata:
      properties:
        subscription_id:
          type: string
          title: Subscription Id
        product_id:
          type: string
          title: Product Id
        past_due_at:
          type: string
          title: Past Due At
        amount:
          type: integer
          title: Amount
        currency:
          type: string
          title: Currency
        recurring_interval:
          type: string
          title: Recurring Interval
        recurring_interval_count:
          type: integer
          title: Recurring Interval Count
      type: object
      required:
        - subscription_id
        - past_due_at
      title: SubscriptionPastDueMetadata
    SubscriptionReactivatedMetadata:
      properties:
        subscription_id:
          type: string
          title: Subscription Id
        product_id:
          type: string
          title: Product Id
        amount:
          type: integer
          title: Amount
        currency:
          type: string
          title: Currency
        recurring_interval:
          type: string
          title: Recurring Interval
        recurring_interval_count:
          type: integer
          title: Recurring Interval Count
      type: object
      required:
        - subscription_id
      title: SubscriptionReactivatedMetadata
    SubscriptionReinstatedMetadata:
      properties:
        subscription_id:
          type: string
          title: Subscription Id
        product_id:
          type: string
          title: Product Id
        amount:
          type: integer
          title: Amount
        currency:
          type: string
          title: Currency
        recurring_interval:
          type: string
          title: Recurring Interval
        recurring_interval_count:
          type: integer
          title: Recurring Interval Count
      type: object
      required:
        - subscription_id
      title: SubscriptionReinstatedMetadata
    SubscriptionPausedMetadata:
      properties:
        subscription_id:
          type: string
          title: Subscription Id
        product_id:
          type: string
          title: Product Id
        amount:
          type: integer
          title: Amount
        currency:
          type: string
          title: Currency
        recurring_interval:
          type: string
          title: Recurring Interval
        recurring_interval_count:
          type: integer
          title: Recurring Interval Count
        paused_at:
          type: string
          title: Paused At
        resumes_at:
          type: string
          title: Resumes At
      type: object
      required:
        - subscription_id
        - paused_at
      title: SubscriptionPausedMetadata
    SubscriptionResumedMetadata:
      properties:
        subscription_id:
          type: string
          title: Subscription Id
        product_id:
          type: string
          title: Product Id
        amount:
          type: integer
          title: Amount
        currency:
          type: string
          title: Currency
        recurring_interval:
          type: string
          title: Recurring Interval
        recurring_interval_count:
          type: integer
          title: Recurring Interval Count
      type: object
      required:
        - subscription_id
      title: SubscriptionResumedMetadata
    SubscriptionMigratedMetadata:
      properties:
        subscription_id:
          type: string
          title: Subscription Id
        provider:
          type: string
          title: Provider
        provider_subscription_id:
          type: string
          title: Provider Subscription Id
        product_id:
          type: string
          title: Product Id
      type: object
      required:
        - subscription_id
        - provider
        - provider_subscription_id
        - product_id
      title: SubscriptionMigratedMetadata
    SubscriptionUncanceledMetadata:
      properties:
        subscription_id:
          type: string
          title: Subscription Id
        product_id:
          type: string
          title: Product Id
        amount:
          type: integer
          title: Amount
        currency:
          type: string
          title: Currency
        recurring_interval:
          type: string
          title: Recurring Interval
        recurring_interval_count:
          type: integer
          title: Recurring Interval Count
      type: object
      required:
        - subscription_id
        - product_id
        - amount
        - currency
        - recurring_interval
        - recurring_interval_count
      title: SubscriptionUncanceledMetadata
    SubscriptionProductUpdatedMetadata:
      properties:
        subscription_id:
          type: string
          title: Subscription Id
        old_product_id:
          type: string
          title: Old Product Id
        new_product_id:
          type: string
          title: New Product Id
      type: object
      required:
        - subscription_id
        - old_product_id
        - new_product_id
      title: SubscriptionProductUpdatedMetadata
    SubscriptionSeatsUpdatedMetadata:
      properties:
        subscription_id:
          type: string
          title: Subscription Id
        old_seats:
          type: integer
          title: Old Seats
        new_seats:
          type: integer
          title: New Seats
        proration_behavior:
          type: string
          title: Proration Behavior
      type: object
      required:
        - subscription_id
        - old_seats
        - new_seats
        - proration_behavior
      title: SubscriptionSeatsUpdatedMetadata
    SubscriptionUnitsUpdatedMetadata:
      properties:
        subscription_id:
          type: string
          title: Subscription Id
        old_units:
          type: integer
          title: Old Units
        new_units:
          type: integer
          title: New Units
        proration_behavior:
          type: string
          title: Proration Behavior
      type: object
      required:
        - subscription_id
        - old_units
        - new_units
        - proration_behavior
      title: SubscriptionUnitsUpdatedMetadata
    SubscriptionBillingPeriodUpdatedMetadata:
      properties:
        subscription_id:
          type: string
          title: Subscription Id
        old_period_end:
          type: string
          title: Old Period End
        new_period_end:
          type: string
          title: New Period End
      type: object
      required:
        - subscription_id
        - old_period_end
        - new_period_end
      title: SubscriptionBillingPeriodUpdatedMetadata
    SubscriptionUpdateClearedMetadata:
      properties:
        subscription_id:
          type: string
          title: Subscription Id
      type: object
      required:
        - subscription_id
      title: SubscriptionUpdateClearedMetadata
    OrderPaidMetadata:
      properties:
        order_id:
          type: string
          title: Order Id
        product_id:
          type: string
          title: Product Id
        billing_type:
          type: string
          title: Billing Type
        amount:
          type: integer
          title: Amount
        currency:
          type: string
          title: Currency
        net_amount:
          type: integer
          title: Net Amount
        tax_amount:
          type: integer
          title: Tax Amount
        applied_balance_amount:
          type: integer
          title: Applied Balance Amount
        discount_amount:
          type: integer
          title: Discount Amount
        discount_id:
          type: string
          title: Discount Id
        platform_fee:
          type: integer
          title: Platform Fee
        subscription_id:
          type: string
          title: Subscription Id
        recurring_interval:
          type: string
          title: Recurring Interval
        recurring_interval_count:
          type: integer
          title: Recurring Interval Count
      type: object
      required:
        - order_id
        - amount
      title: OrderPaidMetadata
    OrderRefundedMetadata:
      properties:
        order_id:
          type: string
          title: Order Id
        refunded_amount:
          type: integer
          title: Refunded Amount
        currency:
          type: string
          title: Currency
      type: object
      required:
        - order_id
        - refunded_amount
        - currency
      title: OrderRefundedMetadata
    OrderVoidedMetadata:
      properties:
        order_id:
          type: string
          title: Order Id
        amount:
          type: integer
          title: Amount
        currency:
          type: string
          title: Currency
      type: object
      required:
        - order_id
        - amount
        - currency
      title: OrderVoidedMetadata
    OrderUnvoidedMetadata:
      properties:
        order_id:
          type: string
          title: Order Id
        amount:
          type: integer
          title: Amount
        currency:
          type: string
          title: Currency
      type: object
      required:
        - order_id
        - amount
        - currency
      title: OrderUnvoidedMetadata
    CheckoutCreatedMetadata:
      properties:
        checkout_id:
          type: string
          title: Checkout Id
        checkout_status:
          type: string
          title: Checkout Status
        product_id:
          type: string
          title: Product Id
      type: object
      required:
        - checkout_id
        - checkout_status
      title: CheckoutCreatedMetadata
    CustomerCreatedMetadata:
      properties:
        customer_id:
          type: string
          title: Customer Id
        customer_email:
          anyOf:
            - type: string
            - type: 'null'
          title: Customer Email
        customer_name:
          anyOf:
            - type: string
            - type: 'null'
          title: Customer Name
        customer_external_id:
          anyOf:
            - type: string
            - type: 'null'
          title: Customer External Id
      type: object
      required:
        - customer_id
        - customer_email
        - customer_name
        - customer_external_id
      title: CustomerCreatedMetadata
    CustomerUpdatedMetadata:
      properties:
        customer_id:
          type: string
          title: Customer Id
        customer_email:
          anyOf:
            - type: string
            - type: 'null'
          title: Customer Email
        customer_name:
          anyOf:
            - type: string
            - type: 'null'
          title: Customer Name
        customer_external_id:
          anyOf:
            - type: string
            - type: 'null'
          title: Customer External Id
        updated_fields:
          $ref: '#/components/schemas/CustomerUpdatedFields'
      type: object
      required:
        - customer_id
        - customer_email
        - customer_name
        - customer_external_id
        - updated_fields
      title: CustomerUpdatedMetadata
    CustomerDeletedMetadata:
      properties:
        customer_id:
          type: string
          title: Customer Id
        customer_email:
          anyOf:
            - type: string
            - type: 'null'
          title: Customer Email
        customer_name:
          anyOf:
            - type: string
            - type: 'null'
          title: Customer Name
        customer_external_id:
          anyOf:
            - type: string
            - type: 'null'
          title: Customer External Id
      type: object
      required:
        - customer_id
        - customer_email
        - customer_name
        - customer_external_id
      title: CustomerDeletedMetadata
    BalanceOrderMetadata:
      properties:
        transaction_id:
          type: string
          title: Transaction Id
        order_id:
          type: string
          title: Order Id
        product_id:
          type: string
          title: Product Id
        subscription_id:
          type: string
          title: Subscription Id
        amount:
          type: integer
          title: Amount
        net_amount:
          type: integer
          title: Net Amount
        currency:
          type: string
          title: Currency
        presentment_amount:
          type: integer
          title: Presentment Amount
        presentment_currency:
          type: string
          title: Presentment Currency
        tax_amount:
          type: integer
          title: Tax Amount
        tax_state:
          anyOf:
            - type: string
            - type: 'null'
          title: Tax State
        tax_country:
          anyOf:
            - type: string
            - type: 'null'
          title: Tax Country
        fee:
          type: integer
          title: Fee
        exchange_rate:
          type: number
          title: Exchange Rate
      type: object
      required:
        - transaction_id
        - order_id
        - amount
        - currency
        - presentment_amount
        - presentment_currency
        - tax_amount
        - fee
      title: BalanceOrderMetadata
    BalanceCreditOrderMetadata:
      properties:
        order_id:
          type: string
          title: Order Id
        product_id:
          type: string
          title: Product Id
        subscription_id:
          type: string
          title: Subscription Id
        amount:
          type: integer
          title: Amount
        currency:
          type: string
          title: Currency
        tax_amount:
          type: integer
          title: Tax Amount
        tax_state:
          anyOf:
            - type: string
            - type: 'null'
          title: Tax State
        tax_country:
          anyOf:
            - type: string
            - type: 'null'
          title: Tax Country
        fee:
          type: integer
          title: Fee
        exchange_rate:
          type: number
          title: Exchange Rate
      type: object
      required:
        - order_id
        - amount
        - currency
        - tax_amount
        - fee
      title: BalanceCreditOrderMetadata
    BalanceRefundMetadata:
      properties:
        transaction_id:
          type: string
          title: Transaction Id
        refund_id:
          type: string
          title: Refund Id
        order_id:
          type: string
          title: Order Id
        order_created_at:
          type: string
          title: Order Created At
        product_id:
          type: string
          title: Product Id
        subscription_id:
          type: string
          title: Subscription Id
        amount:
          type: integer
          title: Amount
        currency:
          type: string
          title: Currency
        presentment_amount:
          type: integer
          title: Presentment Amount
        presentment_currency:
          type: string
          title: Presentment Currency
        refundable_amount:
          type: integer
          title: Refundable Amount
        tax_amount:
          type: integer
          title: Tax Amount
        tax_state:
          anyOf:
            - type: string
            - type: 'null'
          title: Tax State
        tax_country:
          anyOf:
            - type: string
            - type: 'null'
          title: Tax Country
        fee:
          type: integer
          title: Fee
        exchange_rate:
          type: number
          title: Exchange Rate
      type: object
      required:
        - transaction_id
        - refund_id
        - amount
        - currency
        - presentment_amount
        - presentment_currency
        - tax_amount
        - fee
      title: BalanceRefundMetadata
    BalanceDisputeMetadata:
      properties:
        transaction_id:
          type: string
          title: Transaction Id
        dispute_id:
          type: string
          title: Dispute Id
        order_id:
          type: string
          title: Order Id
        order_created_at:
          type: string
          title: Order Created At
        product_id:
          type: string
          title: Product Id
        subscription_id:
          type: string
          title: Subscription Id
        amount:
          type: integer
          title: Amount
        currency:
          type: string
          title: Currency
        presentment_amount:
          type: integer
          title: Presentment Amount
        presentment_currency:
          type: string
          title: Presentment Currency
        tax_amount:
          type: integer
          title: Tax Amount
        tax_state:
          anyOf:
            - type: string
            - type: 'null'
          title: Tax State
        tax_country:
          anyOf:
            - type: string
            - type: 'null'
          title: Tax Country
        fee:
          type: integer
          title: Fee
        exchange_rate:
          type: number
          title: Exchange Rate
      type: object
      required:
        - transaction_id
        - dispute_id
        - amount
        - currency
        - presentment_amount
        - presentment_currency
        - tax_amount
        - fee
      title: BalanceDisputeMetadata
    CustomerIndividual:
      properties:
        id:
          type: string
          format: uuid4
          title: Id
          description: The ID of the customer.
          examples:
            - 992fae2a-2a17-4b7a-8d9e-e287cf90131b
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
        metadata:
          $ref: '#/components/schemas/MetadataOutputType'
        external_id:
          anyOf:
            - type: string
            - type: 'null'
          title: External Id
          description: >-
            The ID of the customer in your system. This must be unique within
            the organization. Once set, it can't be updated.
          examples:
            - usr_1337
        email:
          type: string
          title: Email
          description: >-
            The email address of the customer. This must be unique within the
            organization.
          examples:
            - customer@example.com
        email_verified:
          type: boolean
          title: Email Verified
          description: >-
            Whether the customer email address is verified. The address is
            automatically verified when the customer accesses the customer
            portal using their email address.
          examples:
            - true
        type:
          type: string
          const: individual
          title: Type
          description: The type of customer.
          examples:
            - individual
        name:
          anyOf:
            - type: string
            - type: 'null'
          title: Name
          description: The name of the customer.
          examples:
            - John Doe
        billing_name:
          anyOf:
            - type: string
            - type: 'null'
          title: Billing Name
          description: >-
            The name that should appear on the customer's invoices. Falls back
            to the customer name when not explicitly set.
          examples:
            - John Doe
        billing_address:
          anyOf:
            - $ref: '#/components/schemas/Address'
            - type: 'null'
        tax_id:
          anyOf:
            - prefixItems:
                - type: string
                - $ref: '#/components/schemas/TaxIDFormat'
              type: array
              maxItems: 2
              minItems: 2
              examples:
                - - '911144442'
                  - us_ein
                - - FR61954506077
                  - eu_vat
            - type: 'null'
          title: Tax Id
        locale:
          anyOf:
            - type: string
            - type: 'null'
          title: Locale
        organization_id:
          type: string
          format: uuid4
          title: Organization Id
          description: The ID of the organization owning the customer.
          examples:
            - 1dbfc517-0bbf-4301-9ba8-555ca42b9737
        default_payment_method_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Default Payment Method Id
          description: >-
            The ID of the customer's default payment method, if any. Use the
            payment methods endpoint to retrieve its details.
        deleted_at:
          anyOf:
            - type: string
              format: date-time
              examples:
                - '2026-01-01T00:00:00.000000Z'
            - type: 'null'
          title: Deleted At
          description: Timestamp for when the customer was soft deleted.
        first_user_event_at:
          anyOf:
            - type: string
              format: date-time
              examples:
                - '2026-01-01T00:00:00.000000Z'
            - type: 'null'
          title: First User Event At
          description: >-
            Timestamp of the first event ingested for this customer. Can predate
            `created_at`, and is null if no event was ever ingested.
        avatar_url:
          anyOf:
            - type: string
            - type: 'null'
          title: Avatar Url
          examples:
            - https://www.gravatar.com/avatar/xxx?d=404
      type: object
      required:
        - id
        - created_at
        - modified_at
        - metadata
        - email
        - email_verified
        - type
        - name
        - billing_name
        - billing_address
        - tax_id
        - organization_id
        - deleted_at
        - first_user_event_at
        - avatar_url
      title: CustomerIndividual
      description: A customer in an organization.
    CustomerTeam:
      properties:
        id:
          type: string
          format: uuid4
          title: Id
          description: The ID of the customer.
          examples:
            - 992fae2a-2a17-4b7a-8d9e-e287cf90131b
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
        metadata:
          $ref: '#/components/schemas/MetadataOutputType'
        external_id:
          anyOf:
            - type: string
            - type: 'null'
          title: External Id
          description: >-
            The ID of the customer in your system. This must be unique within
            the organization. Once set, it can't be updated.
          examples:
            - usr_1337
        email:
          anyOf:
            - type: string
            - type: 'null'
          title: Email
          description: >-
            The email address of the customer. This must be unique within the
            organization.
          examples:
            - customer@example.com
        email_verified:
          type: boolean
          title: Email Verified
          description: >-
            Whether the customer email address is verified. The address is
            automatically verified when the customer accesses the customer
            portal using their email address.
          examples:
            - true
        type:
          type: string
          const: team
          title: Type
          description: The type of customer. Team customers can have multiple members.
          examples:
            - team
        name:
          anyOf:
            - type: string
            - type: 'null'
          title: Name
          description: The name of the customer.
          examples:
            - John Doe
        billing_name:
          anyOf:
            - type: string
            - type: 'null'
          title: Billing Name
          description: >-
            The name that should appear on the customer's invoices. Falls back
            to the customer name when not explicitly set.
          examples:
            - John Doe
        billing_address:
          anyOf:
            - $ref: '#/components/schemas/Address'
            - type: 'null'
        tax_id:
          anyOf:
            - prefixItems:
                - type: string
                - $ref: '#/components/schemas/TaxIDFormat'
              type: array
              maxItems: 2
              minItems: 2
              examples:
                - - '911144442'
                  - us_ein
                - - FR61954506077
                  - eu_vat
            - type: 'null'
          title: Tax Id
        locale:
          anyOf:
            - type: string
            - type: 'null'
          title: Locale
        organization_id:
          type: string
          format: uuid4
          title: Organization Id
          description: The ID of the organization owning the customer.
          examples:
            - 1dbfc517-0bbf-4301-9ba8-555ca42b9737
        default_payment_method_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Default Payment Method Id
          description: >-
            The ID of the customer's default payment method, if any. Use the
            payment methods endpoint to retrieve its details.
        deleted_at:
          anyOf:
            - type: string
              format: date-time
              examples:
                - '2026-01-01T00:00:00.000000Z'
            - type: 'null'
          title: Deleted At
          description: Timestamp for when the customer was soft deleted.
        first_user_event_at:
          anyOf:
            - type: string
              format: date-time
              examples:
                - '2026-01-01T00:00:00.000000Z'
            - type: 'null'
          title: First User Event At
          description: >-
            Timestamp of the first event ingested for this customer. Can predate
            `created_at`, and is null if no event was ever ingested.
        avatar_url:
          anyOf:
            - type: string
            - type: 'null'
          title: Avatar Url
          examples:
            - https://www.gravatar.com/avatar/xxx?d=404
      type: object
      required:
        - id
        - created_at
        - modified_at
        - metadata
        - email_verified
        - type
        - name
        - billing_name
        - billing_address
        - tax_id
        - organization_id
        - deleted_at
        - first_user_event_at
        - avatar_url
      title: CustomerTeam
      description: A team customer in an organization.
    CostMetadata-Output:
      properties:
        amount:
          type: string
          pattern: >-
            ^(?!^[-+.]*$)[+-]?0*(?:\d{0,5}|(?=[\d.]{1,18}0*$)\d{0,5}\.\d{0,12}0*$)
          title: Amount
          description: The amount in cents.
        currency:
          type: string
          pattern: usd
          title: Currency
          description: The currency. Currently, only `usd` is supported.
      type: object
      required:
        - amount
        - currency
      title: CostMetadata
    LLMMetadata:
      properties:
        vendor:
          type: string
          title: Vendor
          description: The vendor of the event.
        model:
          type: string
          title: Model
          description: The model used for the event.
        prompt:
          anyOf:
            - type: string
            - type: 'null'
          title: Prompt
          description: The LLM prompt used for the event.
        response:
          anyOf:
            - type: string
            - type: 'null'
          title: Response
          description: The LLM response used for the event.
        input_tokens:
          type: integer
          title: Input Tokens
          description: The number of LLM input tokens used for the event.
        cached_input_tokens:
          type: integer
          title: Cached Input Tokens
          description: The number of LLM cached tokens that were used for the event.
        output_tokens:
          type: integer
          title: Output Tokens
          description: The number of LLM output tokens used for the event.
        total_tokens:
          type: integer
          title: Total Tokens
          description: The total number of LLM tokens used for the event.
      type: object
      required:
        - vendor
        - model
        - input_tokens
        - output_tokens
        - total_tokens
      title: LLMMetadata
    BenefitType:
      type: string
      enum:
        - custom
        - discord
        - github_repository
        - downloadables
        - license_keys
        - meter_credit
        - feature_flag
        - slack_shared_channel
      title: BenefitType
    SubscriptionProrationBehavior:
      type: string
      enum:
        - invoice
        - prorate
        - next_period
        - reset
      title: SubscriptionProrationBehavior
    CustomerUpdatedFields:
      properties:
        name:
          anyOf:
            - type: string
            - type: 'null'
          title: Name
        billing_name:
          anyOf:
            - type: string
            - type: 'null'
          title: Billing Name
        email:
          anyOf:
            - type: string
            - type: 'null'
          title: Email
        billing_address:
          anyOf:
            - $ref: '#/components/schemas/AddressDict'
            - type: 'null'
        tax_id:
          anyOf:
            - type: string
            - type: 'null'
          title: Tax Id
        metadata:
          anyOf:
            - additionalProperties:
                anyOf:
                  - type: string
                  - type: integer
                  - type: boolean
              type: object
            - type: 'null'
          title: Metadata
      type: object
      title: CustomerUpdatedFields
    MetadataOutputType:
      additionalProperties:
        anyOf:
          - type: string
          - type: integer
          - type: number
          - type: boolean
      type: object
    Address:
      properties:
        line1:
          anyOf:
            - type: string
            - type: 'null'
          title: Line1
        line2:
          anyOf:
            - type: string
            - type: 'null'
          title: Line2
        postal_code:
          anyOf:
            - type: string
            - type: 'null'
          title: Postal Code
        city:
          anyOf:
            - type: string
            - type: 'null'
          title: City
        state:
          anyOf:
            - type: string
            - type: 'null'
          title: State
        country:
          type: string
          enum:
            - AD
            - AE
            - AF
            - AG
            - AI
            - AL
            - AM
            - AO
            - AQ
            - AR
            - AS
            - AT
            - AU
            - AW
            - AX
            - AZ
            - BA
            - BB
            - BD
            - BE
            - BF
            - BG
            - BH
            - BI
            - BJ
            - BL
            - BM
            - BN
            - BO
            - BQ
            - BR
            - BS
            - BT
            - BV
            - BW
            - BY
            - BZ
            - CA
            - CC
            - CD
            - CF
            - CG
            - CH
            - CI
            - CK
            - CL
            - CM
            - CN
            - CO
            - CR
            - CU
            - CV
            - CW
            - CX
            - CY
            - CZ
            - DE
            - DJ
            - DK
            - DM
            - DO
            - DZ
            - EC
            - EE
            - EG
            - EH
            - ER
            - ES
            - ET
            - FI
            - FJ
            - FK
            - FM
            - FO
            - FR
            - GA
            - GB
            - GD
            - GE
            - GF
            - GG
            - GH
            - GI
            - GL
            - GM
            - GN
            - GP
            - GQ
            - GR
            - GS
            - GT
            - GU
            - GW
            - GY
            - HK
            - HM
            - HN
            - HR
            - HT
            - HU
            - ID
            - IE
            - IL
            - IM
            - IN
            - IO
            - IQ
            - IR
            - IS
            - IT
            - JE
            - JM
            - JO
            - JP
            - KE
            - KG
            - KH
            - KI
            - KM
            - KN
            - KP
            - KR
            - KW
            - KY
            - KZ
            - LA
            - LB
            - LC
            - LI
            - LK
            - LR
            - LS
            - LT
            - LU
            - LV
            - LY
            - MA
            - MC
            - MD
            - ME
            - MF
            - MG
            - MH
            - MK
            - ML
            - MM
            - MN
            - MO
            - MP
            - MQ
            - MR
            - MS
            - MT
            - MU
            - MV
            - MW
            - MX
            - MY
            - MZ
            - NA
            - NC
            - NE
            - NF
            - NG
            - NI
            - NL
            - 'NO'
            - NP
            - NR
            - NU
            - NZ
            - OM
            - PA
            - PE
            - PF
            - PG
            - PH
            - PK
            - PL
            - PM
            - PN
            - PR
            - PS
            - PT
            - PW
            - PY
            - QA
            - RE
            - RO
            - RS
            - RU
            - RW
            - SA
            - SB
            - SC
            - SD
            - SE
            - SG
            - SH
            - SI
            - SJ
            - SK
            - SL
            - SM
            - SN
            - SO
            - SR
            - SS
            - ST
            - SV
            - SX
            - SY
            - SZ
            - TC
            - TD
            - TF
            - TG
            - TH
            - TJ
            - TK
            - TL
            - TM
            - TN
            - TO
            - TR
            - TT
            - TV
            - TW
            - TZ
            - UA
            - UG
            - UM
            - US
            - UY
            - UZ
            - VA
            - VC
            - VE
            - VG
            - VI
            - VN
            - VU
            - WF
            - WS
            - YE
            - YT
            - ZA
            - ZM
            - ZW
          title: CountryAlpha2
          examples:
            - US
            - SE
            - FR
          x-speakeasy-enums:
            - AD
            - AE
            - AF
            - AG
            - AI
            - AL
            - AM
            - AO
            - AQ
            - AR
            - AS
            - AT
            - AU
            - AW
            - AX
            - AZ
            - BA
            - BB
            - BD
            - BE
            - BF
            - BG
            - BH
            - BI
            - BJ
            - BL
            - BM
            - BN
            - BO
            - BQ
            - BR
            - BS
            - BT
            - BV
            - BW
            - BY
            - BZ
            - CA
            - CC
            - CD
            - CF
            - CG
            - CH
            - CI
            - CK
            - CL
            - CM
            - CN
            - CO
            - CR
            - CU
            - CV
            - CW
            - CX
            - CY
            - CZ
            - DE
            - DJ
            - DK
            - DM
            - DO
            - DZ
            - EC
            - EE
            - EG
            - EH
            - ER
            - ES
            - ET
            - FI
            - FJ
            - FK
            - FM
            - FO
            - FR
            - GA
            - GB
            - GD
            - GE
            - GF
            - GG
            - GH
            - GI
            - GL
            - GM
            - GN
            - GP
            - GQ
            - GR
            - GS
            - GT
            - GU
            - GW
            - GY
            - HK
            - HM
            - HN
            - HR
            - HT
            - HU
            - ID
            - IE
            - IL
            - IM
            - IN
            - IO
            - IQ
            - IR
            - IS
            - IT
            - JE
            - JM
            - JO
            - JP
            - KE
            - KG
            - KH
            - KI
            - KM
            - KN
            - KP
            - KR
            - KW
            - KY
            - KZ
            - LA
            - LB
            - LC
            - LI
            - LK
            - LR
            - LS
            - LT
            - LU
            - LV
            - LY
            - MA
            - MC
            - MD
            - ME
            - MF
            - MG
            - MH
            - MK
            - ML
            - MM
            - MN
            - MO
            - MP
            - MQ
            - MR
            - MS
            - MT
            - MU
            - MV
            - MW
            - MX
            - MY
            - MZ
            - NA
            - NC
            - NE
            - NF
            - NG
            - NI
            - NL
            - 'NO'
            - NP
            - NR
            - NU
            - NZ
            - OM
            - PA
            - PE
            - PF
            - PG
            - PH
            - PK
            - PL
            - PM
            - PN
            - PR
            - PS
            - PT
            - PW
            - PY
            - QA
            - RE
            - RO
            - RS
            - RU
            - RW
            - SA
            - SB
            - SC
            - SD
            - SE
            - SG
            - SH
            - SI
            - SJ
            - SK
            - SL
            - SM
            - SN
            - SO
            - SR
            - SS
            - ST
            - SV
            - SX
            - SY
            - SZ
            - TC
            - TD
            - TF
            - TG
            - TH
            - TJ
            - TK
            - TL
            - TM
            - TN
            - TO
            - TR
            - TT
            - TV
            - TW
            - TZ
            - UA
            - UG
            - UM
            - US
            - UY
            - UZ
            - VA
            - VC
            - VE
            - VG
            - VI
            - VN
            - VU
            - WF
            - WS
            - YE
            - YT
            - ZA
            - ZM
            - ZW
      type: object
      required:
        - country
      title: Address
    AddressDict:
      properties:
        line1:
          type: string
          title: Line1
        line2:
          type: string
          title: Line2
        postal_code:
          type: string
          title: Postal Code
        city:
          type: string
          title: City
        state:
          type: string
          title: State
        country:
          type: string
          title: Country
      type: object
      required:
        - country
      title: AddressDict
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
