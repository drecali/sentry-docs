---
name: Google Cloud Functions (Python)
doc_link: https://docs.sentry.io/platforms/python/guides/gcp-functions/
support_level: production
type: framework
---

Install our Python SDK using `pip`:

```bash
pip install --upgrade sentry-sdk
```

You can use the Google Cloud Functions integration for the Python SDK like this:

```python
import sentry_sdk
from sentry_sdk.integrations.gcp import GcpIntegration

sentry_sdk.init(
    dsn="___PUBLIC_DSN___",
    integrations=[
        GcpIntegration(),
    ],

    # Set traces_sample_rate to 1.0 to capture 100%
    # of transactions for performance monitoring.
    # We recommend adjusting this value in production,
    traces_sample_rate=1.0,
)

def http_function_entrypoint(request):
    ...
```

Check out Sentry's [GCP sample apps](https://github.com/getsentry/examples/tree/master/gcp-cloud-functions) for detailed examples.

### Timeout Warning

The timeout warning reports an issue when the function execution time is near
the [configured timeout](https://cloud.google.com/functions/docs/concepts/exec#timeout).

To enable the warning, update the SDK initialization to set `timeout_warning` to
`true`:

```python
sentry_sdk.init(
    dsn="___PUBLIC_DSN___",
    integrations=[
        GcpIntegration(timeout_warning=True),
    ],

    # Set traces_sample_rate to 1.0 to capture 100%
    # of transactions for performance monitoring.
    # We recommend adjusting this value in production,
    traces_sample_rate=1.0,
)
```

The timeout warning is sent only if the timeout in the Cloud Function configuration is set to a value greater than one second.

<div class="alert alert-info" role="alert"><h5 class="no_toc">Note</h5><div class="alert-body content-flush-bottom">If you are using a web framework in your Cloud Function, the framework might catch those exceptions before we get to see them. Make sure to enable the framework specific integration as well, if one exists. See [*Integrations*](/platforms/python/#integrations) for more information.</div>
</div>
