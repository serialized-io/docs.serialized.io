---
description: >-
  Serialized provides a simple yet secure authentication using API keys over
  HTTPS.
---

# Authentication

## Authenticating with the API

All your Serialized projects have separate API key pairs that you use to authenticate towards the API. Your keys will redirect you to the corresponding project so the API urls will remain the same regardless of which project you authenticate to.

The keys are provided as HTTP headers:

```text
Serialized-Access-Key: <YOUR-ACCESS-KEY>
Serialized-Secret-Access-Key: <YOUR-SECRET-ACCESS-KEY>
```

This is an example of a cURL request to the API authenticated with example keys:

```text
curl --header "Serialized-Access-Key: example-access-key" \
     --header "Serialized-Secret-Access-Key: example-secret-key" \
     https://api.serialized.io/feeds
```

{% hint style="info" %}
You must provide the access key headers in all requests to the API.
{% endhint %}

