> ## Documentation Index
> Fetch the complete documentation index at: https://polar.sh/docs/llms.txt
> Use this file to discover all available pages before exploring further.

# polar update

> Update the CLI to the latest release

```bash theme={null}
polar update [flags]
```

**Flags**

| Flag | Type | Description |
| - | - | - |
| `--method` | `choice` | How the CLI was installed: binary (install.sh), npm, pnpm, bun, yarn or vp (Vite+). Detected when omitted (choices: binary, npm, pnpm, bun, yarn, vp) |

**Examples**

Update to the latest version of the CLI

```bash theme={null}
polar update
```

Update through npm instead of the detected install method

```bash theme={null}
polar update --method npm
```


This documentation is built and hosted on [Mintlify](https://mintlify.com), a developer documentation platform.
