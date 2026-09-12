> ## Documentation Index
> Fetch the complete documentation index at: https://polar.sh/docs/llms.txt
> Use this file to discover all available pages before exploring further.

# List Subscriptions

> List subscriptions.

**Scopes**: `subscriptions:read` `subscriptions:write`



## OpenAPI

````yaml /openapi/2026-04.openapi.json get /v1/subscriptions/
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
paths:
  /v1/subscriptions/:
    get:
      tags:
        - subscriptions
        - public
      summary: List Subscriptions
      description: |-
        List subscriptions.

        **Scopes**: `subscriptions:read` `subscriptions:write`
      operationId: subscriptions:list
      parameters:
        - name: organization_id
          in: query
          required: false
          schema:
            anyOf:
              - type: string
                format: uuid4
                description: The organization ID.
                examples:
                  - 1dbfc517-0bbf-4301-9ba8-555ca42b9737
              - type: array
                items:
                  type: string
                  format: uuid4
                  description: The organization ID.
                  examples:
                    - 1dbfc517-0bbf-4301-9ba8-555ca42b9737
              - type: 'null'
            title: OrganizationID Filter
            description: Filter by organization ID.
          description: Filter by organization ID.
        - name: product_id
          in: query
          required: false
          schema:
            anyOf:
              - type: string
                format: uuid4
                description: The product ID.
              - type: array
                items:
                  type: string
                  format: uuid4
                  description: The product ID.
              - type: 'null'
            title: ProductID Filter
            description: Filter by product ID.
          description: Filter by product ID.
        - name: customer_id
          in: query
          required: false
          schema:
            anyOf:
              - type: string
                format: uuid4
                description: The customer ID.
              - type: array
                items:
                  type: string
                  format: uuid4
                  description: The customer ID.
              - type: 'null'
            title: CustomerID Filter
            description: Filter by customer ID.
          description: Filter by customer ID.
        - name: external_customer_id
          in: query
          required: false
          schema:
            anyOf:
              - type: string
                description: The customer external ID.
              - type: array
                items:
                  type: string
                  description: The customer external ID.
              - type: 'null'
            title: ExternalCustomerID Filter
            description: Filter by customer external ID.
          description: Filter by customer external ID.
        - name: discount_id
          in: query
          required: false
          schema:
            anyOf:
              - type: string
                format: uuid4
                description: The discount ID.
              - type: array
                items:
                  type: string
                  format: uuid4
                  description: The discount ID.
              - type: 'null'
            title: DiscountID Filter
            description: Filter by discount ID.
          description: Filter by discount ID.
        - name: active
          in: query
          required: false
          schema:
            anyOf:
              - type: boolean
              - type: 'null'
            description: Filter by active or inactive subscription.
            deprecated: true
            title: Active
          description: Filter by active or inactive subscription.
          deprecated: true
        - name: status
          in: query
          required: false
          schema:
            anyOf:
              - $ref: '#/components/schemas/SubscriptionStatus'
              - type: array
                items:
                  $ref: '#/components/schemas/SubscriptionStatus'
              - type: 'null'
            title: Status Filter
            description: Filter by subscription status.
          description: Filter by subscription status.
        - name: cancel_at_period_end
          in: query
          required: false
          schema:
            anyOf:
              - type: boolean
              - type: 'null'
            description: Filter by subscriptions that are set to cancel at period end.
            title: Cancel At Period End
          description: Filter by subscriptions that are set to cancel at period end.
        - name: customer_cancellation_reason
          in: query
          required: false
          schema:
            anyOf:
              - $ref: '#/components/schemas/CustomerCancellationReason'
              - type: array
                items:
                  $ref: '#/components/schemas/CustomerCancellationReason'
              - type: 'null'
            title: CustomerCancellationReason Filter
            description: Filter by customer cancellation reason.
          description: Filter by customer cancellation reason.
        - name: canceled_at_after
          in: query
          required: false
          schema:
            anyOf:
              - type: string
                format: date-time
              - type: 'null'
            description: Filter by cancellation date (after or equal to).
            title: Canceled At After
          description: Filter by cancellation date (after or equal to).
        - name: canceled_at_before
          in: query
          required: false
          schema:
            anyOf:
              - type: string
                format: date-time
              - type: 'null'
            description: Filter by cancellation date (before or equal to).
            title: Canceled At Before
          description: Filter by cancellation date (before or equal to).
        - name: started_after
          in: query
          required: false
          schema:
            anyOf:
              - type: string
                format: date-time
              - type: 'null'
            description: Only include subscriptions started after this date.
            title: Started After
          description: Only include subscriptions started after this date.
        - name: started_before
          in: query
          required: false
          schema:
            anyOf:
              - type: string
                format: date-time
              - type: 'null'
            description: Only include subscriptions started before this date.
            title: Started Before
          description: Only include subscriptions started before this date.
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
        - name: sorting
          in: query
          required: false
          schema:
            anyOf:
              - type: array
                items:
                  $ref: '#/components/schemas/SubscriptionSortProperty'
              - type: 'null'
            description: >-
              Sorting criterion. Several criteria can be used simultaneously and
              will be applied in order. Add a minus sign `-` before the criteria
              name to sort by descending order.
            default:
              - '-started_at'
            title: Sorting
          description: >-
            Sorting criterion. Several criteria can be used simultaneously and
            will be applied in order. Add a minus sign `-` before the criteria
            name to sort by descending order.
        - name: metadata
          in: query
          required: false
          style: deepObject
          schema:
            $ref: '#/components/schemas/MetadataQuery'
          description: >-
            Filter by metadata key-value pairs. It uses the `deepObject` style,
            e.g. `?metadata[key]=value`.
      responses:
        '200':
          description: Successful Response
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/ListResource_Subscription_'
        '422':
          description: Validation Error
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/HTTPValidationError'
      security:
        - oidc:
            - subscriptions:read
            - subscriptions:write
        - pat:
            - subscriptions:read
            - subscriptions:write
        - oat:
            - subscriptions:read
            - subscriptions:write
      x-codeSamples:
        - lang: python
          source: |
            from polar.v2026_04 import Polar

            polar = Polar("polar_oat_xxx")

            for item in polar.subscriptions.iter_list(
                page=1,
                limit=10,
                sorting=['-started_at'],
            ):
                print(item)
        - lang: typescript
          source: |
            import { createPolar } from "@polar-sh/sdk/2026-04";

            const polar = createPolar({
              accessToken: "polar_oat_xxx",
            });

            for await (const item of polar.subscriptions.iterList(
              {
                "page": 1,
                "limit": 10,
                "sorting": [
                  "-started_at"
                ]
              },
            )) {
              console.log(item);
            }
components:
  schemas:
    SubscriptionStatus:
      type: string
      enum:
        - incomplete
        - incomplete_expired
        - trialing
        - active
        - past_due
        - canceled
        - unpaid
        - paused
      title: SubscriptionStatus
    CustomerCancellationReason:
      type: string
      enum:
        - customer_service
        - low_quality
        - missing_features
        - switched_service
        - too_complex
        - too_expensive
        - unused
        - other
      title: CustomerCancellationReason
    SubscriptionSortProperty:
      type: string
      enum:
        - customer
        - '-customer'
        - status
        - '-status'
        - started_at
        - '-started_at'
        - current_period_end
        - '-current_period_end'
        - ended_at
        - '-ended_at'
        - ends_at
        - '-ends_at'
        - amount
        - '-amount'
        - product
        - '-product'
        - discount
        - '-discount'
      title: SubscriptionSortProperty
    MetadataQuery:
      anyOf:
        - type: object
          additionalProperties:
            anyOf:
              - type: string
              - type: integer
              - type: boolean
              - items:
                  type: string
                type: array
              - type: array
                items:
                  type: integer
              - type: array
                items:
                  type: boolean
        - type: 'null'
      title: MetadataQuery
    ListResource_Subscription_:
      properties:
        items:
          items:
            $ref: '#/components/schemas/Subscription'
          type: array
          title: Items
        pagination:
          $ref: '#/components/schemas/Pagination'
      type: object
      required:
        - items
        - pagination
      title: ListResource[Subscription]
    HTTPValidationError:
      properties:
        detail:
          items:
            $ref: '#/components/schemas/ValidationError'
          type: array
          title: Detail
      type: object
      title: HTTPValidationError
    Subscription:
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
          format: uuid4
          title: Id
          description: The ID of the object.
        amount:
          type: integer
          title: Amount
          description: The amount of the subscription.
          examples:
            - 10000
        currency:
          type: string
          title: Currency
          description: The currency of the subscription.
          examples:
            - usd
        recurring_interval:
          $ref: '#/components/schemas/RecurringInterval'
          description: The interval at which the subscription recurs.
          examples:
            - month
        recurring_interval_count:
          type: integer
          title: Recurring Interval Count
          description: >-
            Number of interval units of the subscription. If this is set to 1
            the charge will happen every interval (e.g. every month), if set to
            2 it will be every other month, and so on.
        status:
          $ref: '#/components/schemas/SubscriptionStatus'
          description: The status of the subscription.
          examples:
            - active
        current_period_start:
          type: string
          format: date-time
          title: Current Period Start
          description: The start timestamp of the current billing period.
          examples:
            - '2026-01-01T00:00:00.000000Z'
        current_period_end:
          type: string
          format: date-time
          title: Current Period End
          description: The end timestamp of the current billing period.
          examples:
            - '2026-01-01T00:00:00.000000Z'
        current_meter_period_start:
          anyOf:
            - type: string
              format: date-time
              examples:
                - '2026-01-01T00:00:00.000000Z'
            - type: 'null'
          title: Current Meter Period Start
          description: >-
            The start timestamp of the current meter period, if the product has
            a meter cycle set. Metered credits are granted and overage is
            settled on this cadence.
        current_meter_period_end:
          anyOf:
            - type: string
              format: date-time
              examples:
                - '2026-01-01T00:00:00.000000Z'
            - type: 'null'
          title: Current Meter Period End
          description: >-
            The end timestamp of the current meter period, if the product has a
            meter cycle set. This is when credits next renew.
        trial_start:
          anyOf:
            - type: string
              format: date-time
              examples:
                - '2026-01-01T00:00:00.000000Z'
            - type: 'null'
          title: Trial Start
          description: The start timestamp of the trial period, if any.
        trial_end:
          anyOf:
            - type: string
              format: date-time
              examples:
                - '2026-01-01T00:00:00.000000Z'
            - type: 'null'
          title: Trial End
          description: The end timestamp of the trial period, if any.
        cancel_at_period_end:
          type: boolean
          title: Cancel At Period End
          description: >-
            Whether the subscription will be canceled at the end of the current
            period.
        canceled_at:
          anyOf:
            - type: string
              format: date-time
              examples:
                - '2026-01-01T00:00:00.000000Z'
            - type: 'null'
          title: Canceled At
          description: >-
            The timestamp when the subscription was canceled. The subscription
            might still be active if `cancel_at_period_end` is `true`.
        started_at:
          anyOf:
            - type: string
              format: date-time
              examples:
                - '2026-01-01T00:00:00.000000Z'
            - type: 'null'
          title: Started At
          description: The timestamp when the subscription started.
        ends_at:
          anyOf:
            - type: string
              format: date-time
              examples:
                - '2026-01-01T00:00:00.000000Z'
            - type: 'null'
          title: Ends At
          description: The timestamp when the subscription will end.
        ended_at:
          anyOf:
            - type: string
              format: date-time
              examples:
                - '2026-01-01T00:00:00.000000Z'
            - type: 'null'
          title: Ended At
          description: The timestamp when the subscription ended.
        past_due_at:
          anyOf:
            - type: string
              format: date-time
              examples:
                - '2026-01-01T00:00:00.000000Z'
            - type: 'null'
          title: Past Due At
          description: The timestamp when the subscription entered `past_due` status.
        pause_at_period_end:
          type: boolean
          title: Pause At Period End
          description: >-
            Whether the subscription will be paused at the end of the current
            period.
        paused_at:
          anyOf:
            - type: string
              format: date-time
              examples:
                - '2026-01-01T00:00:00.000000Z'
            - type: 'null'
          title: Paused At
          description: The timestamp when the subscription was paused.
        resumes_at:
          anyOf:
            - type: string
              format: date-time
              examples:
                - '2026-01-01T00:00:00.000000Z'
            - type: 'null'
          title: Resumes At
          description: >-
            The timestamp when a paused subscription is scheduled to
            automatically resume, if set.
        customer_id:
          type: string
          format: uuid4
          title: Customer Id
          description: The ID of the subscribed customer.
        product_id:
          type: string
          format: uuid4
          title: Product Id
          description: The ID of the subscribed product.
        discount_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Discount Id
          description: The ID of the applied discount, if any.
        checkout_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Checkout Id
        seats:
          anyOf:
            - type: integer
            - type: 'null'
          title: Seats
          description: >-
            The number of seats for seat-based subscriptions. None for non-seat
            subscriptions.
        units:
          anyOf:
            - type: integer
            - type: 'null'
          title: Units
          description: >-
            The number of units for unit-based subscriptions. None for non-unit
            subscriptions.
        customer_cancellation_reason:
          anyOf:
            - $ref: '#/components/schemas/CustomerCancellationReason'
            - type: 'null'
        customer_cancellation_comment:
          anyOf:
            - type: string
            - type: 'null'
          title: Customer Cancellation Comment
        metadata:
          $ref: '#/components/schemas/MetadataOutputType'
        custom_field_data:
          additionalProperties:
            anyOf:
              - type: string
              - type: integer
              - type: boolean
              - type: string
                format: date-time
                examples:
                  - '2026-01-01T00:00:00.000000Z'
              - type: 'null'
          type: object
          title: Custom Field Data
          description: Key-value object storing custom field values.
        customer:
          $ref: '#/components/schemas/SubscriptionCustomer'
        product:
          $ref: '#/components/schemas/Product'
        discount:
          anyOf:
            - oneOf:
                - $ref: '#/components/schemas/DiscountFixedOnceForeverDurationBase'
                - $ref: '#/components/schemas/DiscountFixedRepeatDurationBase'
                - $ref: >-
                    #/components/schemas/DiscountPercentageOnceForeverDurationBase
                - $ref: '#/components/schemas/DiscountPercentageRepeatDurationBase'
              title: SubscriptionDiscount
            - type: 'null'
          title: Discount
        prices:
          items:
            oneOf:
              - $ref: '#/components/schemas/LegacyRecurringProductPrice'
              - $ref: '#/components/schemas/ProductPrice'
          type: array
          title: Prices
          description: List of enabled prices for the subscription.
        meters:
          items:
            $ref: '#/components/schemas/SubscriptionMeter'
          type: array
          title: Meters
          description: List of meters associated with the subscription.
        pending_update:
          anyOf:
            - $ref: '#/components/schemas/PendingSubscriptionUpdate'
            - type: 'null'
          description: >-
            Pending subscription update that will be applied at the beginning of
            the next period. If `null`, there is no pending update.
      type: object
      required:
        - created_at
        - modified_at
        - id
        - amount
        - currency
        - recurring_interval
        - recurring_interval_count
        - status
        - current_period_start
        - current_period_end
        - current_meter_period_start
        - current_meter_period_end
        - trial_start
        - trial_end
        - cancel_at_period_end
        - canceled_at
        - started_at
        - ends_at
        - ended_at
        - pause_at_period_end
        - paused_at
        - resumes_at
        - customer_id
        - product_id
        - discount_id
        - checkout_id
        - units
        - customer_cancellation_reason
        - customer_cancellation_comment
        - metadata
        - customer
        - product
        - discount
        - prices
        - meters
        - pending_update
      title: Subscription
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
    RecurringInterval:
      type: string
      enum:
        - day
        - week
        - month
        - year
      title: RecurringInterval
    MetadataOutputType:
      additionalProperties:
        anyOf:
          - type: string
          - type: integer
          - type: number
          - type: boolean
      type: object
    SubscriptionCustomer:
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
          $ref: '#/components/schemas/CustomerType'
          description: >-
            The type of customer: 'individual' for single users, 'team' for
            customers with multiple members.
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
      title: SubscriptionCustomer
    Product:
      properties:
        id:
          type: string
          format: uuid4
          title: Id
          description: The ID of the object.
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
        trial_interval:
          anyOf:
            - $ref: '#/components/schemas/TrialInterval'
            - type: 'null'
          description: The interval unit for the trial period.
        trial_interval_count:
          anyOf:
            - type: integer
            - type: 'null'
          title: Trial Interval Count
          description: The number of interval units for the trial period.
        name:
          type: string
          title: Name
          description: The name of the product.
        description:
          anyOf:
            - type: string
            - type: 'null'
          title: Description
          description: The description of the product.
        visibility:
          $ref: '#/components/schemas/ProductVisibility'
          description: The visibility of the product.
        recurring_interval:
          anyOf:
            - $ref: '#/components/schemas/RecurringInterval'
            - type: 'null'
          description: >-
            The recurring interval of the product. If `None`, the product is a
            one-time purchase.
        recurring_interval_count:
          anyOf:
            - type: integer
            - type: 'null'
          title: Recurring Interval Count
          description: >-
            Number of interval units of the subscription. If this is set to 1
            the charge will happen every interval (e.g. every month), if set to
            2 it will be every other month, and so on. None for one-time
            products.
        meter_interval:
          anyOf:
            - $ref: '#/components/schemas/RecurringInterval'
            - type: 'null'
          description: >-
            The meter cycle of the product, independent of the billing interval.
            If `None`, metered concerns follow the billing interval.
        meter_interval_count:
          anyOf:
            - type: integer
            - type: 'null'
          title: Meter Interval Count
          description: Number of meter interval units. None when no meter cycle is set.
        is_recurring:
          type: boolean
          title: Is Recurring
          description: Whether the product is a subscription.
        is_archived:
          type: boolean
          title: Is Archived
          description: Whether the product is archived and no longer available.
        organization_id:
          type: string
          format: uuid4
          title: Organization Id
          description: The ID of the organization owning the product.
        metadata:
          $ref: '#/components/schemas/MetadataOutputType'
        prices:
          items:
            oneOf:
              - $ref: '#/components/schemas/LegacyRecurringProductPrice'
              - $ref: '#/components/schemas/ProductPrice'
          type: array
          title: Prices
          description: List of prices for this product.
        benefits:
          items:
            $ref: '#/components/schemas/Benefit'
            title: Benefit
          type: array
          title: Benefits
          description: List of benefits granted by the product.
        medias:
          items:
            $ref: '#/components/schemas/ProductMediaFileRead'
          type: array
          title: Medias
          description: List of medias associated to the product.
        attached_custom_fields:
          items:
            $ref: '#/components/schemas/AttachedCustomField'
          type: array
          title: Attached Custom Fields
          description: List of custom fields attached to the product.
      type: object
      required:
        - id
        - created_at
        - modified_at
        - trial_interval
        - trial_interval_count
        - name
        - description
        - visibility
        - recurring_interval
        - recurring_interval_count
        - meter_interval
        - meter_interval_count
        - is_recurring
        - is_archived
        - organization_id
        - metadata
        - prices
        - benefits
        - medias
        - attached_custom_fields
      title: Product
      description: A product.
    DiscountFixedOnceForeverDurationBase:
      properties:
        duration:
          $ref: '#/components/schemas/DiscountDuration'
        type:
          $ref: '#/components/schemas/DiscountType'
        amount:
          type: integer
          title: Amount
          deprecated: true
          examples:
            - 1000
        currency:
          type: string
          title: Currency
          deprecated: true
          examples:
            - usd
        amounts:
          additionalProperties:
            type: integer
          type: object
          title: Amounts
          description: Map of currency to fixed amount to discount from the total.
          examples:
            - eur: 900
              usd: 1000
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
          format: uuid4
          title: Id
          description: The ID of the object.
        metadata:
          $ref: '#/components/schemas/MetadataOutputType'
        name:
          type: string
          title: Name
          description: >-
            Name of the discount. Will be displayed to the customer when the
            discount is applied.
        code:
          anyOf:
            - type: string
            - type: 'null'
          title: Code
          description: Code customers can use to apply the discount during checkout.
        starts_at:
          anyOf:
            - type: string
              format: date-time
              examples:
                - '2026-01-01T00:00:00.000000Z'
            - type: 'null'
          title: Starts At
          description: Timestamp after which the discount is redeemable.
        ends_at:
          anyOf:
            - type: string
              format: date-time
              examples:
                - '2026-01-01T00:00:00.000000Z'
            - type: 'null'
          title: Ends At
          description: Timestamp after which the discount is no longer redeemable.
        max_redemptions:
          anyOf:
            - type: integer
            - type: 'null'
          title: Max Redemptions
          description: Maximum number of times the discount can be redeemed.
        max_redemptions_per_customer:
          anyOf:
            - type: integer
            - type: 'null'
          title: Max Redemptions Per Customer
          description: >-
            Maximum number of times the discount can be redeemed by a single
            customer.
        redemptions_count:
          type: integer
          title: Redemptions Count
          description: Number of times the discount has been redeemed.
        organization_id:
          type: string
          format: uuid4
          title: Organization Id
          description: The organization ID.
          examples:
            - 1dbfc517-0bbf-4301-9ba8-555ca42b9737
      type: object
      required:
        - duration
        - type
        - amount
        - currency
        - amounts
        - created_at
        - modified_at
        - id
        - metadata
        - name
        - code
        - starts_at
        - ends_at
        - max_redemptions
        - max_redemptions_per_customer
        - redemptions_count
        - organization_id
      title: DiscountFixedOnceForeverDurationBase
    DiscountFixedRepeatDurationBase:
      properties:
        duration:
          $ref: '#/components/schemas/DiscountDuration'
        duration_in_months:
          type: integer
          title: Duration In Months
        type:
          $ref: '#/components/schemas/DiscountType'
        amount:
          type: integer
          title: Amount
          deprecated: true
          examples:
            - 1000
        currency:
          type: string
          title: Currency
          deprecated: true
          examples:
            - usd
        amounts:
          additionalProperties:
            type: integer
          type: object
          title: Amounts
          description: Map of currency to fixed amount to discount from the total.
          examples:
            - eur: 900
              usd: 1000
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
          format: uuid4
          title: Id
          description: The ID of the object.
        metadata:
          $ref: '#/components/schemas/MetadataOutputType'
        name:
          type: string
          title: Name
          description: >-
            Name of the discount. Will be displayed to the customer when the
            discount is applied.
        code:
          anyOf:
            - type: string
            - type: 'null'
          title: Code
          description: Code customers can use to apply the discount during checkout.
        starts_at:
          anyOf:
            - type: string
              format: date-time
              examples:
                - '2026-01-01T00:00:00.000000Z'
            - type: 'null'
          title: Starts At
          description: Timestamp after which the discount is redeemable.
        ends_at:
          anyOf:
            - type: string
              format: date-time
              examples:
                - '2026-01-01T00:00:00.000000Z'
            - type: 'null'
          title: Ends At
          description: Timestamp after which the discount is no longer redeemable.
        max_redemptions:
          anyOf:
            - type: integer
            - type: 'null'
          title: Max Redemptions
          description: Maximum number of times the discount can be redeemed.
        max_redemptions_per_customer:
          anyOf:
            - type: integer
            - type: 'null'
          title: Max Redemptions Per Customer
          description: >-
            Maximum number of times the discount can be redeemed by a single
            customer.
        redemptions_count:
          type: integer
          title: Redemptions Count
          description: Number of times the discount has been redeemed.
        organization_id:
          type: string
          format: uuid4
          title: Organization Id
          description: The organization ID.
          examples:
            - 1dbfc517-0bbf-4301-9ba8-555ca42b9737
      type: object
      required:
        - duration
        - duration_in_months
        - type
        - amount
        - currency
        - amounts
        - created_at
        - modified_at
        - id
        - metadata
        - name
        - code
        - starts_at
        - ends_at
        - max_redemptions
        - max_redemptions_per_customer
        - redemptions_count
        - organization_id
      title: DiscountFixedRepeatDurationBase
    DiscountPercentageOnceForeverDurationBase:
      properties:
        duration:
          $ref: '#/components/schemas/DiscountDuration'
        type:
          $ref: '#/components/schemas/DiscountType'
        basis_points:
          type: integer
          title: Basis Points
          description: >-
            Discount percentage in basis points. A basis point is 1/100th of a
            percent. For example, 1000 basis points equals a 10% discount.
          examples:
            - 1000
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
          format: uuid4
          title: Id
          description: The ID of the object.
        metadata:
          $ref: '#/components/schemas/MetadataOutputType'
        name:
          type: string
          title: Name
          description: >-
            Name of the discount. Will be displayed to the customer when the
            discount is applied.
        code:
          anyOf:
            - type: string
            - type: 'null'
          title: Code
          description: Code customers can use to apply the discount during checkout.
        starts_at:
          anyOf:
            - type: string
              format: date-time
              examples:
                - '2026-01-01T00:00:00.000000Z'
            - type: 'null'
          title: Starts At
          description: Timestamp after which the discount is redeemable.
        ends_at:
          anyOf:
            - type: string
              format: date-time
              examples:
                - '2026-01-01T00:00:00.000000Z'
            - type: 'null'
          title: Ends At
          description: Timestamp after which the discount is no longer redeemable.
        max_redemptions:
          anyOf:
            - type: integer
            - type: 'null'
          title: Max Redemptions
          description: Maximum number of times the discount can be redeemed.
        max_redemptions_per_customer:
          anyOf:
            - type: integer
            - type: 'null'
          title: Max Redemptions Per Customer
          description: >-
            Maximum number of times the discount can be redeemed by a single
            customer.
        redemptions_count:
          type: integer
          title: Redemptions Count
          description: Number of times the discount has been redeemed.
        organization_id:
          type: string
          format: uuid4
          title: Organization Id
          description: The organization ID.
          examples:
            - 1dbfc517-0bbf-4301-9ba8-555ca42b9737
      type: object
      required:
        - duration
        - type
        - basis_points
        - created_at
        - modified_at
        - id
        - metadata
        - name
        - code
        - starts_at
        - ends_at
        - max_redemptions
        - max_redemptions_per_customer
        - redemptions_count
        - organization_id
      title: DiscountPercentageOnceForeverDurationBase
    DiscountPercentageRepeatDurationBase:
      properties:
        duration:
          $ref: '#/components/schemas/DiscountDuration'
        duration_in_months:
          type: integer
          title: Duration In Months
        type:
          $ref: '#/components/schemas/DiscountType'
        basis_points:
          type: integer
          title: Basis Points
          description: >-
            Discount percentage in basis points. A basis point is 1/100th of a
            percent. For example, 1000 basis points equals a 10% discount.
          examples:
            - 1000
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
          format: uuid4
          title: Id
          description: The ID of the object.
        metadata:
          $ref: '#/components/schemas/MetadataOutputType'
        name:
          type: string
          title: Name
          description: >-
            Name of the discount. Will be displayed to the customer when the
            discount is applied.
        code:
          anyOf:
            - type: string
            - type: 'null'
          title: Code
          description: Code customers can use to apply the discount during checkout.
        starts_at:
          anyOf:
            - type: string
              format: date-time
              examples:
                - '2026-01-01T00:00:00.000000Z'
            - type: 'null'
          title: Starts At
          description: Timestamp after which the discount is redeemable.
        ends_at:
          anyOf:
            - type: string
              format: date-time
              examples:
                - '2026-01-01T00:00:00.000000Z'
            - type: 'null'
          title: Ends At
          description: Timestamp after which the discount is no longer redeemable.
        max_redemptions:
          anyOf:
            - type: integer
            - type: 'null'
          title: Max Redemptions
          description: Maximum number of times the discount can be redeemed.
        max_redemptions_per_customer:
          anyOf:
            - type: integer
            - type: 'null'
          title: Max Redemptions Per Customer
          description: >-
            Maximum number of times the discount can be redeemed by a single
            customer.
        redemptions_count:
          type: integer
          title: Redemptions Count
          description: Number of times the discount has been redeemed.
        organization_id:
          type: string
          format: uuid4
          title: Organization Id
          description: The organization ID.
          examples:
            - 1dbfc517-0bbf-4301-9ba8-555ca42b9737
      type: object
      required:
        - duration
        - duration_in_months
        - type
        - basis_points
        - created_at
        - modified_at
        - id
        - metadata
        - name
        - code
        - starts_at
        - ends_at
        - max_redemptions
        - max_redemptions_per_customer
        - redemptions_count
        - organization_id
      title: DiscountPercentageRepeatDurationBase
    LegacyRecurringProductPrice:
      oneOf:
        - $ref: '#/components/schemas/LegacyRecurringProductPriceFixed'
        - $ref: '#/components/schemas/LegacyRecurringProductPriceCustom'
      discriminator:
        propertyName: amount_type
        mapping:
          custom:
            $ref: '#/components/schemas/LegacyRecurringProductPriceCustom'
          fixed:
            $ref: '#/components/schemas/LegacyRecurringProductPriceFixed'
    ProductPrice:
      oneOf:
        - $ref: '#/components/schemas/ProductPriceFixed'
        - $ref: '#/components/schemas/ProductPriceCustom'
        - $ref: '#/components/schemas/ProductPriceSeatBased'
        - $ref: '#/components/schemas/ProductPriceUnitBased'
        - $ref: '#/components/schemas/ProductPriceMeteredUnit'
        - $ref: '#/components/schemas/ProductPriceMeteredTiers'
      discriminator:
        propertyName: amount_type
        mapping:
          custom:
            $ref: '#/components/schemas/ProductPriceCustom'
          fixed:
            $ref: '#/components/schemas/ProductPriceFixed'
          metered_tiers:
            $ref: '#/components/schemas/ProductPriceMeteredTiers'
          metered_unit:
            $ref: '#/components/schemas/ProductPriceMeteredUnit'
          seat_based:
            $ref: '#/components/schemas/ProductPriceSeatBased'
          unit_based:
            $ref: '#/components/schemas/ProductPriceUnitBased'
    SubscriptionMeter:
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
          format: uuid4
          title: Id
          description: The ID of the object.
        consumed_units:
          type: number
          title: Consumed Units
          description: The number of consumed units so far in this billing period.
          examples:
            - 25
        credited_units:
          type: integer
          title: Credited Units
          description: The number of credited units so far in this billing period.
          examples:
            - 100
        amount:
          type: integer
          title: Amount
          description: The amount due in cents so far in this billing period.
          examples:
            - 0
        meter_id:
          type: string
          format: uuid4
          title: Meter Id
          description: The ID of the meter.
          examples:
            - d498a884-e2cd-4d3e-8002-f536468a8b22
        meter:
          $ref: '#/components/schemas/Meter'
          description: The meter associated with this subscription.
      type: object
      required:
        - created_at
        - modified_at
        - id
        - consumed_units
        - credited_units
        - amount
        - meter_id
        - meter
      title: SubscriptionMeter
      description: Current consumption and spending for a subscription meter.
    PendingSubscriptionUpdate:
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
          format: uuid4
          title: Id
          description: The ID of the object.
        applies_at:
          type: string
          format: date-time
          title: Applies At
          description: The date and time when the subscription update will be applied.
          examples:
            - '2026-01-01T00:00:00.000000Z'
        product_id:
          anyOf:
            - type: string
              format: uuid4
            - type: 'null'
          title: Product Id
          description: >-
            ID of the new product to apply to the subscription. If `null`, the
            product won't be changed.
        seats:
          anyOf:
            - type: integer
            - type: 'null'
          title: Seats
          description: >-
            Number of seats to apply to the subscription. If `null`, the number
            of seats won't be changed.
        units:
          anyOf:
            - type: integer
            - type: 'null'
          title: Units
          description: >-
            Number of units to apply to the subscription. If `null`, the number
            of units won't be changed.
      type: object
      required:
        - created_at
        - modified_at
        - id
        - applies_at
        - product_id
        - seats
        - units
      title: PendingSubscriptionUpdate
      description: >-
        Pending update to be applied to a subscription at the beginning of the
        next period.
    CustomerType:
      type: string
      enum:
        - individual
        - team
      title: CustomerType
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
    TrialInterval:
      type: string
      enum:
        - day
        - week
        - month
        - year
      title: TrialInterval
    ProductVisibility:
      type: string
      enum:
        - draft
        - private
        - public
      title: Visibility
    Benefit:
      oneOf:
        - $ref: '#/components/schemas/BenefitCustom'
        - $ref: '#/components/schemas/BenefitDiscord'
        - $ref: '#/components/schemas/BenefitGitHubRepository'
        - $ref: '#/components/schemas/BenefitDownloadables'
        - $ref: '#/components/schemas/BenefitLicenseKeys'
        - $ref: '#/components/schemas/BenefitMeterCredit'
        - $ref: '#/components/schemas/BenefitFeatureFlag'
        - $ref: '#/components/schemas/BenefitSlackSharedChannel'
      discriminator:
        propertyName: type
        mapping:
          custom:
            $ref: '#/components/schemas/BenefitCustom'
          discord:
            $ref: '#/components/schemas/BenefitDiscord'
          downloadables:
            $ref: '#/components/schemas/BenefitDownloadables'
          feature_flag:
            $ref: '#/components/schemas/BenefitFeatureFlag'
          github_repository:
            $ref: '#/components/schemas/BenefitGitHubRepository'
          license_keys:
            $ref: '#/components/schemas/BenefitLicenseKeys'
          meter_credit:
            $ref: '#/components/schemas/BenefitMeterCredit'
          slack_shared_channel:
            $ref: '#/components/schemas/BenefitSlackSharedChannel'
    ProductMediaFileRead:
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
              examples:
                - '2026-01-01T00:00:00.000000Z'
            - type: 'null'
          title: Last Modified At
        version:
          anyOf:
            - type: string
            - type: 'null'
          title: Version
        service:
          type: string
          const: product_media
          title: Service
        is_uploaded:
          type: boolean
          title: Is Uploaded
        created_at:
          type: string
          format: date-time
          title: Created At
          examples:
            - '2026-01-01T00:00:00.000000Z'
        size_readable:
          type: string
          title: Size Readable
          readOnly: true
        public_url:
          type: string
          title: Public Url
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
        - version
        - service
        - is_uploaded
        - created_at
        - size_readable
        - public_url
      title: ProductMediaFileRead
      description: File to be used as a product media file.
    AttachedCustomField:
      properties:
        custom_field_id:
          type: string
          format: uuid4
          title: Custom Field Id
          description: ID of the custom field.
        custom_field:
          $ref: '#/components/schemas/CustomField'
          title: CustomField
        order:
          type: integer
          title: Order
          description: Order of the custom field in the resource.
        required:
          type: boolean
          title: Required
          description: Whether the value is required for this custom field.
      type: object
      required:
        - custom_field_id
        - custom_field
        - order
        - required
      title: AttachedCustomField
      description: Schema of a custom field attached to a resource.
    DiscountDuration:
      type: string
      enum:
        - once
        - forever
        - repeating
      title: DiscountDuration
    DiscountType:
      type: string
      enum:
        - fixed
        - percentage
      title: DiscountType
    LegacyRecurringProductPriceFixed:
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
          format: uuid4
          title: Id
          description: The ID of the price.
        source:
          $ref: '#/components/schemas/ProductPriceSource'
          description: >-
            The source of the price . `catalog` is a predefined price, while
            `ad_hoc` is a price created dynamically on a Checkout session.
        amount_type:
          type: string
          const: fixed
          title: Amount Type
        price_currency:
          type: string
          title: Price Currency
          description: The currency in which the customer will be charged.
        tax_behavior:
          anyOf:
            - $ref: '#/components/schemas/TaxBehaviorOption'
            - type: 'null'
          description: >-
            The tax behavior of the price. If null, it defaults to the
            organization's default tax behavior.
        is_archived:
          type: boolean
          title: Is Archived
          description: Whether the price is archived and no longer available.
        product_id:
          type: string
          format: uuid4
          title: Product Id
          description: The ID of the product owning the price.
        type:
          type: string
          const: recurring
          title: Type
          description: The type of the price.
        recurring_interval:
          $ref: '#/components/schemas/RecurringInterval'
          description: The recurring interval of the price.
        price_amount:
          type: integer
          title: Price Amount
          description: The price in cents.
        legacy:
          type: boolean
          const: true
          title: Legacy
          readOnly: true
      type: object
      required:
        - created_at
        - modified_at
        - id
        - source
        - amount_type
        - price_currency
        - tax_behavior
        - is_archived
        - product_id
        - type
        - recurring_interval
        - price_amount
        - legacy
      title: LegacyRecurringProductPriceFixed
      description: >-
        A recurring price for a product, i.e. a subscription.


        **Deprecated**: The recurring interval should be set on the product
        itself.
    LegacyRecurringProductPriceCustom:
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
          format: uuid4
          title: Id
          description: The ID of the price.
        source:
          $ref: '#/components/schemas/ProductPriceSource'
          description: >-
            The source of the price . `catalog` is a predefined price, while
            `ad_hoc` is a price created dynamically on a Checkout session.
        amount_type:
          type: string
          const: custom
          title: Amount Type
        price_currency:
          type: string
          title: Price Currency
          description: The currency in which the customer will be charged.
        tax_behavior:
          anyOf:
            - $ref: '#/components/schemas/TaxBehaviorOption'
            - type: 'null'
          description: >-
            The tax behavior of the price. If null, it defaults to the
            organization's default tax behavior.
        is_archived:
          type: boolean
          title: Is Archived
          description: Whether the price is archived and no longer available.
        product_id:
          type: string
          format: uuid4
          title: Product Id
          description: The ID of the product owning the price.
        type:
          type: string
          const: recurring
          title: Type
          description: The type of the price.
        recurring_interval:
          $ref: '#/components/schemas/RecurringInterval'
          description: The recurring interval of the price.
        minimum_amount:
          type: integer
          title: Minimum Amount
          description: >-
            The minimum amount the customer can pay. If 0, the price is 'free or
            pay what you want'.
        maximum_amount:
          anyOf:
            - type: integer
            - type: 'null'
          title: Maximum Amount
          description: The maximum amount the customer can pay.
        preset_amount:
          anyOf:
            - type: integer
            - type: 'null'
          title: Preset Amount
          description: The initial amount shown to the customer.
        legacy:
          type: boolean
          const: true
          title: Legacy
          readOnly: true
      type: object
      required:
        - created_at
        - modified_at
        - id
        - source
        - amount_type
        - price_currency
        - tax_behavior
        - is_archived
        - product_id
        - type
        - recurring_interval
        - minimum_amount
        - maximum_amount
        - preset_amount
        - legacy
      title: LegacyRecurringProductPriceCustom
      description: >-
        A pay-what-you-want recurring price for a product, i.e. a subscription.


        **Deprecated**: The recurring interval should be set on the product
        itself.
    ProductPriceFixed:
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
          format: uuid4
          title: Id
          description: The ID of the price.
        source:
          $ref: '#/components/schemas/ProductPriceSource'
          description: >-
            The source of the price . `catalog` is a predefined price, while
            `ad_hoc` is a price created dynamically on a Checkout session.
        amount_type:
          type: string
          const: fixed
          title: Amount Type
        price_currency:
          type: string
          title: Price Currency
          description: The currency in which the customer will be charged.
        tax_behavior:
          anyOf:
            - $ref: '#/components/schemas/TaxBehaviorOption'
            - type: 'null'
          description: >-
            The tax behavior of the price. If null, it defaults to the
            organization's default tax behavior.
        is_archived:
          type: boolean
          title: Is Archived
          description: Whether the price is archived and no longer available.
        product_id:
          type: string
          format: uuid4
          title: Product Id
          description: The ID of the product owning the price.
        price_amount:
          type: integer
          title: Price Amount
          description: The price in cents.
      type: object
      required:
        - created_at
        - modified_at
        - id
        - source
        - amount_type
        - price_currency
        - tax_behavior
        - is_archived
        - product_id
        - price_amount
      title: ProductPriceFixed
      description: A fixed price for a product.
    ProductPriceCustom:
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
          format: uuid4
          title: Id
          description: The ID of the price.
        source:
          $ref: '#/components/schemas/ProductPriceSource'
          description: >-
            The source of the price . `catalog` is a predefined price, while
            `ad_hoc` is a price created dynamically on a Checkout session.
        amount_type:
          type: string
          const: custom
          title: Amount Type
        price_currency:
          type: string
          title: Price Currency
          description: The currency in which the customer will be charged.
        tax_behavior:
          anyOf:
            - $ref: '#/components/schemas/TaxBehaviorOption'
            - type: 'null'
          description: >-
            The tax behavior of the price. If null, it defaults to the
            organization's default tax behavior.
        is_archived:
          type: boolean
          title: Is Archived
          description: Whether the price is archived and no longer available.
        product_id:
          type: string
          format: uuid4
          title: Product Id
          description: The ID of the product owning the price.
        minimum_amount:
          type: integer
          title: Minimum Amount
          description: >-
            The minimum amount the customer can pay. If 0, the price is 'free or
            pay what you want'.
        maximum_amount:
          anyOf:
            - type: integer
            - type: 'null'
          title: Maximum Amount
          description: The maximum amount the customer can pay.
        preset_amount:
          anyOf:
            - type: integer
            - type: 'null'
          title: Preset Amount
          description: The initial amount shown to the customer.
      type: object
      required:
        - created_at
        - modified_at
        - id
        - source
        - amount_type
        - price_currency
        - tax_behavior
        - is_archived
        - product_id
        - minimum_amount
        - maximum_amount
        - preset_amount
      title: ProductPriceCustom
      description: A pay-what-you-want price for a product.
    ProductPriceSeatBased:
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
          format: uuid4
          title: Id
          description: The ID of the price.
        source:
          $ref: '#/components/schemas/ProductPriceSource'
          description: >-
            The source of the price . `catalog` is a predefined price, while
            `ad_hoc` is a price created dynamically on a Checkout session.
        amount_type:
          type: string
          const: seat_based
          title: Amount Type
        price_currency:
          type: string
          title: Price Currency
          description: The currency in which the customer will be charged.
        tax_behavior:
          anyOf:
            - $ref: '#/components/schemas/TaxBehaviorOption'
            - type: 'null'
          description: >-
            The tax behavior of the price. If null, it defaults to the
            organization's default tax behavior.
        is_archived:
          type: boolean
          title: Is Archived
          description: Whether the price is archived and no longer available.
        product_id:
          type: string
          format: uuid4
          title: Product Id
          description: The ID of the product owning the price.
        seat_tiers:
          $ref: '#/components/schemas/ProductPriceSeatTiers-Output'
          description: Tiered pricing based on seat quantity
      type: object
      required:
        - created_at
        - modified_at
        - id
        - source
        - amount_type
        - price_currency
        - tax_behavior
        - is_archived
        - product_id
        - seat_tiers
      title: ProductPriceSeatBased
      description: A seat-based price for a product.
    ProductPriceUnitBased:
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
          format: uuid4
          title: Id
          description: The ID of the price.
        source:
          $ref: '#/components/schemas/ProductPriceSource'
          description: >-
            The source of the price . `catalog` is a predefined price, while
            `ad_hoc` is a price created dynamically on a Checkout session.
        amount_type:
          type: string
          const: unit_based
          title: Amount Type
        price_currency:
          type: string
          title: Price Currency
          description: The currency in which the customer will be charged.
        tax_behavior:
          anyOf:
            - $ref: '#/components/schemas/TaxBehaviorOption'
            - type: 'null'
          description: >-
            The tax behavior of the price. If null, it defaults to the
            organization's default tax behavior.
        is_archived:
          type: boolean
          title: Is Archived
          description: Whether the price is archived and no longer available.
        product_id:
          type: string
          format: uuid4
          title: Product Id
          description: The ID of the product owning the price.
        tiers:
          $ref: '#/components/schemas/Tiers'
          description: Tiered pricing based on the purchased unit quantity.
        minimum_units:
          anyOf:
            - type: integer
            - type: 'null'
          title: Minimum Units
          description: The minimum purchasable quantity (inclusive).
        unit_label:
          anyOf:
            - additionalProperties:
                additionalProperties:
                  type: string
                type: object
              type: object
              minLength: 1
              examples:
                - en:
                    '=1': seat
                    other: seats
            - type: 'null'
          title: Unit Label
          description: >-
            Per-locale unit nouns shown at checkout and on invoices. `null`
            defaults to "unit"/"units".
        maximum_units:
          anyOf:
            - type: integer
            - type: 'null'
          title: Maximum Units
          description: >-
            The maximum purchasable quantity, from the last tier's bound. `null`
            for unlimited.
          readOnly: true
      type: object
      required:
        - created_at
        - modified_at
        - id
        - source
        - amount_type
        - price_currency
        - tax_behavior
        - is_archived
        - product_id
        - tiers
        - minimum_units
        - unit_label
        - maximum_units
      title: ProductPriceUnitBased
      description: |-
        A unit-based price for a product: the buyer picks a quantity of units,
        pays for it up-front. On subscriptions, quantity changes are prorated.
    ProductPriceMeteredUnit:
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
          format: uuid4
          title: Id
          description: The ID of the price.
        source:
          $ref: '#/components/schemas/ProductPriceSource'
          description: >-
            The source of the price . `catalog` is a predefined price, while
            `ad_hoc` is a price created dynamically on a Checkout session.
        amount_type:
          type: string
          const: metered_unit
          title: Amount Type
        price_currency:
          type: string
          title: Price Currency
          description: The currency in which the customer will be charged.
        tax_behavior:
          anyOf:
            - $ref: '#/components/schemas/TaxBehaviorOption'
            - type: 'null'
          description: >-
            The tax behavior of the price. If null, it defaults to the
            organization's default tax behavior.
        is_archived:
          type: boolean
          title: Is Archived
          description: Whether the price is archived and no longer available.
        product_id:
          type: string
          format: uuid4
          title: Product Id
          description: The ID of the product owning the price.
        cap_amount:
          anyOf:
            - type: integer
            - type: 'null'
          title: Cap Amount
          description: >-
            The maximum amount in cents that can be charged, regardless of the
            number of units consumed.
        meter_id:
          type: string
          format: uuid4
          title: Meter Id
          description: The ID of the meter associated to the price.
        meter:
          $ref: '#/components/schemas/ProductPriceMeter'
          description: The meter associated to the price.
        unit_amount:
          type: string
          pattern: ^(?!^[-+.]*$)[+-]?0*\d*\.?\d*$
          title: Unit Amount
          description: The price per unit in cents.
      type: object
      required:
        - created_at
        - modified_at
        - id
        - source
        - amount_type
        - price_currency
        - tax_behavior
        - is_archived
        - product_id
        - cap_amount
        - meter_id
        - meter
        - unit_amount
      title: ProductPriceMeteredUnit
      description: A metered, usage-based, price for a product, with a fixed unit price.
    ProductPriceMeteredTiers:
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
          format: uuid4
          title: Id
          description: The ID of the price.
        source:
          $ref: '#/components/schemas/ProductPriceSource'
          description: >-
            The source of the price . `catalog` is a predefined price, while
            `ad_hoc` is a price created dynamically on a Checkout session.
        amount_type:
          type: string
          const: metered_tiers
          title: Amount Type
        price_currency:
          type: string
          title: Price Currency
          description: The currency in which the customer will be charged.
        tax_behavior:
          anyOf:
            - $ref: '#/components/schemas/TaxBehaviorOption'
            - type: 'null'
          description: >-
            The tax behavior of the price. If null, it defaults to the
            organization's default tax behavior.
        is_archived:
          type: boolean
          title: Is Archived
          description: Whether the price is archived and no longer available.
        product_id:
          type: string
          format: uuid4
          title: Product Id
          description: The ID of the product owning the price.
        cap_amount:
          anyOf:
            - type: integer
            - type: 'null'
          title: Cap Amount
          description: >-
            The maximum amount in cents that can be charged, regardless of the
            number of units consumed.
        meter_id:
          type: string
          format: uuid4
          title: Meter Id
          description: The ID of the meter associated to the price.
        meter:
          $ref: '#/components/schemas/ProductPriceMeter'
          description: The meter associated to the price.
        tiers:
          $ref: '#/components/schemas/Tiers'
          description: The pricing tiers based on consumed units.
      type: object
      required:
        - created_at
        - modified_at
        - id
        - source
        - amount_type
        - price_currency
        - tax_behavior
        - is_archived
        - product_id
        - cap_amount
        - meter_id
        - meter
        - tiers
      title: ProductPriceMeteredTiers
      description: A metered, usage-based, price for a product, billed from tiers.
    Meter:
      properties:
        metadata:
          $ref: '#/components/schemas/MetadataOutputType'
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
          format: uuid4
          title: Id
          description: The ID of the object.
        name:
          type: string
          title: Name
          description: >-
            The name of the meter. Will be shown on customer's invoices and
            usage.
        unit:
          $ref: '#/components/schemas/MeterUnit'
          description: The unit of the meter.
        custom_label:
          anyOf:
            - type: string
            - type: 'null'
          title: Custom Label
          description: The label for the custom unit.
        custom_multiplier:
          anyOf:
            - type: integer
            - type: 'null'
          title: Custom Multiplier
          description: The multiplier to convert from base unit to display scale.
        filter:
          $ref: '#/components/schemas/Filter'
          description: >-
            The filter to apply on events that'll be used to calculate the
            meter.
        aggregation:
          oneOf:
            - $ref: '#/components/schemas/CountAggregation'
            - $ref: '#/components/schemas/PropertyAggregation'
            - $ref: '#/components/schemas/UniqueAggregation'
          title: Aggregation
          description: >-
            The aggregation to apply on the filtered events to calculate the
            meter.
          discriminator:
            propertyName: func
            mapping:
              avg:
                $ref: '#/components/schemas/PropertyAggregation'
              count:
                $ref: '#/components/schemas/CountAggregation'
              max:
                $ref: '#/components/schemas/PropertyAggregation'
              min:
                $ref: '#/components/schemas/PropertyAggregation'
              sum:
                $ref: '#/components/schemas/PropertyAggregation'
              unique:
                $ref: '#/components/schemas/UniqueAggregation'
        organization_id:
          type: string
          format: uuid4
          title: Organization Id
          description: The ID of the organization owning the meter.
        archived_at:
          anyOf:
            - type: string
              format: date-time
              examples:
                - '2026-01-01T00:00:00.000000Z'
            - type: 'null'
          title: Archived At
          description: Whether the meter is archived and the time it was archived.
      type: object
      required:
        - metadata
        - created_at
        - modified_at
        - id
        - name
        - unit
        - filter
        - aggregation
        - organization_id
      title: Meter
    BenefitCustom:
      properties:
        id:
          type: string
          format: uuid4
          title: Id
          description: The ID of the benefit.
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
        type:
          type: string
          const: custom
          title: Type
        description:
          type: string
          title: Description
          description: The description of the benefit.
        selectable:
          type: boolean
          title: Selectable
          description: Whether the benefit is selectable when creating a product.
        deletable:
          type: boolean
          title: Deletable
          description: Whether the benefit is deletable.
        is_deleted:
          type: boolean
          title: Is Deleted
          description: Whether the benefit is deleted.
        organization_id:
          type: string
          format: uuid4
          title: Organization Id
          description: The ID of the organization owning the benefit.
        metadata:
          $ref: '#/components/schemas/MetadataOutputType'
        visibility:
          $ref: '#/components/schemas/BenefitVisibility'
          description: The visibility of the benefit in the customer portal.
        properties:
          $ref: '#/components/schemas/BenefitCustomProperties'
        visibility_configurable:
          type: boolean
          title: Visibility Configurable
          readOnly: true
      type: object
      required:
        - id
        - created_at
        - modified_at
        - type
        - description
        - selectable
        - deletable
        - is_deleted
        - organization_id
        - metadata
        - visibility
        - properties
        - visibility_configurable
      title: BenefitCustom
      description: |-
        A benefit of type `custom`.

        Use it to grant any kind of benefit that doesn't fit in the other types.
    BenefitDiscord:
      properties:
        id:
          type: string
          format: uuid4
          title: Id
          description: The ID of the benefit.
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
        type:
          type: string
          const: discord
          title: Type
        description:
          type: string
          title: Description
          description: The description of the benefit.
        selectable:
          type: boolean
          title: Selectable
          description: Whether the benefit is selectable when creating a product.
        deletable:
          type: boolean
          title: Deletable
          description: Whether the benefit is deletable.
        is_deleted:
          type: boolean
          title: Is Deleted
          description: Whether the benefit is deleted.
        organization_id:
          type: string
          format: uuid4
          title: Organization Id
          description: The ID of the organization owning the benefit.
        metadata:
          $ref: '#/components/schemas/MetadataOutputType'
        visibility:
          $ref: '#/components/schemas/BenefitVisibility'
          description: The visibility of the benefit in the customer portal.
        properties:
          $ref: '#/components/schemas/BenefitDiscordProperties'
        visibility_configurable:
          type: boolean
          title: Visibility Configurable
          readOnly: true
      type: object
      required:
        - id
        - created_at
        - modified_at
        - type
        - description
        - selectable
        - deletable
        - is_deleted
        - organization_id
        - metadata
        - visibility
        - properties
        - visibility_configurable
      title: BenefitDiscord
      description: |-
        A benefit of type `discord`.

        Use it to automatically invite your backers to a Discord server.
    BenefitGitHubRepository:
      properties:
        id:
          type: string
          format: uuid4
          title: Id
          description: The ID of the benefit.
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
        type:
          type: string
          const: github_repository
          title: Type
        description:
          type: string
          title: Description
          description: The description of the benefit.
        selectable:
          type: boolean
          title: Selectable
          description: Whether the benefit is selectable when creating a product.
        deletable:
          type: boolean
          title: Deletable
          description: Whether the benefit is deletable.
        is_deleted:
          type: boolean
          title: Is Deleted
          description: Whether the benefit is deleted.
        organization_id:
          type: string
          format: uuid4
          title: Organization Id
          description: The ID of the organization owning the benefit.
        metadata:
          $ref: '#/components/schemas/MetadataOutputType'
        visibility:
          $ref: '#/components/schemas/BenefitVisibility'
          description: The visibility of the benefit in the customer portal.
        properties:
          $ref: '#/components/schemas/BenefitGitHubRepositoryProperties'
        visibility_configurable:
          type: boolean
          title: Visibility Configurable
          readOnly: true
      type: object
      required:
        - id
        - created_at
        - modified_at
        - type
        - description
        - selectable
        - deletable
        - is_deleted
        - organization_id
        - metadata
        - visibility
        - properties
        - visibility_configurable
      title: BenefitGitHubRepository
      description: >-
        A benefit of type `github_repository`.


        Use it to automatically invite your backers to a private GitHub
        repository.
    BenefitDownloadables:
      properties:
        id:
          type: string
          format: uuid4
          title: Id
          description: The ID of the benefit.
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
        type:
          type: string
          const: downloadables
          title: Type
        description:
          type: string
          title: Description
          description: The description of the benefit.
        selectable:
          type: boolean
          title: Selectable
          description: Whether the benefit is selectable when creating a product.
        deletable:
          type: boolean
          title: Deletable
          description: Whether the benefit is deletable.
        is_deleted:
          type: boolean
          title: Is Deleted
          description: Whether the benefit is deleted.
        organization_id:
          type: string
          format: uuid4
          title: Organization Id
          description: The ID of the organization owning the benefit.
        metadata:
          $ref: '#/components/schemas/MetadataOutputType'
        visibility:
          $ref: '#/components/schemas/BenefitVisibility'
          description: The visibility of the benefit in the customer portal.
        properties:
          $ref: '#/components/schemas/BenefitDownloadablesProperties'
        visibility_configurable:
          type: boolean
          title: Visibility Configurable
          readOnly: true
      type: object
      required:
        - id
        - created_at
        - modified_at
        - type
        - description
        - selectable
        - deletable
        - is_deleted
        - organization_id
        - metadata
        - visibility
        - properties
        - visibility_configurable
      title: BenefitDownloadables
    BenefitLicenseKeys:
      properties:
        id:
          type: string
          format: uuid4
          title: Id
          description: The ID of the benefit.
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
        type:
          type: string
          const: license_keys
          title: Type
        description:
          type: string
          title: Description
          description: The description of the benefit.
        selectable:
          type: boolean
          title: Selectable
          description: Whether the benefit is selectable when creating a product.
        deletable:
          type: boolean
          title: Deletable
          description: Whether the benefit is deletable.
        is_deleted:
          type: boolean
          title: Is Deleted
          description: Whether the benefit is deleted.
        organization_id:
          type: string
          format: uuid4
          title: Organization Id
          description: The ID of the organization owning the benefit.
        metadata:
          $ref: '#/components/schemas/MetadataOutputType'
        visibility:
          $ref: '#/components/schemas/BenefitVisibility'
          description: The visibility of the benefit in the customer portal.
        properties:
          $ref: '#/components/schemas/BenefitLicenseKeysProperties'
        visibility_configurable:
          type: boolean
          title: Visibility Configurable
          readOnly: true
      type: object
      required:
        - id
        - created_at
        - modified_at
        - type
        - description
        - selectable
        - deletable
        - is_deleted
        - organization_id
        - metadata
        - visibility
        - properties
        - visibility_configurable
      title: BenefitLicenseKeys
    BenefitMeterCredit:
      properties:
        id:
          type: string
          format: uuid4
          title: Id
          description: The ID of the benefit.
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
        type:
          type: string
          const: meter_credit
          title: Type
        description:
          type: string
          title: Description
          description: The description of the benefit.
        selectable:
          type: boolean
          title: Selectable
          description: Whether the benefit is selectable when creating a product.
        deletable:
          type: boolean
          title: Deletable
          description: Whether the benefit is deletable.
        is_deleted:
          type: boolean
          title: Is Deleted
          description: Whether the benefit is deleted.
        organization_id:
          type: string
          format: uuid4
          title: Organization Id
          description: The ID of the organization owning the benefit.
        metadata:
          $ref: '#/components/schemas/MetadataOutputType'
        visibility:
          $ref: '#/components/schemas/BenefitVisibility'
          description: The visibility of the benefit in the customer portal.
        properties:
          $ref: '#/components/schemas/BenefitMeterCreditProperties'
        visibility_configurable:
          type: boolean
          title: Visibility Configurable
          readOnly: true
      type: object
      required:
        - id
        - created_at
        - modified_at
        - type
        - description
        - selectable
        - deletable
        - is_deleted
        - organization_id
        - metadata
        - visibility
        - properties
        - visibility_configurable
      title: BenefitMeterCredit
      description: |-
        A benefit of type `meter_unit`.

        Use it to grant a number of units on a specific meter.
    BenefitFeatureFlag:
      properties:
        id:
          type: string
          format: uuid4
          title: Id
          description: The ID of the benefit.
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
        type:
          type: string
          const: feature_flag
          title: Type
        description:
          type: string
          title: Description
          description: The description of the benefit.
        selectable:
          type: boolean
          title: Selectable
          description: Whether the benefit is selectable when creating a product.
        deletable:
          type: boolean
          title: Deletable
          description: Whether the benefit is deletable.
        is_deleted:
          type: boolean
          title: Is Deleted
          description: Whether the benefit is deleted.
        organization_id:
          type: string
          format: uuid4
          title: Organization Id
          description: The ID of the organization owning the benefit.
        metadata:
          $ref: '#/components/schemas/MetadataOutputType'
        visibility:
          $ref: '#/components/schemas/BenefitVisibility'
          description: The visibility of the benefit in the customer portal.
        properties:
          $ref: '#/components/schemas/BenefitFeatureFlagProperties'
        visibility_configurable:
          type: boolean
          title: Visibility Configurable
          readOnly: true
      type: object
      required:
        - id
        - created_at
        - modified_at
        - type
        - description
        - selectable
        - deletable
        - is_deleted
        - organization_id
        - metadata
        - visibility
        - properties
        - visibility_configurable
      title: BenefitFeatureFlag
      description: |-
        A benefit of type `feature_flag`.

        Use it to grant feature flags with key-value metadata
        that can be queried via the API and webhooks.
    BenefitSlackSharedChannel:
      properties:
        id:
          type: string
          format: uuid4
          title: Id
          description: The ID of the benefit.
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
        type:
          type: string
          const: slack_shared_channel
          title: Type
        description:
          type: string
          title: Description
          description: The description of the benefit.
        selectable:
          type: boolean
          title: Selectable
          description: Whether the benefit is selectable when creating a product.
        deletable:
          type: boolean
          title: Deletable
          description: Whether the benefit is deletable.
        is_deleted:
          type: boolean
          title: Is Deleted
          description: Whether the benefit is deleted.
        organization_id:
          type: string
          format: uuid4
          title: Organization Id
          description: The ID of the organization owning the benefit.
        metadata:
          $ref: '#/components/schemas/MetadataOutputType'
        visibility:
          $ref: '#/components/schemas/BenefitVisibility'
          description: The visibility of the benefit in the customer portal.
        properties:
          $ref: '#/components/schemas/BenefitSlackSharedChannelProperties'
        visibility_configurable:
          type: boolean
          title: Visibility Configurable
          readOnly: true
      type: object
      required:
        - id
        - created_at
        - modified_at
        - type
        - description
        - selectable
        - deletable
        - is_deleted
        - organization_id
        - metadata
        - visibility
        - properties
        - visibility_configurable
      title: BenefitSlackSharedChannel
    CustomField:
      oneOf:
        - $ref: '#/components/schemas/CustomFieldText'
        - $ref: '#/components/schemas/CustomFieldNumber'
        - $ref: '#/components/schemas/CustomFieldDate'
        - $ref: '#/components/schemas/CustomFieldCheckbox'
        - $ref: '#/components/schemas/CustomFieldSelect'
      discriminator:
        propertyName: type
        mapping:
          checkbox:
            $ref: '#/components/schemas/CustomFieldCheckbox'
          date:
            $ref: '#/components/schemas/CustomFieldDate'
          number:
            $ref: '#/components/schemas/CustomFieldNumber'
          select:
            $ref: '#/components/schemas/CustomFieldSelect'
          text:
            $ref: '#/components/schemas/CustomFieldText'
    ProductPriceSource:
      type: string
      enum:
        - catalog
        - ad_hoc
      title: ProductPriceSource
    TaxBehaviorOption:
      type: string
      enum:
        - location
        - inclusive
        - exclusive
      title: TaxBehaviorOption
    ProductPriceSeatTiers-Output:
      properties:
        seat_tier_type:
          $ref: '#/components/schemas/SeatTierType'
          description: >-
            How tiers are applied. 'volume' prices all seats at the matching
            tier's rate. 'graduated' prices each tier's range independently.
          default: volume
        tiers:
          items:
            $ref: '#/components/schemas/ProductPriceSeatTier'
          type: array
          minItems: 1
          title: Tiers
          description: List of pricing tiers
        minimum_seats:
          type: integer
          title: Minimum Seats
          description: >-
            Minimum number of seats required for purchase, derived from first
            tier.
          readOnly: true
        maximum_seats:
          anyOf:
            - type: integer
            - type: 'null'
          title: Maximum Seats
          description: >-
            Maximum number of seats allowed for purchase, derived from last
            tier. None for unlimited.
          readOnly: true
      type: object
      required:
        - tiers
        - minimum_seats
        - maximum_seats
      title: ProductPriceSeatTiers
      description: |-
        List of pricing tiers for seat-based pricing.

        The minimum and maximum seat limits are derived from the tiers:
        - minimum_seats = first tier's min_seats
        - maximum_seats = last tier's max_seats (None for unlimited)
    Tiers:
      properties:
        type:
          $ref: '#/components/schemas/TierType'
        tiers:
          items:
            $ref: '#/components/schemas/Tier'
          type: array
          minItems: 1
          title: Tiers
      type: object
      required:
        - type
        - tiers
      title: Tiers
      description: |-
        The structure of the shared tiers JSONB column, used by every tiered
        price type. Purchasable quantity bounds live in the `minimum_units` and
        `maximum_units` columns, not here.
    ProductPriceMeter:
      properties:
        id:
          type: string
          format: uuid4
          title: Id
          description: The ID of the object.
        name:
          type: string
          title: Name
          description: The name of the meter.
        unit:
          $ref: '#/components/schemas/MeterUnit'
          description: The unit of the meter.
        custom_label:
          anyOf:
            - type: string
            - type: 'null'
          title: Custom Label
          description: The label for the custom unit.
        custom_multiplier:
          anyOf:
            - type: integer
            - type: 'null'
          title: Custom Multiplier
          description: The multiplier to convert from base unit to display scale.
      type: object
      required:
        - id
        - name
        - unit
        - custom_label
        - custom_multiplier
      title: ProductPriceMeter
      description: A meter associated to a metered price.
    MeterUnit:
      type: string
      enum:
        - scalar
        - token
        - custom
      title: MeterUnit
    Filter:
      properties:
        conjunction:
          $ref: '#/components/schemas/FilterConjunction'
        clauses:
          items:
            anyOf:
              - $ref: '#/components/schemas/FilterClause'
              - $ref: '#/components/schemas/Filter'
          type: array
          title: Clauses
      type: object
      required:
        - conjunction
        - clauses
      title: Filter
    CountAggregation:
      properties:
        func:
          type: string
          const: count
          title: Func
          default: count
      type: object
      title: CountAggregation
    PropertyAggregation:
      properties:
        func:
          type: string
          enum:
            - sum
            - max
            - min
            - avg
          title: Func
        property:
          type: string
          title: Property
      type: object
      required:
        - func
        - property
      title: PropertyAggregation
    UniqueAggregation:
      properties:
        func:
          type: string
          const: unique
          title: Func
          default: unique
        property:
          type: string
          title: Property
      type: object
      required:
        - property
      title: UniqueAggregation
    BenefitVisibility:
      type: string
      enum:
        - draft
        - private
        - public
      title: Visibility
    BenefitCustomProperties:
      properties:
        note:
          anyOf:
            - anyOf:
                - type: string
                - type: 'null'
              description: >-
                Private note to be shared with customers who have this benefit
                granted.
            - type: 'null'
          title: Note
      type: object
      required:
        - note
      title: BenefitCustomProperties
      description: Properties for a benefit of type `custom`.
    BenefitDiscordProperties:
      properties:
        guild_id:
          type: string
          title: Guild Id
          description: The ID of the Discord server.
        role_id:
          type: string
          title: Role Id
          description: The ID of the Discord role to grant.
        kick_member:
          type: boolean
          title: Kick Member
          description: Whether to kick the member from the Discord server on revocation.
        guild_token:
          type: string
          title: Guild Token
          readOnly: true
      type: object
      required:
        - guild_id
        - role_id
        - kick_member
        - guild_token
      title: BenefitDiscordProperties
      description: Properties for a benefit of type `discord`.
    BenefitGitHubRepositoryProperties:
      properties:
        repository_owner:
          type: string
          title: Repository Owner
          description: The owner of the repository.
          examples:
            - polarsource
        repository_name:
          type: string
          title: Repository Name
          description: The name of the repository.
          examples:
            - private_repo
        permission:
          type: string
          enum:
            - pull
            - triage
            - push
            - maintain
            - admin
          title: Permission
          description: >-
            The permission level to grant. Read more about roles and their
            permissions on [GitHub
            documentation](https://docs.github.com/en/organizations/managing-user-access-to-your-organizations-repositories/managing-repository-roles/repository-roles-for-an-organization#permissions-for-each-role).
      type: object
      required:
        - repository_owner
        - repository_name
        - permission
      title: BenefitGitHubRepositoryProperties
      description: Properties for a benefit of type `github_repository`.
    BenefitDownloadablesProperties:
      properties:
        archived:
          additionalProperties:
            type: boolean
          propertyNames:
            format: uuid4
          type: object
          title: Archived
        files:
          items:
            type: string
            format: uuid4
          type: array
          title: Files
      type: object
      required:
        - archived
        - files
      title: BenefitDownloadablesProperties
    BenefitLicenseKeysProperties:
      properties:
        prefix:
          anyOf:
            - type: string
            - type: 'null'
          title: Prefix
        expires:
          anyOf:
            - $ref: '#/components/schemas/BenefitLicenseKeyExpirationProperties'
            - type: 'null'
        activations:
          anyOf:
            - $ref: '#/components/schemas/BenefitLicenseKeyActivationProperties'
            - type: 'null'
        limit_usage:
          anyOf:
            - type: integer
            - type: 'null'
          title: Limit Usage
      type: object
      required:
        - prefix
        - expires
        - activations
        - limit_usage
      title: BenefitLicenseKeysProperties
    BenefitMeterCreditProperties:
      properties:
        units:
          type: integer
          title: Units
        rollover:
          type: boolean
          title: Rollover
        meter_id:
          type: string
          format: uuid4
          title: Meter Id
      type: object
      required:
        - units
        - rollover
        - meter_id
      title: BenefitMeterCreditProperties
      description: Properties for a benefit of type `meter_unit`.
    BenefitFeatureFlagProperties:
      properties: {}
      type: object
      title: BenefitFeatureFlagProperties
      description: Properties for a benefit of type `feature_flag`.
    BenefitSlackSharedChannelProperties:
      properties:
        slack_integration_id:
          type: string
          format: uuid4
          title: Slack Integration Id
          description: Polar Slack integration linked to this benefit.
        channel_name_template:
          type: string
          maxLength: 80
          minLength: 1
          title: Channel Name Template
          description: >-
            Template for the channel name. Supports placeholders:
            {customer_name}, {customer_email_local}, and {metadata.<key>} for
            any value stored in customer user metadata.
        private:
          type: boolean
          title: Private
          description: Create the channel as private (recommended).
          default: true
        welcome_message:
          anyOf:
            - type: string
              maxLength: 4000
            - type: 'null'
          title: Welcome Message
          description: Optional message posted to the channel right after creation.
        archive_on_revoke:
          type: boolean
          title: Archive On Revoke
          description: Archive the channel when the benefit is revoked.
          default: true
        team_invitees:
          items:
            type: string
          type: array
          title: Team Invitees
          description: >-
            Slack user IDs from the merchant workspace to invite to every
            channel created for this benefit.
      type: object
      required:
        - slack_integration_id
        - channel_name_template
      title: BenefitSlackSharedChannelProperties
    CustomFieldText:
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
          format: uuid4
          title: Id
          description: The ID of the object.
        metadata:
          $ref: '#/components/schemas/MetadataOutputType'
        type:
          type: string
          const: text
          title: Type
        slug:
          type: string
          title: Slug
          description: >-
            Identifier of the custom field. It'll be used as key when storing
            the value.
        name:
          type: string
          title: Name
          description: Name of the custom field.
        organization_id:
          type: string
          format: uuid4
          title: Organization Id
          description: The ID of the organization owning the custom field.
          examples:
            - 1dbfc517-0bbf-4301-9ba8-555ca42b9737
        properties:
          $ref: '#/components/schemas/CustomFieldTextProperties'
      type: object
      required:
        - created_at
        - modified_at
        - id
        - metadata
        - type
        - slug
        - name
        - organization_id
        - properties
      title: CustomFieldText
      description: Schema for a custom field of type text.
    CustomFieldNumber:
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
          format: uuid4
          title: Id
          description: The ID of the object.
        metadata:
          $ref: '#/components/schemas/MetadataOutputType'
        type:
          type: string
          const: number
          title: Type
        slug:
          type: string
          title: Slug
          description: >-
            Identifier of the custom field. It'll be used as key when storing
            the value.
        name:
          type: string
          title: Name
          description: Name of the custom field.
        organization_id:
          type: string
          format: uuid4
          title: Organization Id
          description: The ID of the organization owning the custom field.
          examples:
            - 1dbfc517-0bbf-4301-9ba8-555ca42b9737
        properties:
          $ref: '#/components/schemas/CustomFieldNumberProperties'
      type: object
      required:
        - created_at
        - modified_at
        - id
        - metadata
        - type
        - slug
        - name
        - organization_id
        - properties
      title: CustomFieldNumber
      description: Schema for a custom field of type number.
    CustomFieldDate:
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
          format: uuid4
          title: Id
          description: The ID of the object.
        metadata:
          $ref: '#/components/schemas/MetadataOutputType'
        type:
          type: string
          const: date
          title: Type
        slug:
          type: string
          title: Slug
          description: >-
            Identifier of the custom field. It'll be used as key when storing
            the value.
        name:
          type: string
          title: Name
          description: Name of the custom field.
        organization_id:
          type: string
          format: uuid4
          title: Organization Id
          description: The ID of the organization owning the custom field.
          examples:
            - 1dbfc517-0bbf-4301-9ba8-555ca42b9737
        properties:
          $ref: '#/components/schemas/CustomFieldDateProperties'
      type: object
      required:
        - created_at
        - modified_at
        - id
        - metadata
        - type
        - slug
        - name
        - organization_id
        - properties
      title: CustomFieldDate
      description: Schema for a custom field of type date.
    CustomFieldCheckbox:
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
          format: uuid4
          title: Id
          description: The ID of the object.
        metadata:
          $ref: '#/components/schemas/MetadataOutputType'
        type:
          type: string
          const: checkbox
          title: Type
        slug:
          type: string
          title: Slug
          description: >-
            Identifier of the custom field. It'll be used as key when storing
            the value.
        name:
          type: string
          title: Name
          description: Name of the custom field.
        organization_id:
          type: string
          format: uuid4
          title: Organization Id
          description: The ID of the organization owning the custom field.
          examples:
            - 1dbfc517-0bbf-4301-9ba8-555ca42b9737
        properties:
          $ref: '#/components/schemas/CustomFieldCheckboxProperties'
      type: object
      required:
        - created_at
        - modified_at
        - id
        - metadata
        - type
        - slug
        - name
        - organization_id
        - properties
      title: CustomFieldCheckbox
      description: Schema for a custom field of type checkbox.
    CustomFieldSelect:
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
          format: uuid4
          title: Id
          description: The ID of the object.
        metadata:
          $ref: '#/components/schemas/MetadataOutputType'
        type:
          type: string
          const: select
          title: Type
        slug:
          type: string
          title: Slug
          description: >-
            Identifier of the custom field. It'll be used as key when storing
            the value.
        name:
          type: string
          title: Name
          description: Name of the custom field.
        organization_id:
          type: string
          format: uuid4
          title: Organization Id
          description: The ID of the organization owning the custom field.
          examples:
            - 1dbfc517-0bbf-4301-9ba8-555ca42b9737
        properties:
          $ref: '#/components/schemas/CustomFieldSelectProperties'
      type: object
      required:
        - created_at
        - modified_at
        - id
        - metadata
        - type
        - slug
        - name
        - organization_id
        - properties
      title: CustomFieldSelect
      description: Schema for a custom field of type select.
    SeatTierType:
      type: string
      enum:
        - volume
        - graduated
      title: SeatTierType
    ProductPriceSeatTier:
      properties:
        min_seats:
          type: integer
          minimum: 1
          title: Min Seats
          description: Minimum number of seats (inclusive)
        max_seats:
          anyOf:
            - type: integer
              minimum: 1
            - type: 'null'
          title: Max Seats
          description: Maximum number of seats (inclusive). None for unlimited.
        price_per_seat:
          type: integer
          maximum: 9223372036854776000
          minimum: 0
          title: Price Per Seat
          description: Price per seat in cents for this tier
      type: object
      required:
        - min_seats
        - price_per_seat
      title: ProductPriceSeatTier
      description: A pricing tier for seat-based pricing.
    TierType:
      type: string
      enum:
        - volume
        - graduated
      title: TierType
    Tier:
      properties:
        bound:
          anyOf:
            - type: integer
              exclusiveMinimum: 0
            - type: 'null'
          title: Bound
        unit_amount:
          type: string
          pattern: ^(?!^[-+.]*$)[+-]?0*\d*\.?\d*$
          title: Unit Amount
      type: object
      required:
        - unit_amount
      title: Tier
      description: |-
        A per-unit rate up to and including `bound`.

        Each tier starts where the previous one ended. The first starts at
        zero. `bound` is None on the last tier if it's unbounded. Rates are
        in cents and may be fractional.

        Rates carry no precision bound: this schema reads stored rows, and a
        bound tightened later would stop them loading. `TierInput` holds the
        rules new rates must meet.
    FilterConjunction:
      type: string
      enum:
        - and
        - or
      title: FilterConjunction
    FilterClause:
      properties:
        property:
          type: string
          title: Property
        operator:
          $ref: '#/components/schemas/FilterOperator'
        value:
          anyOf:
            - type: string
              maxLength: 1000
            - type: integer
              maximum: 2147483647
              minimum: -2147483648
            - type: boolean
          title: Value
      type: object
      required:
        - property
        - operator
        - value
      title: FilterClause
    BenefitLicenseKeyExpirationProperties:
      properties:
        ttl:
          type: integer
          exclusiveMinimum: 0
          title: Ttl
        timeframe:
          type: string
          enum:
            - year
            - month
            - day
          title: Timeframe
      type: object
      required:
        - ttl
        - timeframe
      title: BenefitLicenseKeyExpirationProperties
    BenefitLicenseKeyActivationProperties:
      properties:
        limit:
          type: integer
          title: Limit
        enable_customer_admin:
          type: boolean
          title: Enable Customer Admin
      type: object
      required:
        - limit
        - enable_customer_admin
      title: BenefitLicenseKeyActivationProperties
    CustomFieldTextProperties:
      properties:
        form_label:
          type: string
          minLength: 1
          title: Form Label
        form_help_text:
          type: string
          minLength: 1
          title: Form Help Text
        form_placeholder:
          type: string
          minLength: 1
          title: Form Placeholder
        textarea:
          type: boolean
          title: Textarea
        min_length:
          type: integer
          maximum: 2147483647
          minimum: 0
          title: Min Length
        max_length:
          type: integer
          maximum: 2147483647
          minimum: 0
          title: Max Length
      type: object
      title: CustomFieldTextProperties
    CustomFieldNumberProperties:
      properties:
        form_label:
          type: string
          minLength: 1
          title: Form Label
        form_help_text:
          type: string
          minLength: 1
          title: Form Help Text
        form_placeholder:
          type: string
          minLength: 1
          title: Form Placeholder
        ge:
          type: integer
          maximum: 2147483647
          minimum: -2147483648
          title: Ge
        le:
          type: integer
          maximum: 2147483647
          minimum: -2147483648
          title: Le
      type: object
      title: CustomFieldNumberProperties
    CustomFieldDateProperties:
      properties:
        form_label:
          type: string
          minLength: 1
          title: Form Label
        form_help_text:
          type: string
          minLength: 1
          title: Form Help Text
        form_placeholder:
          type: string
          minLength: 1
          title: Form Placeholder
        ge:
          type: integer
          maximum: 2147483647
          minimum: -2147483648
          title: Ge
        le:
          type: integer
          maximum: 2147483647
          minimum: -2147483648
          title: Le
      type: object
      title: CustomFieldDateProperties
    CustomFieldCheckboxProperties:
      properties:
        form_label:
          type: string
          minLength: 1
          title: Form Label
        form_help_text:
          type: string
          minLength: 1
          title: Form Help Text
        form_placeholder:
          type: string
          minLength: 1
          title: Form Placeholder
      type: object
      title: CustomFieldCheckboxProperties
    CustomFieldSelectProperties:
      properties:
        form_label:
          type: string
          minLength: 1
          title: Form Label
        form_help_text:
          type: string
          minLength: 1
          title: Form Help Text
        form_placeholder:
          type: string
          minLength: 1
          title: Form Placeholder
        options:
          items:
            $ref: '#/components/schemas/CustomFieldSelectOption'
          type: array
          minItems: 1
          title: Options
      type: object
      required:
        - options
      title: CustomFieldSelectProperties
    FilterOperator:
      type: string
      enum:
        - eq
        - ne
        - gt
        - gte
        - lt
        - lte
        - like
        - not_like
      title: FilterOperator
    CustomFieldSelectOption:
      properties:
        value:
          type: string
          minLength: 1
          title: Value
        label:
          type: string
          minLength: 1
          title: Label
      type: object
      required:
        - value
        - label
      title: CustomFieldSelectOption
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
