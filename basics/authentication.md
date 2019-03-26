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

This is an example of an authenticated request to the API using these headers:

{% tabs %}
{% tab title="cURL" %}
```bash
curl --header "Serialized-Access-Key: example-access-key" \
     --header "Serialized-Secret-Access-Key: example-secret-key" \
     https://api.serialized.io/feeds
```
{% endtab %}
{% endtabs %}

{% hint style="warning" %}
You must provide these access key headers in all requests to the API.
{% endhint %}

