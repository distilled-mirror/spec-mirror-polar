> ## Documentation Index
> Fetch the complete documentation index at: https://polar.sh/docs/llms.txt
> Use this file to discover all available pages before exploring further.

# polar customer_meters

```bash theme={null}
polar customer_meters <subcommand> [flags]
```

**Subcommands**

* [`polar customer_meters get`](#polar-customer_meters-get) Get a customer meter by ID.
* [`polar customer_meters list`](#polar-customer_meters-list) List customer meters.

## polar customer\_meters get

Get a customer meter by ID.

```bash theme={null}
polar customer_meters get [flags] <id>
```

**Arguments**

| Argument | Type | Description |
| - | - | - |
| `id` | `string` | |

## polar customer\_meters list

List customer meters.

```bash theme={null}
polar customer_meters list [flags]
```

**Flags**

| Flag | Type | Description |
| - | - | - |
| `--data`, `-d` | `string` | JSON object; explicitly supplied flags override its top-level keys |
| `--organization-id`, `--org` | `string` | Filter by organization ID. Defaults to the active organization. |
| `--customer-id` | `string` | Filter by customer ID. |
| `--external-customer-id` | `string` | Filter by external customer ID. |
| `--meter-id` | `string` | Filter by meter ID. |
| `--page` | `integer` | Page number, defaults to 1. |
| `--limit` | `integer` | Size of a page, defaults to 10. Maximum is 100. |
| `--sorting` | `choice` | Sorting criterion. Several criteria can be used simultaneously and will be applied in order. Add a minus sign `-` before the criteria name to sort by descending order. (choices: created\_at, -created\_at, modified\_at, -modified\_at, customer\_id, -customer\_id, customer\_name, -customer\_name, meter\_id, -meter\_id, meter\_name, -meter\_name, consumed\_units, -consumed\_units, credited\_units, -credited\_units, balance, -balance) |


This documentation is built and hosted on [Mintlify](https://mintlify.com), a developer documentation platform.
