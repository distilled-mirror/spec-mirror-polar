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

* [`polar auth`](/docs/integrate/cli/reference/auth) Manage your Polar sessions and active organization
* [`polar listen`](/docs/integrate/cli/reference/listen) Forward webhook events for an organization to a local URL
* [`polar trigger`](/docs/integrate/cli/reference/trigger) Send a sample webhook event to your local server
* [`polar update`](/docs/integrate/cli/reference/update) Update the CLI to the latest release

## API resources

* [`polar benefit_grants`](/docs/integrate/cli/reference/benefit_grants)
* [`polar benefits`](/docs/integrate/cli/reference/benefits)
* [`polar checkout_links`](/docs/integrate/cli/reference/checkout_links)
* [`polar checkouts`](/docs/integrate/cli/reference/checkouts)
* [`polar custom_fields`](/docs/integrate/cli/reference/custom_fields)
* [`polar customer_meters`](/docs/integrate/cli/reference/customer_meters)
* [`polar customer_seats`](/docs/integrate/cli/reference/customer_seats)
* [`polar customers`](/docs/integrate/cli/reference/customers)
* [`polar discounts`](/docs/integrate/cli/reference/discounts)
* [`polar event_types`](/docs/integrate/cli/reference/event_types)
* [`polar events`](/docs/integrate/cli/reference/events)
* [`polar files`](/docs/integrate/cli/reference/files)
* [`polar license_keys`](/docs/integrate/cli/reference/license_keys)
* [`polar meters`](/docs/integrate/cli/reference/meters)
* [`polar metrics`](/docs/integrate/cli/reference/metrics)
* [`polar orders`](/docs/integrate/cli/reference/orders)
* [`polar organizations`](/docs/integrate/cli/reference/organizations)
* [`polar payments`](/docs/integrate/cli/reference/payments)
* [`polar products`](/docs/integrate/cli/reference/products)
* [`polar refunds`](/docs/integrate/cli/reference/refunds)
* [`polar subscriptions`](/docs/integrate/cli/reference/subscriptions)
* [`polar webhooks`](/docs/integrate/cli/reference/webhooks)

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
