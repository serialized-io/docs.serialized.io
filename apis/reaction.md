---
description: >-
  The Event Reaction API provides support for connecting your events to
  side-effect behavior such as sending e-mails, calling your backend or any
  other external service.
---

# Reaction

Reactions are configured to react to a specific event and trigger an action whenever that event is processed by the event reactor. An example could be sending a confirmation email to a customer when an order has been successfully placed.

It is not possible for a Reaction to trigger on an event that has happened in the past. Reactions are always configured to listen to events from the moment when the Reaction is configured.

## Reaction definitions

Reaction definitions dictates how and when side-effects should trigger.

```text
https://api.serialized.io/reactions/definitions
```

## Actions

When a Reaction is triggered it will execute its configured Action.

### HTTP Post

The action type \(`HTTP_POST`\) requires the field `targetUri` to be populated with the address that should receive the POST request. Optionally the field `httpHeaders` can be set, i.e. to handle Basic Authentication etc.

### Slack

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

## Configuring execution time

You can configure when a Reaction should invoke an action for a matching event by configuring its execution time.

### Immediate Reactions

Immediate Reactions causes the preconfigured action to execute as soon as an event of the expected type is received.

Example of a request setting up a Reaction that triggers immediately on event:

```javascript
{
  "reactionName": "notify-on-order-shipped",
  "feedName": "order",
  "reactOnEventType": "OrderShippedEvent",
  "action": {
    "actionType": "HTTP_POST",
    "targetUri": "https://your-email-service"
  }
}
```

A scheduled Reaction is configured to wait a certain time before executing its action. This makes it possible to implement use-cases such as reminders, loyalty awards or other cases where time is an important factor.

### Scheduled Reactions

A scheduled Reaction is configured to wait a certain time before executing its action. This makes it possible to implement use-cases such as reminders, loyalty awards or other cases where time is an important factor.

To create a scheduled Reaction that triggers at a point in time relative to the time the event occurs, specify a pattern in the field `offset`. The pattern is defined in the ISO-8601 duration format `(PnDTnHnMn.nS)` - see separate section for details. Note that specifying a negative offset does not make sense in this case and will cause the Reaction to trigger instantly when the event occurs.

Example of a request setting up a Reaction with a 12h delay:

```javascript
{
  "reactionName": "remind-players",
  "feedName": "gameround",
  "reactOnEventType": "GameRoundCompletedEvent",
  "offset": "PT12H",
  "action": {
    "actionType": "HTTP_POST",
    "targetUri": "https://your-push-notification-service"
  }
}
```

### Absolute trigger time

To create a scheduled Reaction that triggers at a point in time specified in the event data, use the field `triggerTimeField` and point out where the time information is located in the event. Note that dot notation is allowed.

Example of an event containing time information:

```javascript
 {  
     "eventId":"634daa41-ba64-4558-9ee3-a281060f37ee",
     "eventType":"MeetingCreatedEvent",
     "data":{  
        "meeting": {
          "startTime": "2018-02-05T12:00:00Z"
        }
     }
  }
```

Example of a request setting up a Reaction that will trigger when the meeting starts:

```javascript
{
  "reactionName": "notify-participants",
  "feedName": "meeting",
  "reactOnEventType": "MeetingCreatedEvent",
  "triggerTimeField": "meeting.startTime",
  "action": {
    "actionType": "HTTP_POST",
    "targetUri": "https://some-notification-service"
  }
}
```

### Absolute trigger time with offset

It’s possible to add an offset to the case described above. For instance, to create a Reaction triggering two hours before the meeting starts, create a Reaction with both the `triggerTimeField` and the `offset` field specified. The offset pattern is defined in the ISO-8601 duration format \(PnDTnHnMn.nS\) which also supports negative offsets. See separate section for details.

```javascript
{
  "reactionName": "remind-participants",
  "feedName": "meeting",
  "reactOnEventType": "MeetingCreatedEvent",
  "triggerTimeField": "meeting.startTime",
  "offset": "-PT2H",
  "action": {
    "actionType": "HTTP_POST",
    "targetUri": "https://your-push-notification-service"
  }
}
```

#### Offset pattern

The offset pattern is defined in the ISO-8601 duration format \(PnDTnHnMn.nS\).

Examples:

```text
"PT20.345S" - parses as "20.345 seconds"
"PT15M"     - parses as "15 minutes" (where a minute is 60 seconds)
"PT10H"     - parses as "10 hours" (where an hour is 3600 seconds)
"P2D"       - parses as "2 days" (where a day is 24 hours or 86400 seconds)
"P2DT3H4M"  - parses as "2 days, 3 hours and 4 minutes"
"PT-6H3M"   - parses as "-6 hours and +3 minutes"
"-PT6H3M"   - parses as "-6 hours and -3 minutes"
"-PT-6H+3M" - parses as "+6 hours and -3 minutes"
```



#### Supported date/time formats <a id="supported-date-time-formats"></a>

The date or timestamp in the field specified as the `triggerTimeField` must be in any of the following formats:

* `Epoc millis timestamp (number)`
* `'yyyy-MM-dd'T'HH:mm:ssZ'`
* `'yyyy-MM-dd'`
* `'yyyyMMdd'`
* `'yyMMdd'`

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

