# Defining event handlers

Your projection definition consists of a number of event handlers that are applied to your events in sequence, to modify the current state of a projection. Each event handler defines processing logic for a particular event type and includes a number of options for filtering/matching event data and producing different outputs. This is how you can easily create and adapt client-specific views based on your events.

Event handlers are defined in the `handlers` section of your projection definition. You can add any number of handlers to your projection definition, even though it’s likely that a few will suffice for most cases.

## JsonPath templating

To create useful projections we need to merge the event data with the projection data in different ways. We provide templating support built on [JsonPath](http://goessner.net/articles/JsonPath) to support this out-of-the-box.

If the templating support we provide is too restricted for your use-case we encourage you to write your own projector, deployed either as an external function \(hosted on eg. AWS\) or external service using the [Feed API](https://serialized.io/docs/apis/event-feed/) and a storage of your choice. Read more about customized projectors in the next chapter.

The structure of a handler using the provided JsonPath templating looks like this:

```javascript
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

```javascript
{ "function": "merge" }
```

since the default values for `targetSelector` and `eventSelector` are `$.event` and `$.projection`, respectively.

## **Supported handler functions**

### **merge**

Merges two JSON objects.

| Argument | Expected evaluation |
| :--- | :--- |
| targetSelector | Object |
| eventSelector | Object |

### **push**

Adds anything to the end of an array.

| Argument | Expected evaluation |
| :--- | :--- |
| targetSelector | Array |
| eventSelector | Array |

{% code-tabs %}
{% code-tabs-item title="Push example" %}
```javascript
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
{% endcode-tabs-item %}
{% endcode-tabs %}

### **prepend**

Adds anything to the beginning of an array.

| Argument | Expected evaluation |
| :--- | :--- |
| targetSelector | Array |
| eventSelector | Array |

{% code-tabs %}
{% code-tabs-item title="Prepend example" %}
```javascript
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
{% endcode-tabs-item %}
{% endcode-tabs %}

### **set**

Replaces the value of an existing key

| Argument | Expected evaluation |
| :--- | :--- |
| targetSelector | Any \(but must exist\) |
| eventSelector | Any |

{% code-tabs %}
{% code-tabs-item title="Set example" %}
```javascript
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
{% endcode-tabs-item %}
{% endcode-tabs %}

### **remove**

Removes an existing key or an array element matching filter expression.

| Argument | Expected evaluation |
| :--- | :--- |
| targetSelector | Any \(but must exist\) |
| eventSelector | Any |
| targetFilter \(required\) | Filter expression |

{% code-tabs %}
{% code-tabs-item title="Remove example" %}
```javascript
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
{% endcode-tabs-item %}
{% endcode-tabs %}

### **add**

Sums two numbers together.

| Argument | Expected evaluation |
| :--- | :--- |
| targetSelector | Number |
| eventSelector | Number |

{% code-tabs %}
{% code-tabs-item title="Add example" %}
```javascript
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
{% endcode-tabs-item %}
{% endcode-tabs %}

### **subtract**

Subtracts two numbers.

| Argument | Expected evaluation |
| :--- | :--- |
| targetSelector | Number |
| eventSelector | Number |

{% code-tabs %}
{% code-tabs-item title="Subtract example" %}
```javascript
{
  "eventType": "OrderRefundedEvent",
  "functions": [
    {
      "function": "subtract",
      "targetSelector": "$.projection.totalAmount",
      "eventSelector": "$.event['orderAmount']"
    }
  ]
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

### **inc**

Increases a number by one.

| Argument | Expected evaluation |
| :--- | :--- |
| targetSelector | Number |

{% code-tabs %}
{% code-tabs-item title="Inc example" %}
```javascript
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
{% endcode-tabs-item %}
{% endcode-tabs %}

### **dec**

Decreases a number by one.

| Argument | Expected evaluation |
| :--- | :--- |
| targetSelector | Number |

{% code-tabs %}
{% code-tabs-item title="Dec example" %}
```javascript
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
{% endcode-tabs-item %}
{% endcode-tabs %}

## Filters

Functions can also provide two filters: `targetFilter` and `eventFilter`.

To add a filter to a selector you provide a `[?]` in the selector text, as described in the JsonPath documentation. The filter for the selector is then applied for the given function. This is useful for matching on ids in nested lists or to apply conditional logic for when/how to process events.

All Filter Operators that are described [here](https://github.com/json-path/JsonPath) are supported.

