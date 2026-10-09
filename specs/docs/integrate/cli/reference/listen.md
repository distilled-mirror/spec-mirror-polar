> ## Documentation Index
> Fetch the complete documentation index at: https://polar.sh/docs/llms.txt
> Use this file to discover all available pages before exploring further.

# polar listen

> Forward webhook events for an organization to a local URL

```bash theme={null}
polar listen [flags] [<url>]
```

**Arguments**

| Argument | Type | Description |
| - | - | - |
| `url` | `string` | Where to forward webhook events: a port like 3000, or a URL like [http://localhost:3000/api/webhooks](http://localhost:3000/api/webhooks) (optional) |

**Flags**

| Flag | Type | Description |
| - | - | - |
| `--org` | `string` | Organization ID or slug for this invocation only |
| `--print-secret` | `boolean` | Print the secret that signs forwarded events, then exit |

**Examples**

Forward events to [http://localhost:3000](http://localhost:3000)

```bash theme={null}
polar listen 3000
```

Forward events to a route on your local server

```bash theme={null}
polar listen 3000/api/webhooks
```

Forward events for a specific organization

```bash theme={null}
polar listen http://localhost:3000/api/webhooks --org <id>
```

Print the signing secret, e.g. for POLAR\_WEBHOOK\_SECRET in .env

```bash theme={null}
polar listen --print-secret
```


This documentation is built and hosted on [Mintlify](https://mintlify.com), a developer documentation platform.
