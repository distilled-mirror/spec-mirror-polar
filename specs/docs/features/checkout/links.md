> ## Documentation Index
> Fetch the complete documentation index at: https://polar.sh/docs/llms.txt
> Use this file to discover all available pages before exploring further.

# Checkout Links

> Persistent URLs that create a Checkout Session on visit

This is the simplest way to start selling. Configure once in the dashboard, share the URL anywhere (your website, social media, email, a button in your app), and Polar handles the rest.

> [!TIP]
>
> If you need to create Checkout Sessions programmatically (e.g. with per-customer data computed by your backend), use the [Checkout API](/docs/features/checkout/session) directly instead.

## How it works

A Checkout Link is a long-lived URL tied to your organization. The link itself doesn't expire, and you can share it indefinitely.

When a customer visits the link:

1. Polar reads the link's configuration (products, preset discount, metadata, etc.).
2. A new, short-lived Checkout Session is created from that configuration.
3. The customer is redirected to the session's checkout page to complete the purchase.

Each visit produces a brand new Checkout Session. Always share the Checkout Link URL itself, never the URL of a generated Checkout Session, since those are temporary and will expire.

## Create a Checkout Link

Checkout Links are managed from the [**Checkout Links**](https://polar.sh/to/dashboard/products/checkout-links) page. Click on **New Link** to create one.

### Products

You can select one or several products. With several products, customers can switch between them on the checkout page before paying. Products are shown in the order you list them.

> [!NOTE]
>
> Customers always purchase a single product per checkout. Selecting multiple products on a Checkout Link gives the customer a choice between them; it doesn't bundle them together. True multi-product checkout (multiple products in the same order) isn't supported yet.

### Discount

You can preset a discount on the link. It will be **automatically applied** when the customer lands on the checkout page, with no action required from them. Useful for running promotions or offering special pricing through specific links.

If **Allow discount codes** is enabled, customers can still manually enter another discount code during checkout.

> [!TIP]
>
> Discounts without a code can only be applied through Checkout Link presets or the [Checkout API](/docs/features/checkout/session). Learn more in the [Discounts documentation](/docs/features/discounts).

### Success URL

URL where the customer is redirected after a successful payment. If left empty, the customer stays on the Polar-hosted confirmation page.

You can add the `checkout_id={CHECKOUT_ID}` query parameter to the Success URL and Polar will substitute it with the actual Checkout Session ID at redirect time. This is handy to fetch the order details from your backend on the confirmation page.

### Return URL

URL for the back button shown on the checkout page. When the customer clicks it, they're sent back to this URL (typically your pricing page or the previous page on your site). If not set, no back button is shown.

### Trial

If the link's products are subscriptions, you can configure a trial period directly on the Checkout Link. When set, this trial **overrides** the default trial configured on the products themselves, but only for sessions created from this link.

This is useful for running campaigns with longer (or shorter) trials than your default, without having to duplicate the product. If left empty, the product's own trial configuration is used.

### Seats

If the link's products use seat-based pricing, you can preconfigure a fixed number of seats. When set, the checkout session is locked to that seat count—the customer cannot change it. This is useful for creating links with a predetermined team size.

If the products no longer support the configured seat count when the link is opened (e.g., tier limits changed), the value is ignored and the customer can select freely.

### Metadata

An optional key-value object for storing extra information. Metadata set on the link is copied to the generated Checkout Session, and propagates to the resulting Order and/or Subscription on success.

## Query parameters

Append query parameters to a Checkout Link URL to override or extend its preset configuration on a per-visit basis.

### Preselect a product

When a link is configured with several products, point the customer directly to one of them using `product_id`. The customer can still switch products on the checkout page.

**product\_id** `string`

ID of the product to preselect. Must be one of the products configured on the Checkout Link.

### Prefill fields

**customer\_email** `string`

Prefill the customer email.

**customer\_name** `string`

Prefill the customer name.

**discount\_code** `string`

Prefill the discount code input. Only works with discounts that have a code set. See the [Discounts documentation](/docs/features/discounts) for details.

This is different from presetting a discount on the link. A preset discount is silently applied, whereas `discount_code` only fills the input field so the customer can see it.

**amount** `string`

Prefill the amount when the product uses Pay-What-You-Want pricing.

**locale** `string`

Force the checkout page language, given as an IETF BCP 47 language tag (e.g.
`en`, `fr`, `pt-BR`). If omitted, the language is detected from the
customer's browser. Checkout localization is in beta. See
[Checkout Localization](/docs/features/checkout/localization) for details.

**custom\_field\_data.\{slug}** `string`

Prefill custom field data, where `{slug}` is the slug of the custom field.

**theme** `string`

Force the checkout page theme. Accepts `light` or `dark`. If omitted, the
theme follows the customer's system preference.

### Attribution and reference metadata

These parameters are automatically attached to the generated Checkout Session [`metadata`](/docs/api-reference/current/checkouts/get-checkout-session#response-metadata).

**reference\_id** `string`

Your own reference ID for the Checkout Session.

**utm\_source** `string`

UTM source of the Checkout Session.

**utm\_medium** `string`

UTM medium of the Checkout Session.

**utm\_campaign** `string`

UTM campaign of the Checkout Session.

**utm\_content** `string`

UTM content of the Checkout Session.

**utm\_term** `string`

UTM term of the Checkout Session.


This documentation is built and hosted on [Mintlify](https://mintlify.com), a developer documentation platform.
