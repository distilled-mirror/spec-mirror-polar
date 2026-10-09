> ## Documentation Index
> Fetch the complete documentation index at: https://polar.sh/docs/llms.txt
> Use this file to discover all available pages before exploring further.

# polar trigger

> Send a sample webhook event to your local server

```bash theme={null}
polar trigger [flags] [<event>]
```

**Arguments**

| Argument | Type | Description |
| - | - | - |
| `event` | `string` | Webhook event to send, e.g. order.created. Omit to pick from a list. (optional) |

**Flags**

| Flag | Type | Description |
| - | - | - |
| `--org` | `string` | Organization ID or slug for this invocation only |
| `--override` | `string` | Override a payload field, as path=value. Values are sent as text; use null, a JSON object, array or quoted string for other types. Repeatable. |
| `--seed` | `integer` | Seed for generated IDs, for a reproducible payload |
| `--dry-run` | `boolean` | Print the payload that would be sent, without sending it |
| `--list` | `boolean` | List every event you can trigger, with descriptions |

**Examples**

Pick an event from a list

```bash theme={null}
polar trigger
```

Send a sample order.paid event

```bash theme={null}
polar trigger order.paid
```

Change a field in the payload

```bash theme={null}
polar trigger order.paid --override data.customer.email=jane@example.com
```

Generate the same IDs every time

```bash theme={null}
polar trigger order.paid --seed 7
```

Print the payload instead of sending it

```bash theme={null}
polar trigger order.paid --dry-run
```


This documentation is built and hosted on [Mintlify](https://mintlify.com), a developer documentation platform.
