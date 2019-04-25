# Conditional requests

To minimize bandwidth usage, the response from `/aggregates/` and `/projections/` returns the `ETag` header. You can use the value of this header to make subsequent requests to that resources using the `If-None-Match` header. If the resource has not changed, the server will return a `304 Not Modified`.

```bash
HTTP/1.1 200 OK
Content-Type: application/json
Date: Tue, 19 Sep 2017 14:04:15 GMT
ETag: "eccbc87e4b5ce2fe28308fd9f2a7baf3"
...

{
  "aggregateId": "..."
}
```

### Making a conditional request

{% tabs %}
{% tab title="cURL" %}
```bash
curl -i \
  --header "Serialized-Access-Key: <YOUR_ACCESS_KEY>" \
  --header "Serialized-Secret-Access-Key: <YOUR_SECRET_ACCESS_KEY>" \
  --header "If-None-Match: \"eccbc87e4b5ce2fe28308fd9f2a7baf3\"" \
  https://api.serialized.io/aggregates/order/99415be8-6819-4470-860c-c2933558d8d3
```
{% endtab %}

{% tab title="Java" %}
```java
import javax.ws.rs.client.Client;
import javax.ws.rs.client.ClientBuilder;
import javax.ws.rs.core.UriBuilder;
import javax.ws.rs.core.Response;

Client client = ClientBuilder.newClient();
URI apiRoot = URI.create("https://api.serialized.io");
    
Response response = client.target(apiRoot)
    .path("aggregates")
    .path("order")
    .path("99415be8-6819-4470-860c-c2933558d8d3")
    .request()
    .header("Serialized-Access-Key", "<YOUR_ACCESS_KEY>")
    .header("Serialized-Secret-Access-Key", "<YOUR_SECRET_ACCESS_KEY>")
    .header("If-None-Match", "eccbc87e4b5ce2fe28308fd9f2a7baf3")
    .get();
```
{% endtab %}

{% tab title="C\#" %}
```csharp
using RestSharp;
using System;

var client = new RestClient("https://api.serialized.io");

var request = new RestRequest("aggregates/{aggregateType}/{aggregateId}", Method.GET)
   .AddUrlSegment("aggregateType", "order")
   .AddUrlSegment("aggregateId", "99415be8-6819-4470-860c-c2933558d8d3")
   .AddHeader("Serialized-Access-Key", "<YOUR_ACCESS_KEY>")
   .AddHeader("Serialized-Secret-Access-Key", "<YOUR_SECRET_ACCESS_KEY>")
   .AddHeader("If-None-Match", "eccbc87e4b5ce2fe28308fd9f2a7baf3")

var response = client.Execute(request);
```
{% endtab %}

{% tab title="Node" %}
```javascript
const axios = require("axios");

const client = axios.create({
  baseURL: "https://api.serialized.io",
  headers: {"Serialized-Access-Key": "<YOUR_ACCESS_KEY>"},
  headers: {"Serialized-Secret-Access-Key": "<YOUR_SECRET_ACCESS_KEY>"}
});

const params = {
  headers: {"If-None-Match": "eccbc87e4b5ce2fe28308fd9f2a7baf3"},
};

client.get("aggregates/order/99415be8-6819-4470-860c-c2933558d8d3", params)
    .then(function (response) {
      // Handle response
    })
    .catch(function (error) {
      // Handle error
    });
```
{% endtab %}
{% endtabs %}



