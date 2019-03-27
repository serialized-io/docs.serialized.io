---
description: >-
  Each reaction is configured with a specific Action that defines what will
  happen when the reaction is triggered. When a Reaction is triggered it will
  execute its configured Action.
---

# Actions

## HTTP Post

The action type \(`HTTP_POST`\) requires the field `targetUri` to be populated with the address that should receive the POST request. Optionally the field `httpHeaders` can be set, i.e. to handle Basic Authentication etc.

## Slack

The action type \(`SLACK_POST`\) requires the field `targetUri` to be populated with a valid [Slack webhook URI](https://api.slack.com/incoming-webhooks). The message text is specified in the field `body`. Simple templating is supported and event data can be accessed using dot \(.\) notation.

Example of a Reaction posting a notification to Slack:

{% code-tabs %}
{% code-tabs-item title="Slack reaction example" %}
```javascript
{
  "reactionName": "on-order-placed-slack-notifier",
  "feedName": "order",
  "reactOnEventType": "OrderPlacedEvent",
  "action": {
    "actionType": "SLACK_POST",
    "targetUri": "https://hooks.slack.com/services/T0000/B0000/XXXX",
    "body": "A new order with number ${event.data.orderNumber} was placed by ${event.data.customer.email}!"
  }
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Given this event:

```javascript
{
  "eventId": "ca37d05c-a852-4de5-961f-16fb35e8cd7b",
  "eventType": "OrderPlacedEvent",
  "data": {  
    "orderNumber": "12312345",
    "customer": {
      "email": "customer@example.com"
    }
  }
}
```

the message to Slack will look like this:

`A new order with number 12312345 was placed by customer@example.com!`

## Cancelling scheduled actions

Each scheduled Reaction can be configured to automatically cancel its pending action if a certain event occurs by adding the event type to the array `cancelOnEventTypes`. This is particularly useful for cancelling timeouts or reminders no longer valid.

Example of a body setting up a Reaction with a 12h delay and a cancelOn event:

```javascript
{
  "reactionName": "send-reminder-to-inactive-users",
  "feedName": "gamerounds",
  "reactOnEventType": "UserLoggedOutEvent",
  "cancelOnEventTypes": ["UserLoggedInEvent"],
  "offset": "P7D",
  "action": {
    "actionType": "HTTP_POST",
    "targetUri": "https://your-email-service"
  }
}
```

## Receiving reaction requests

The reaction requests are HTTP POST requests with a JSON body. The JSON has the following format:

```javascript
{  
   "metadata":{  
      "aggregateId":"<some-aggregate-id>",
      "timestamp": <epoc-millis>,
      "sequenceNumber": <n>
   },
   "event":{  
      "eventId":"<some-event-id>",
      "eventType":"<event-type>",
      "data":{  
         "key":"value",
         "key":"value",
         ...
      }
   }
}
```

### Target end-point security

As the end-point specified in the `targetUri` must be exposed to the public internet it should be protected. Currently it’s possible to utilize Basic Authentication and include the appropriate HTTP header in the Reaction definition.

Example:

```javascript
{
  "reactionName": "notify-on-order-shipped",
  "feedName": "order",
  "reactOnEventType": "OrderShippedEvent",
  "action": {
    "actionType": "HTTP_POST",
    "targetUri": "https://your-service",
    "httpHeaders": {
      "Authorization": "Basic c2VasXlZdyW2dGsONfM5fD4adleRDgVbDNgRkgFgZUpgoxgNHdsg15QktFcm1ndjI="
    }    
  }
}
```

### Request signatures

All outgoing HTTP requests has the `User-Agent` header set to `Serialized-IO-Reaction/1.0` and includes a Serialized specific signature header that can be used to verify the request’s authenticity.

The header is named `Serialized-Request-Signature` and contains a [HMAC](https://en.wikipedia.org/wiki/HMAC) calculated using the HmacSHA256 algorithm, specified in RFC 2104 and FIPS PUB 180-2, with the reaction definition name used as key.

Example code of a Java/Jersey/Dropwizard server end-point verifying the signature:

```java
    import org.apache.commons.codec.digest.*;
    import javax.ws.rs.*;

    @POST
    @Path("notifications")
    public Response performNotification(@Context HttpHeaders headers, String body) {
      String expectedReactionName = "notify-on-order-shipped";
      String receivedSignature = headers.getHeaderString("Serialized-Request-Signature");
      String calculatedSignature = new HmacUtils(HMAC_SHA_256, expectedReactionName).hmacHex(body);

      if (!calculatedSignature.equals(receivedSignature)) {
        throw new WebApplicationException(BAD_REQUEST);
      }

      ...
    }
```

## Automatic retries

Serialized Reactions have a built-in retry mechanism and will be retried several times over the course of a day. A 2xx response is required for success, whereas a 4xx or 5xx response will be treated as an error and retried.

