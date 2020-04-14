# Create reaction definition

{% api-method method="post" host="https://api.serialized.io" path="/reactions/definitions" %}
{% api-method-summary %}
Create reaction definition
{% endapi-method-summary %}

{% api-method-description %}
Create a new reaction definition
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-headers %}
{% api-method-parameter name="Serialized-Access-Key" type="string" required=true %}
Serialized access key
{% endapi-method-parameter %}

{% api-method-parameter name="Serialized-Secret-Access-Key" type="string" required=true %}
Serialized secret key
{% endapi-method-parameter %}
{% endapi-method-headers %}

{% api-method-body-parameters %}
{% api-method-parameter name="reactionName" type="string" required=true %}
Unique name of the reaction
{% endapi-method-parameter %}

{% api-method-parameter name="feedName" type="string" required=true %}
Name of the feed
{% endapi-method-parameter %}

{% api-method-parameter name="reactOnEventType" type="string" required=true %}
Event type to react on
{% endapi-method-parameter %}

{% api-method-parameter name="cancelOnEventTypes" type="array" required=false %}
Event types to cancel reaction scheduled in the future
{% endapi-method-parameter %}

{% api-method-parameter name="triggerTimeField" type="string" required=false %}
Path to event data field containing trigger time. If not specified, trigger time will be ASAP. Dot notation supported.
{% endapi-method-parameter %}

{% api-method-parameter name="offset" type="string" required=false %}
Trigger time offset. Defined in the ISO-8601 duration format \(PnDTnHnMn.nS\). May be negative.
{% endapi-method-parameter %}

{% api-method-parameter name="action" type="object" required=true %}
Action to invoke. See examples below.
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
Reaction definition successfully created.
{% endapi-method-response-example-description %}

```javascript

```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=409 %}
{% api-method-response-example-description %}
If reaction definition name is not unique
{% endapi-method-response-example-description %}

```text

```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=422 %}
{% api-method-response-example-description %}
Invalid request body
{% endapi-method-response-example-description %}

```text

```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

## Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl -i https://api.serialized.io/reactions/definitions \
  --header "Content-Type: application/json" \
  --header "Serialized-Access-Key: <YOUR_ACCESS_KEY>" \
  --header "Serialized-Secret-Access-Key: <YOUR_SECRET_ACCESS_KEY>" \
  --data '
  {
    "reactionName": "payment-processed-email-reaction",
    "feedName": "payment",
    "reactOnEventType": "PaymentProcessed",
    "action": {
      "actionType": "HTTP_POST",
      "targetUri": "https://your-email-service"
    }
  }
  '
```
{% endtab %}

{% tab title="Java" %}
```java
import com.google.common.collect.ImmutableMap;
import javax.ws.rs.client.Client;
import javax.ws.rs.client.ClientBuilder;

Client client = ClientBuilder.newClient();
URI apiRoot = URI.create("https://api.serialized.io");

Map<String, Object> reactionDefinition = ImmutableMap.of(
        "reactionName", "payment-processed-email-reaction",
        "feedName", "payment",
        "reactOnEventType", "PaymentProcessed",
        "action", ImmutableMap.of(
            "actionType", "HTTP_POST",
            "targetUri", "https://your-email-service"
        )
    );

Response response = client.target(apiRoot)
    .path("reactions")
    .path("definitions")
    .request()
    .header("Serialized-Access-Key", "<YOUR_ACCESS_KEY>")
    .header("Serialized-Secret-Access-Key", "<YOUR_SECRET_ACCESS_KEY>")
    .post(Entity.json(reactionDefinition));
```
{% endtab %}

{% tab title="C\#" %}
```csharp
using System;
using System.Collections.Generic;
using RestSharp;

var definition = new Dictionary<string, object>
{
    { "reactionName", "payment-processed-email-reaction" },
    { "feedName", "payment" },
    { "reactOnEventType", "PaymentProcessed" },
    { "action", new Dictionary<string, Object>
        {
            {"actionType", "HTTP_POST"},
            {"targetUri", "https://your-email-service"}
        }
    },
};

var postRequest = new RestRequest("reactions/definitions", Method.POST)
   .AddHeader("Serialized-Access-Key", "<YOUR_ACCESS_KEY>")
   .AddHeader("Serialized-Secret-Access-Key", "<YOUR_SECRET_ACCESS_KEY>");
   .AddJsonBody(definition);

var response = client.Execute(postRequest);
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

const definition = {
  reactionName: "payment-processed-email-reaction",
  feedName: "payment",
  reactOnEventType: "PaymentProcessed",
  action: {
    actionType: "HTTP_POST",
    targetUri: "https://your-email-service"
  }
};

client.post("reactions/definitions", definition)
    .then(function (response) {
      // Handle response
    })
    .catch(function (error) {
      // Handle error
    });
```
{% endtab %}
{% endtabs %}

