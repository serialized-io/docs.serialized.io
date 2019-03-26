---
description: >-
  For greater flexibility you can write your own projector code and deploy it as
  a function wherever you prefer. AWS Lambda, Google Cloud Functions and Azure
  Functions are all viable alternatives.
---

# External projector functions

## Executing external projector functions

The external function will be automatically executed by Serialized for every event in the particular feed matching the event type\(s\) specified in the projection definition.

The structure of a handler using an external function looks like this:

```javascript
{
  "eventType": "<YOUR_EVENT_TYPE>",
  "functionUri": "https://example.com/your-function"
}
```

An example of a complete projection definition could look like this:

```javascript
{
  "projectionName": "orders-per-customer",
  "feedName": "order",
  "handlers": [
    {
      "eventType": "OrderPlacedEvent",
      "functionUri": "https://example.com/calculate-orders-per-customer"
    }
  ]
}
```

### Event Projection API

```text
https://api.serialized.io/projections
```

The Event Projection API makes it easy to use your events to build models that can be used to present your event data.

### Projection definitions  <a id="projection-definitions"></a>

```text
https://api.serialized.io/projections/definitions
```

Projection definitions dictates how events should be projected. When you create a projection definition all events from the beginning of time will be automatically processed to build up the projections. When you update your projection definition, all events will be re-processed and generate new projections.

### List all projections  <a id="list-all-projections"></a>

```text
https://api.serialized.io/projections
```

### Single projections  <a id="single-projections"></a>

Single projections creates one projection document per aggregate id in your events, which makes it suitable for “flattening” the events for a particular aggregate making it easier to query.

The projected result will be available below the path `/single/<projection-name>/<id>` eg.

```text
https://api.serialized.io/projections/single/orders
```

…and

```text
https://api.serialized.io/projections/single/orders/4e82e594-6409-4f82-bf64-3a6d82a469e9
```

### Aggregated projections  <a id="aggregated-projections"></a>

Aggregated projections creates one combined projection from all aggregates in the same event feed. This makes it easy to implement things such as statistics and other kind of reports.

The projected result will be available below the path `/aggregated/<projection-name>` eg.

```text
https://api.serialized.io/projections/aggregated/order-totals
```

### Defining an event handler  <a id="defining-an-event-handler"></a>

Your projection definition consists of a number of event handlers that are applied to your events in sequence, to modify the current state of a projection. Each event handler defines processing logic for a particular event type and includes a number of options for filtering/matching event data and producing different outputs. This is how you can easily create and adapt client-specific views based on your events.

Event handlers are defined in the `handlers` section of your projection definition. You can add any number of handlers to your projection definition, even though it’s likely that a few will suffice for most cases.

```text
{
  "projectionName": "todos",
  "feedName": "todos",
  "aggregated": false,
  "handlers": [
    <YOUR_HANDLER_HERE>
  ]
}
```

#### JsonPath Templating  <a id="jsonpath-templating"></a>

To create useful projections we need to merge the event data with the projection data in different ways. We provide templating support built on [JsonPath](http://goessner.net/articles/JsonPath) to support this out-of-the-box.

If the templating support we provide is too restricted for your use-case we encourage you to write your own projector, deployed either as an external function \(hosted on eg. AWS\) or external service using the [Feed API](https://serialized.io/docs/apis/event-feed/) and a storage of your choice. Read more about customized projectors in the next chapter.

The structure of a handler using the provided JsonPath templating looks like this:

```text
{
  "eventType": "<YOUR_EVENT_TYPE>",
  "functions": [
    {
      "function": "merge",
      "targetSelector": "$.projection",
      "eventSelector": "$.event"
    }]
}
```

This example will merge the content of the incoming event by using the JsonPath selector `$.event` with the current state of the projection by targeting the full projection state using the JsonPath selector `$.projection`.

In a handler function you always have access to `$.event` \(the current event being handled\) and `$.projection` \(the current projection state\).

The above example can also be simplified to:

```text
{ "function": "merge" }
```

since the default values for `targetSelector` and `eventSelector` are `$.event` and `$.projection`, respectively.

**Supported handler functions**

We provide a number of handler functions that modify the state in different ways.

`merge`

Merges two JSON objects.

| Argument | Expected evaluation |
| :--- | :--- |
| `targetSelector` | `Object` |
| `eventSelector` | `Object` |

`push`

Adds anything to the end of an array.

| Argument | Expected evaluation |
| :--- | :--- |
| `targetSelector` | `Array` |
| `eventSelector` | `Array` |

Example:

```text
{
  "eventType": "RunnerFinishedRaceEvent",
  "functions": [
    {
      "function": "push",
      "targetSelector": "$.projection.finishers",
      "eventSelector": "$.event['runnerName','finishTime']"
    }
  ]
}
```

`prepend`

Adds anything to the beginning of an array.

| Argument | Expected evaluation |
| :--- | :--- |
| `targetSelector` | `Array` |
| `eventSelector` | `Array` |

Example:

```text
{
  "eventType": "TodoAddedEvent",
  "functions": [
    {
      "function": "prepend",
      "targetSelector": "$.projection.todos",
      "eventSelector": "$.event['todoId','todoText']"
    }
  ]
}
```

`set`

Replaces the value of an existing key.

| Argument | Expected evaluation |
| :--- | :--- |
| `targetSelector` | `Any` but must exist |
| `eventSelector` | `Any` |

Example:

```text
{
  "eventType": "EmailUpdatedEvent",
  "functions": [
    {
      "function": "set",
      "targetSelector": "$.projection.user.email",
      "eventSelector": "$.event.newEmail"
    }
  ]
}
```

`remove`

Removes an existing key or an array element matching filter expression.

| Argument | Expected evaluation |
| :--- | :--- |
| `targetSelector` | `Any` but must exist |
| `eventSelector` | `Any` |
| `targetFilter` | `Filter expression` required for array element removal |

Example:

```text
{
  "eventType": "TodoRemovedEvent",
  "functions": [
    {
      "function": "remove",
      "targetSelector": "$.projection.todos[?]",
      "targetFilter": "[?(@.todoId == $.event.todoId)]"
    }
  ]
}
```

`add`

Sums two numbers together.

| Argument | Expected evaluation |
| :--- | :--- |
| `targetSelector` | `Number` |
| `eventSelector` | `Number` |

Example:

```text
{
  "eventType": "OrderPlacedEvent",
  "functions": [
    {
      "function": "add",
      "targetSelector": "$.projection.totalAmount",
      "eventSelector": "$.event['orderAmount']"
    }
  ]
}
```

`subtract`

Subtracts two numbers.

| Argument | Expected evaluation |
| :--- | :--- |
| `targetSelector` | `Number` |
| `eventSelector` | `Number` |

`inc`

Increases a number by one.

| Argument | Expected evaluation |
| :--- | :--- |
| `targetSelector` | `Number` |

Example:

```text
{
  "eventType": "TicketReservedEvent",
  "functions": [
    {
      "function": "inc",
      "targetSelector": "$.projection.ticketCount"
    }
  ]
}
```

`dec`

Decreases a number by one.

| Argument | Expected evaluation |
| :--- | :--- |
| `targetSelector` | `Number` |

Example:

```text
{
  "eventType": "TicketReleasedEvent",
  "functions": [
    {
      "function": "dec",
      "targetSelector": "$.projection.ticketCount"
    }
  ]
}
```

**Filters**

Functions can also provide two filters: `targetFilter` and `eventFilter`.

To add a filter to a selector you provide a `[?]` in the selector text, as described in the JsonPath documentation. The filter for the selector is then applied for the given function. This is useful for matching on ids in nested lists or to apply conditional logic for when/how to process events.

All Filter Operators that are described [here](https://github.com/json-path/JsonPath) are supported.

**Testing JsonPath projections**

To test your projection definitions you can use our online tool [here](https://console.serialized.io/projection-tester).

**More examples**

More examples can be found [here](https://github.com/serialized-io/samples-java/tree/master/order-service/src/main/resources/projections).

#### Customized external projector functions  <a id="customized-external-projector-functions"></a>

For greater flexibility you can write your own projector code and deploy it as a function wherever you prefer. [AWS Labmda](https://aws.amazon.com/lambda/), [Google Cloud Functions](https://cloud.google.com/functions/) and [Azure Functions](https://azure.microsoft.com/services/functions) are all viable alternatives.

The external function will be automatically executed by Serialized for every event in the particular feed matching the event type\(s\) specified in the projection definition.

The structure of a handler using an external function looks like this:

```javascript
{
  "eventType": "<YOUR_EVENT_TYPE>",
  "functionUri": "https://example.com/your-function"
}
```

An example of a complete projection definition could look like this:

```javascript
{
  "projectionName": "orders-per-customer",
  "feedName": "order",
  "handlers": [
    {
      "eventType": "OrderPlacedEvent",
      "functionUri": "https://example.com/calculate-orders-per-customer"
    }
  ]
}
```

## **Request to the external function**

**&lt;todo&gt;**

