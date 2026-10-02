> ## Documentation Index
> Fetch the complete documentation index at: https://polar.sh/docs/llms.txt
> Use this file to discover all available pages before exploring further.

# polar auth

> Manage your Polar sessions and active organization

```bash theme={null}
polar auth <subcommand> [flags]
```

**Subcommands**

* [`polar auth login`](#polar-auth-login) Sign in to Polar through your browser
* [`polar auth whoami`](#polar-auth-whoami) Show the active organization
* [`polar auth list`](#polar-auth-list) List the organizations you have access to
* [`polar auth org`](#polar-auth-org) Choose the active organization
* [`polar auth logout`](#polar-auth-logout) Sign out and remove saved sessions

## polar auth login

Sign in to Polar through your browser

```bash theme={null}
polar auth login [flags]
```

**Flags**

| Flag | Type | Description |
| - | - | - |
| `--sandbox` | `boolean` | Use the sandbox environment |
| `--production` | `boolean` | Use the production environment |
| `--new-session` | `boolean` | Sign in again even if a session is already saved |

**Examples**

Choose sandbox or production, then sign in

```bash theme={null}
polar auth login
```

Sign in to sandbox

```bash theme={null}
polar auth login --sandbox
```

Sign in to production again, replacing the saved session

```bash theme={null}
polar auth login --production --new-session
```

## polar auth whoami

Show the active organization

```bash theme={null}
polar auth whoami [flags]
```

**Flags**

| Flag | Type | Description |
| - | - | - |
| `--json` | `boolean` | Print the result as JSON |

## polar auth list

List the organizations you have access to

```bash theme={null}
polar auth list [flags]
```

**Flags**

| Flag | Type | Description |
| - | - | - |
| `--json` | `boolean` | Print the result as JSON |

## polar auth org

Choose the active organization

```bash theme={null}
polar auth org [flags]
```

## polar auth logout

Sign out and remove saved sessions

```bash theme={null}
polar auth logout [flags]
```

**Flags**

| Flag | Type | Description |
| - | - | - |
| `--sandbox` | `boolean` | Use the sandbox environment |
| `--production` | `boolean` | Use the production environment |
| `--all` | `boolean` | Remove every saved session |

**Examples**

Sign out, asking which session only if you have both

```bash theme={null}
polar auth logout
```

Remove every saved session

```bash theme={null}
polar auth logout --all
```


This documentation is built and hosted on [Mintlify](https://mintlify.com), a developer documentation platform.
