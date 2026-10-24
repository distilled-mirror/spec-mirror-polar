> ## Documentation Index
> Fetch the complete documentation index at: https://polar.sh/docs/llms.txt
> Use this file to discover all available pages before exploring further.

# Command reference

> Every command, argument and flag of the Polar CLI

Each command has its own page. Run any command with `--help` to see the same information in your terminal.

```bash theme={null}
polar <subcommand> [flags]
```

## CLI commands

* [`polar auth`](https://polar.sh/docs/integrate/cli/reference/auth) Manage your Polar sessions and active organization
* [`polar webhook`](https://polar.sh/docs/integrate/cli/reference/webhook) Test webhooks against your local server
* [`polar update`](https://polar.sh/docs/integrate/cli/reference/update) Update the CLI to the latest release

## API resources

* [`polar benefit_grants`](https://polar.sh/docs/integrate/cli/reference/benefit_grants)
* [`polar benefits`](https://polar.sh/docs/integrate/cli/reference/benefits)
* [`polar checkout_links`](https://polar.sh/docs/integrate/cli/reference/checkout_links)
* [`polar checkouts`](https://polar.sh/docs/integrate/cli/reference/checkouts)
* [`polar custom_fields`](https://polar.sh/docs/integrate/cli/reference/custom_fields)
* [`polar customer_meters`](https://polar.sh/docs/integrate/cli/reference/customer_meters)
* [`polar customer_seats`](https://polar.sh/docs/integrate/cli/reference/customer_seats)
* [`polar customers`](https://polar.sh/docs/integrate/cli/reference/customers)
* [`polar discounts`](https://polar.sh/docs/integrate/cli/reference/discounts)
* [`polar event_types`](https://polar.sh/docs/integrate/cli/reference/event_types)
* [`polar events`](https://polar.sh/docs/integrate/cli/reference/events)
* [`polar files`](https://polar.sh/docs/integrate/cli/reference/files)
* [`polar license_keys`](https://polar.sh/docs/integrate/cli/reference/license_keys)
* [`polar meters`](https://polar.sh/docs/integrate/cli/reference/meters)
* [`polar metrics`](https://polar.sh/docs/integrate/cli/reference/metrics)
* [`polar orders`](https://polar.sh/docs/integrate/cli/reference/orders)
* [`polar organizations`](https://polar.sh/docs/integrate/cli/reference/organizations)
* [`polar payments`](https://polar.sh/docs/integrate/cli/reference/payments)
* [`polar products`](https://polar.sh/docs/integrate/cli/reference/products)
* [`polar refunds`](https://polar.sh/docs/integrate/cli/reference/refunds)
* [`polar subscriptions`](https://polar.sh/docs/integrate/cli/reference/subscriptions)
* [`polar webhooks`](https://polar.sh/docs/integrate/cli/reference/webhooks)

## Global flags

These flags work with every command.

| Flag | Type | Description |
| - | - | - |
| `--help`, `-h` | `boolean` | Show help information |
| `--version`, `-v` | `boolean` | Show version information |
| `--completions` | `<bash\|zsh\|fish\|sh>` | Print shell completion script (choices: bash, zsh, fish, sh) |
| `--log-level` | `<all\|trace\|debug\|info\|warn\|warning\|error\|fatal\|none>` | Sets the minimum log level (choices: all, trace, debug, info, warn, warning, error, fatal, none) |
| `--json` | `boolean` | Print the result as JSON |


This documentation is built and hosted on [Mintlify](https://mintlify.com), a developer documentation platform.
