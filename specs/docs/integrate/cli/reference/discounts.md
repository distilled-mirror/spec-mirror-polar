> ## Documentation Index
> Fetch the complete documentation index at: https://polar.sh/docs/llms.txt
> Use this file to discover all available pages before exploring further.

# polar discounts

```bash theme={null}
polar discounts <subcommand> [flags]
```

**Subcommands**

* [`polar discounts create`](#polar-discounts-create) Create a discount.
* [`polar discounts delete`](#polar-discounts-delete) Delete a discount.
* [`polar discounts get`](#polar-discounts-get) Get a discount by ID.
* [`polar discounts list`](#polar-discounts-list) List discounts.
* [`polar discounts update`](#polar-discounts-update) Update a discount.

## polar discounts create

Create a discount.

```bash theme={null}
polar discounts create [flags]
```

**Flags**

| Flag | Type | Description |
| - | - | - |
| `--data`, `-d` | `string` | JSON object; explicitly supplied flags override its top-level keys |
| `--metadata` | `string` | Key-value object allowing you to store additional information. JSON: \{"\<key>": string \| integer \| number \| boolean} |
| `--name` | `string` | Required. Name of the discount. Will be displayed to the customer when the discount is applied. |
| `--code` | `string` | Code customers can use to apply the discount during checkout. Must be between 3 and 256 characters long and contain only alphanumeric characters.If not provided, the discount can only be applied via the API. |
| `--starts-at` | `string` | Optional timestamp after which the discount is redeemable. |
| `--ends-at` | `string` | Optional timestamp after which the discount is no longer redeemable. |
| `--max-redemptions` | `integer` | Optional maximum number of times the discount can be redeemed. |
| `--max-redemptions-per-customer` | `integer` | Optional maximum number of times the discount can be redeemed by a single customer. |
| `--products` | `string` | products |
| `--organization-id`, `--org` | `string` | The ID of the organization owning the discount. Defaults to the active organization. |
| `--type` | `choice` | type (choices: fixed, percentage) |
| `--duration` | `choice` | Required. For subscriptions, determines if the discount should be applied once on the first invoice, forever, or for a certain number of months determined by `duration_in_months`. (choices: once, forever, repeating) |
| `--duration-in-months` | `integer` | Number of months the discount should be applied. |
| `--amount` | `integer` | amount |
| `--currency` | `choice` | currency (choices: aed, all, amd, aoa, ars, aud, awg, azn, bam, bbd, bdt, bif, bmd, bnd, bob, brl, bsd, bwp, bzd, cad, cdf, chf, clp, cny, cop, crc, cve, czk, djf, dkk, dop, dzd, egp, etb, eur, fjd, fkp, gbp, gel, gip, gmd, gnf, gtq, gyd, hkd, hnl, htg, huf, idr, ils, inr, isk, jmd, jpy, kes, kgs, khr, kmf, krw, kyd, kzt, lak, lkr, lrd, lsl, mad, mdl, mga, mkd, mnt, mop, mur, mvr, mwk, mxn, myr, mzn, nad, ngn, nio, nok, npr, nzd, pab, pen, pgk, php, pkr, pln, pyg, qar, ron, rsd, rwf, sar, sbd, scr, sek, sgd, shp, sos, srd, szl, thb, tjs, top, try, ttd, twd, tzs, uah, ugx, usd, uyu, uzs, vnd, vuv, wst, xaf, xcd, xcg, xof, xpf, yer, zar, zmw) |
| `--amounts` | `string` | amounts JSON: \{"\<key>": integer} |
| `--basis-points` | `integer` | Discount percentage in basis points. |

## polar discounts delete

Delete a discount.

```bash theme={null}
polar discounts delete [flags] <id>
```

**Arguments**

| Argument | Type | Description |
| - | - | - |
| `id` | `string` | |

**Flags**

| Flag | Type | Description |
| - | - | - |
| `--confirm`, `-c` | `boolean` | Skip the confirmation prompt for destructive requests |

## polar discounts get

Get a discount by ID.

```bash theme={null}
polar discounts get [flags] <id>
```

**Arguments**

| Argument | Type | Description |
| - | - | - |
| `id` | `string` | |

## polar discounts list

List discounts.

```bash theme={null}
polar discounts list [flags]
```

**Flags**

| Flag | Type | Description |
| - | - | - |
| `--data`, `-d` | `string` | JSON object; explicitly supplied flags override its top-level keys |
| `--organization-id`, `--org` | `string` | Filter by organization ID. Defaults to the active organization. |
| `--query` | `string` | Filter by name. |
| `--page` | `integer` | Page number, defaults to 1. |
| `--limit` | `integer` | Size of a page, defaults to 10. Maximum is 100. |
| `--sorting` | `choice` | Sorting criterion. Several criteria can be used simultaneously and will be applied in order. Add a minus sign `-` before the criteria name to sort by descending order. (choices: created\_at, -created\_at, name, -name, code, -code, redemptions\_count, -redemptions\_count, ends\_at, -ends\_at) |

## polar discounts update

Update a discount.

```bash theme={null}
polar discounts update [flags] <id>
```

**Arguments**

| Argument | Type | Description |
| - | - | - |
| `id` | `string` | |

**Flags**

| Flag | Type | Description |
| - | - | - |
| `--data`, `-d` | `string` | JSON object; explicitly supplied flags override its top-level keys |
| `--metadata` | `string` | Key-value object allowing you to store additional information. JSON: \{"\<key>": string \| integer \| number \| boolean} |
| `--name` | `string` | name |
| `--code` | `string` | Code customers can use to apply the discount during checkout. Must be between 3 and 256 characters long and contain only alphanumeric characters.If not provided, the discount can only be applied via the API. |
| `--starts-at` | `string` | Optional timestamp after which the discount is redeemable. |
| `--ends-at` | `string` | Optional timestamp after which the discount is no longer redeemable. |
| `--max-redemptions` | `integer` | Optional maximum number of times the discount can be redeemed. |
| `--max-redemptions-per-customer` | `integer` | Optional maximum number of times the discount can be redeemed by a single customer. |
| `--duration` | `choice` | duration (choices: once, forever, repeating) |
| `--duration-in-months` | `integer` | duration\_in\_months |
| `--type` | `choice` | type (choices: fixed, percentage) |
| `--amount` | `integer` | amount |
| `--currency` | `choice` | currency (choices: aed, all, amd, aoa, ars, aud, awg, azn, bam, bbd, bdt, bif, bmd, bnd, bob, brl, bsd, bwp, bzd, cad, cdf, chf, clp, cny, cop, crc, cve, czk, djf, dkk, dop, dzd, egp, etb, eur, fjd, fkp, gbp, gel, gip, gmd, gnf, gtq, gyd, hkd, hnl, htg, huf, idr, ils, inr, isk, jmd, jpy, kes, kgs, khr, kmf, krw, kyd, kzt, lak, lkr, lrd, lsl, mad, mdl, mga, mkd, mnt, mop, mur, mvr, mwk, mxn, myr, mzn, nad, ngn, nio, nok, npr, nzd, pab, pen, pgk, php, pkr, pln, pyg, qar, ron, rsd, rwf, sar, sbd, scr, sek, sgd, shp, sos, srd, szl, thb, tjs, top, try, ttd, twd, tzs, uah, ugx, usd, uyu, uzs, vnd, vuv, wst, xaf, xcd, xcg, xof, xpf, yer, zar, zmw) |
| `--amounts` | `string` | amounts JSON: \{"\<key>": integer} |
| `--basis-points` | `integer` | basis\_points |
| `--products` | `string` | products |


This documentation is built and hosted on [Mintlify](https://mintlify.com), a developer documentation platform.
