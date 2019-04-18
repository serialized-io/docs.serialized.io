# Get Projections Overview

{% api-method method="get" host="https://api.serialized.io" path="/projections" %}
{% api-method-summary %}
Get projections overview
{% endapi-method-summary %}

{% api-method-description %}
Returns high-level information about all projections, including projection names and count
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
Projections successfully received
{% endapi-method-response-example-description %}

```javascript
{
  "projections": [
    {
      "projectionName": "shipping-stats",
      "feedName": "shipment",
      "aggregated": false,
      "projectionsCount": 1977
    }
  ]
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% tabs %}
{% tab title="cURL" %}
```bash
curl -i \
  --header "Serialized-Access-Key: <YOUR_ACCESS_KEY>" \
  --header "Serialized-Secret-Access-Key: <YOUR_SECRET_ACCESS_KEY>" \
  https://api.serialized.io/projections
```
{% endtab %}

{% tab title="Java" %}
```java
import javax.ws.rs.client.Client;
import javax.ws.rs.client.ClientBuilder;

Map response = client.target(apiRoot)
    .path("projections")
    .request()
    .header("Serialized-Access-Key", "<YOUR_ACCESS_KEY>")
    .header("Serialized-Secret-Access-Key", "<YOUR_SECRET_ACCESS_KEY>")
    .get(Map.class);
```
{% endtab %}
{% endtabs %}

