> ## Documentation Index
> Fetch the complete documentation index at: https://polar.sh/docs/llms.txt
> Use this file to discover all available pages before exploring further.

# Integrating Webhooks Locally

> Setup Webhook delivery to your local machine

### Install Polar CLI

macOS, Linux, WSL:

```bash theme={null}
curl -fsSL https://polar.sh/install.sh | bash
```

### Login to your account

This will allow you to authenticate with Polar.

```bash theme={null}
polar login
```

### Listen for Webhooks

```bash theme={null}
polar listen http://localhost:3000/
```

You will be prompted to select which Organization you want to listen for.

```bash theme={null}
✔ Select Organization …  My Organization

  Connected  My Organization
  Secret     6t3c8ce2247c493a3ade20uea4484d64
  Forwarding http://localhost:3000

  Waiting for events...
```

### Set the secret

Make sure that you copy the secret & set it in your environment variables.

If you don't set the correct secret, you'll see 403 errors if you use our Webhook utilities in your app.

```bash theme={null}
# .env
POLAR_WEBHOOK_SECRET=6t3c8ce2247c493a3ade20uea4484d64
```

### All set!

You're now fully setup. Webhooks will be tunneled via the CLI listen connection, and relayed to the specified target URL.
