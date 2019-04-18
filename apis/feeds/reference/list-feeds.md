# Get feeds overview

{% api-method method="get" host="https://api.serialized.io" path="/feeds" %}
{% api-method-summary %}
Get feeds overview
{% endapi-method-summary %}

{% api-method-description %}
Overview showing number of batches, aggregates and events per aggregate type.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
Feeds successfully received
{% endapi-method-response-example-description %}

```javascript
{
  "feeds": [
    {
      "aggregateType": "payment",
      "aggregateCount": 1337,
      "batchCount": 7331,
      "eventCount": 9977
    }
  ]
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

### Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl -i \
  --header "Serialized-Access-Key: <YOUR_ACCESS_KEY>" \
  --header "Serialized-Secret-Access-Key: <YOUR_SECRET_ACCESS_KEY>" \
  https://api.serialized.io/feeds
```
{% endtab %}

{% tab title="Java" %}
```java
import javax.ws.rs.client.Client;
import javax.ws.rs.client.ClientBuilder;
import javax.ws.rs.core.Response;

Client client = ClientBuilder.newClient();
URI apiRoot = URI.create("https://api.serialized.io");
    
Map response = client.target(apiRoot)
    .path("feeds")
    .request()
    .header("Serialized-Access-Key", "<YOUR_ACCESS_KEY>")
    .header("Serialized-Secret-Access-Key", "<YOUR_SECRET_ACCESS_KEY>")
    .get(Map.class);
```
{% endtab %}
{% endtabs %}

