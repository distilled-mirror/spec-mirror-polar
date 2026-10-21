> ## Documentation Index
> Fetch the complete documentation index at: https://polar.sh/docs/llms.txt
> Use this file to discover all available pages before exploring further.

# API Overview

> Official SDK quickstarts, base URLs, authentication, pagination, rate limits, and API concepts

> **Production Base URL**
>
> `https://api.polar.sh/v1`

> **Sandbox Base URL**
>
> `https://sandbox-api.polar.sh/v1`

> **[Auth (Organization)](/docs/integrate/oat)**
>
> Use an **Organization Access Token (OAT)** in the `Authorization: Bearer`
> header

> **[Auth (Customer Portal)](/docs/api-reference/2026-04/customer-sessions/create-customer-session)**
>
> Use a **Customer Access Token** created via `/v1/customer-sessions/`

## Official SDKs

Use our new, fully typed SDKs to integrate with the Polar API from TypeScript or Python.

Create an [organization access token](/docs/integrate/oat), then install the SDK and make your first request:

**TypeScript**

```bash npm theme={null}
npm install @polar-sh/sdk
```

```typescript app.ts theme={null}
import { createPolar } from "@polar-sh/sdk/2026-04";

const polar = createPolar({
  accessToken: process.env.POLAR_ACCESS_TOKEN!,
});

const customerState = await polar.customers.getStateExternal("customer_external_id");
console.log(customerState);
```

**Python**

```bash uv theme={null}
uv add polar-sdk
```

```bash pip theme={null}
pip install polar-sdk
```

```python main.py theme={null}
import os

from polar.v2026_04 import Polar

polar = Polar(os.environ["POLAR_ACCESS_TOKEN"])

customer_state = polar.customers.get_state_external("customer_external_id")
print(customer_state)
```

Both clients use production by default. Pass `environment="sandbox"` in Python or `environment: "sandbox"` in TypeScript to use the [sandbox environment](/docs/integrate/sandbox).

## Base URLs

| Environment | Base URL | Purpose |
| - | - | - |
| Production | `https://api.polar.sh/v1` | Real customers & live payments |
| Sandbox | `https://sandbox-api.polar.sh/v1` | Safe testing & integration work |

> [!NOTE]
>
> The sandbox environment is fully isolated—data, users, tokens, and
> organizations created there do not affect production. Create separate tokens
> in each environment.

Read more: [Sandbox Environment](/docs/integrate/sandbox)

## Authentication

### Organization Access Tokens (OAT)

Use an **OAT** to act on behalf of your organization (manage products, prices, checkouts, orders, subscriptions, benefits, etc.).

```http theme={null}
Authorization: Bearer polar_oat_xxxxxxxxxxxxxxxxx
```

> [!TIP]
>
> Create OATs in your organization settings. See: [Organization Access
> Tokens](/docs/integrate/oat)

> [!WARNING]
>
> Never expose an OAT in client-side code, public repos, or logs. If leaked, it
> will be revoked automatically by our secret scanning integrations.

### Customer Access Tokens

Do **not** use OATs in the browser. For customer-facing flows, [generate a **Customer Session**](/docs/api-reference/2026-04/customer-sessions/create-customer-session) server-side, then use the returned **customer access token** with the **Customer Portal API** to let a signed-in customer view their own orders, subscriptions, and benefits.

## Core API vs Customer Portal API

| Aspect | Core API | Customer Portal API |
| - | - | - |
| Audience | Your server / backend | One of your customer |
| Auth Type | Organization Access Token (OAT) | Customer Access Token |
| Scope | Full org resources (products, orders, subscriptions, benefits, checkout) | Only the authenticated customer’s data |
| Typical Use | Admin dashboards, internal tools, automation, provisioning | Building a custom customer portal or gated app |
| Token Creation | Via dashboard (manual) | Via `/v1/customer-sessions/` (server-side) |
| Sensitive Operations | Yes (create/update products, issue refunds, etc.) | No (read/update only what the customer owns) |

> [!NOTE]
>
> The Customer Portal API is a *restricted* surface designed for safe exposure
> in user-facing contexts (after exchanging a session). It cannot perform
> privileged org-level mutations like creating products or issuing refunds.

## Quick Examples

```bash curl (Production - Core API) theme={null}
curl https://api.polar.sh/v1/products/ \
  -H "Authorization: Bearer $POLAR_OAT" \
  -H "Accept: application/json"
```

```bash curl (Sandbox - Core API) theme={null}
curl https://sandbox-api.polar.sh/v1/products/ \
  -H "Authorization: Bearer $POLAR_OAT_SANDBOX" \
  -H "Accept: application/json"
```

```bash curl (Customer Portal API) theme={null}
curl https://api.polar.sh/v1/customer-portal/orders/ \
  -H "Authorization: Bearer $POLAR_CUSTOMER_TOKEN" \
  -H "Accept: application/json"
```

## Pagination

List endpoints in the Polar API support pagination to help you efficiently retrieve large datasets. Use the `page` and `limit` query parameters to control pagination.

### Query Parameters

| Parameter | Type | Default | Max | Description |
| - | - | - | - | - |
| `page` | integer | `1` | - | Page number, starting from 1 |
| `limit` | integer | `10` | `100` | Number of items to return per page (window size) |

> [!NOTE]
>
> The `page` parameter works as a window offset. For example, `page=2&limit=10`
> means the API will skip the first 10 elements and return the next 10.

### Response Format

All paginated responses include a `pagination` object with metadata about the current page and total results:

| Field | Type | Description |
| - | - | - |
| `total_count` | integer | Total number of items matching your query across all pages |
| `max_page` | integer | Total number of pages available, given the current `limit` value |

### Example

Let's say you want to fetch products with a limit of 100 items per page:

```bash Request theme={null}
curl https://api.polar.sh/v1/products/?page=1&limit=100 \
  -H "Authorization: Bearer $POLAR_OAT" \
  -H "Accept: application/json"
```

```json Response theme={null}
{
  "items": [
    {
      "id": "...",
      "name": "Product 1",
      ...
    },
    ...
  ],
  "pagination": {
    "total_count": 250,
    "max_page": 3
  }
}
```

In this example:

* `total_count=250` indicates there are 250 total products
* `limit=100` means each page contains up to 100 products
* `max_page=3` means you need to make 3 requests to retrieve all products (pages 1, 2, and 3)

> [!TIP]
>
> To retrieve all pages, increment the `page` parameter from `1` to `max_page`.
> Our SDKs provide built-in pagination helpers to automatically iterate through
> all pages.

## Rate Limits

Polar API has rate limits to ensure fair usage and maintain performance. Limits differ between the **Sandbox** and **Production** environments.

### Production

* **500 requests per minute** per organization/customer or OAuth2 Client.

### Sandbox

* **100 requests per minute** per organization/customer or OAuth2 Client.

Unauthenticated [validation](/docs/api-reference/2026-04/customer_portal/validate-license-key), [activation](/docs/api-reference/2026-04/customer_portal/activate-license-key), and [deactivation](/docs/api-reference/2026-04/customer_portal/deactivate-license-key) endpoints are limited to **3 requests per second** in both environments.

If you exceed the rate limit, you will receive a `429 Too Many Requests` response. The response will include a `Retry-After` header indicating how long you should wait before making another request.

> [!NOTE]
>
> Organizations requiring higher rate limits for production workloads may
> contact our support team to discuss elevated limits.


This documentation is built and hosted on [Mintlify](https://mintlify.com), a developer documentation platform.
