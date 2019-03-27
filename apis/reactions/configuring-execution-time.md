---
description: >-
  You can configure when a Reaction should invoke an action for a matching event
  by configuring its execution time.
---

# Configuring execution time

## Immediate Reactions

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

## Scheduled Reactions

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

#### Supported date/time formats   <a id="supported-date-time-formats"></a>

The date or timestamp in the field specified as the `triggerTimeField` must be in any of the following formats:

* `Epoc millis timestamp (number)`
* `'yyyy-MM-dd'T'HH:mm:ssZ'`
* `'yyyy-MM-dd'`
* `'yyyyMMdd'`
* `'yyMMdd'`

