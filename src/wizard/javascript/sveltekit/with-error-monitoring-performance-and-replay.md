---
name: SvelteKit
doc_link: https://docs.sentry.io/platforms/javascript/guides/sveltekit/
support_level: production
type: framework
---

## Install

Configure your app automatically with the [Sentry wizard](https://docs.sentry.io/platforms/javascript/guides/sveltekit/#install).

```bash
npx @sentry/wizard -i sveltekit
```

## Configure

The Sentry wizard will automatically patch your application to configure the Sentry SDK:

- Create or update `src/hooks.client.js` and `src/hooks.server.js` with the default `Sentry.init` call and SvelteKit hooks handlers.
- Update `vite.config.js` to add source maps upload and auto-instrumentation via Vite plugins.
- create `.sentryclirc` and `sentry.properties` files with configuration for sentry-cli (which is used when automatically uploading source maps).

Alternatively, you can also [set up the SDK manually](https://docs.sentry.io/platforms/javascript/guides/sveltekit/manual-setup/).

**Configure the Sentry SDK**:

To configure the Sentry SDK on the client-side, edit the `Sentry.init` options in `hooks.client.(js|ts)`:

```javascript
import * as Sentry from "@sentry/sveltekit";

Sentry.init({
  dsn: "___PUBLIC_DSN___",
  // Performance Monitoring:
  tracesSampleRate: 1.0, // Capture 100% of the transactions. Adjust this value in production as necessary.
  // Session Replay
  integrations: [new Sentry.Replay()],
  replaysSessionSampleRate: 0.1, // This sets the Replay sample rate at 10%. You may want to change it to 100% while in development and then sample at a lower rate in production.
  replaysOnErrorSampleRate: 1.0, // If you're not already sampling the entire session, change the sample rate to 100% when sampling sessions where errors occur.
});
```

For the server-side, edit the `Sentry.init` options in `hooks.server.(js|ts)`:

```javascript
import * as Sentry from "@sentry/sveltekit";

Sentry.init({
  dsn: "___PUBLIC_DSN___",
  // Performance Monitoring
  tracesSampleRate: 1.0, // Capture 100% of the transactions. Adjust this value in production as necessary.
});
```

## Verify

This snippet contains an intentional error and can be used as a test to make sure that everything's working as expected.

```svelte
<!-- +page.svelte -->
<button type="button" on:click={unknownFunction}>Break the world</button>
```

---

## Next Steps

- [Source Maps](https://docs.sentry.io/platforms/javascript/guides/sveltekit/sourcemaps/): Learn how to enable readable stack traces in your Sentry errors.
