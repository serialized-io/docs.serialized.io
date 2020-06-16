---
description: >-
  This page describes how to use the built-in JsonPath support to process event
  data into projections.
---

# JsonPath Functions

To create useful projections we need to merge the event data with the projection data in different ways. We provide templating support built on [JsonPath](http://goessner.net/articles/JsonPath) to support this out-of-the-box.

If the templating support we provide is too restricted for your use-case we encourage you to write your own projector, deployed either as an external function \(hosted on eg. AWS\) or external service using the [Feed API](https://docs.serialized.io/api-reference/apis/feeds) and a storage of your choice. Read more about customized projectors in the next chapter.

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

In a handler function you always have access to `$.event` \(the current event being handled\) and `$.projection` \(the current projection state\). Besides that, there is also a `$.metadata` struct containing the fields `aggregateId`, `timestamp`, `createdAt` and `updatedAt`.

The above example can also be simplified to:

```javascript
{ "function": "merge" }
```

since the default values for `targetSelector` and `eventSelector` are `$.event` and `$.projection`, respectively.

### Providing raw data

If you have a static value that you want to use as the data for the projection instead of some value that is provided in the event, you can use the `rawData` argument instead of the `eventSelector`. You can then provide any JSON value which will be used as-is.

{% hint style="warning" %}
`rawData` and `eventSelector` are always mutual exclusive, so make sure you only provide one of them in your function configuration.
{% endhint %}

## **Basic object modification**

### **merge**

Merges event data with existing projected data.

| Argument | Evaluated Type | Required |
| :--- | :--- | :--- |
| targetSelector | Object | Yes |
| eventSelector | Object | No |
| targetFilter | _Not used_ | No |
| eventFilter | _Not used_ | No |
| rawData | Object | No |

```javascript
{
  "eventType": "OrderStatusChangedEvent",
  "functions": [
    {
      "function": "merge"
    }
  ]
}
```

### **set**

Sets/replaces the value of an existing key.

| Argument | Evaluated Type | Required |
| :--- | :--- | :--- |
| targetSelector | Any | Yes |
| eventSelector | Any | No |
| targetFilter | Filter expression | No |
| eventFilter | Filter expression | No |
| rawData | Any | No |

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

### **unset**

Removes an existing key.

| Argument | Evaluated Type | Required |
| :--- | :--- | :--- |
| targetSelector | Any | Yes |
| eventSelector | _Not used_ | No |
| targetFilter | Filter expression | No |
| eventFilter | Filter expression | No |
| rawData | _Not used_ | No |

```javascript
{
  "eventType": "EmailRemovedEvent",
  "functions": [
    {
      "function": "unset",
      "targetSelector": "$.projection.user.email"
    }
  ]
}
```

### **clear**

Clear the entire projection or a specific field, i.e reset it to a default value.

| Data type | Default value |
| :--- | :--- |
| String | `""` |
| Boolean | `false` |
| Number | `0` |
| Array | `[]` |
| Object | `{}` |

| Argument | Evaluated Type | Required |
| :--- | :--- | :--- |
| targetSelector | Any | No |
| eventSelector | _Not used_ | No |
| targetFilter | Filter expression | No |
| eventFilter | Filter expression | No |
| rawData | _Not used_ | No |

```javascript
{
  "eventType": "ListClearedEvent",
  "functions": [
    {
      "function": "clear",
      "targetSelector": "$.projection.todoList"
    }
  ]
}
```

### **delete**

Delete a projection instance entirely. Future requests will result in a 404 Not Found.

| Argument | Evaluated Type | Required |
| :--- | :--- | :--- |
| targetSelector | _Not used_ | No |
| eventSelector | _Not used_ | No |
| targetFilter | Filter expression | No |
| eventFilter | Filter expression | No |
| rawData | _Not used_ | No |

```javascript
{
  "eventType": "UserDeletedEvent",
  "functions": [
    {
      "function": "delete"
    }
  ]
}
```

## **Working with arrays/lists**

### **append**

Adds anything to the end of an array.

| Argument | Evaluated Type | Required |
| :--- | :--- | :--- |
| targetSelector | Array | Yes |
| eventSelector | Any | No |
| targetFilter | Filter expression | No |
| eventFilter | Filter expression | No |
| rawData | Any | No |

```javascript
{
  "eventType": "RunnerFinishedRaceEvent",
  "functions": [
    {
      "function": "append",
      "targetSelector": "$.projection.finishers",
      "eventSelector": "$.event['runnerName','finishTime']"
    }
  ]
}
```

### **prepend**

Adds anything to the beginning of an array.

| Argument | Evaluated Type | Required |
| :--- | :--- | :--- |
| targetSelector | Array | Yes |
| eventSelector | Any | No |
| targetFilter | Filter expression | No |
| eventFilter | Filter expression | No |
| rawData | Any | No |

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

### **remove**

Removes an existing key or an array element matching filter expression.

| Argument | Evaluated Type | Required |
| :--- | :--- | :--- |
| targetSelector | Any | Yes |
| eventSelector | _Not used_ | No |
| eventFilter | _Not used_ | No |
| targetFilter | Filter expression | Yes |
| rawData | _Not used_ | No |

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

## Performing arithmetic

### **add**

Sums two numbers together.

| Argument | Evaluated Type | Required |
| :--- | :--- | :--- |
| targetSelector | Number | Yes |
| eventSelector | Number | No |
| eventFilter | _Not used_ | No |
| targetFilter | _Not used_ | No |
| rawData | Number | No |

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

### **subtract**

Subtracts two numbers.

| Argument | Evaluated Type | Required |
| :--- | :--- | :--- |
| targetSelector | Number | Yes |
| eventSelector | Number | No |
| eventFilter | _Not used_ | No |
| targetFilter | _Not used_ | No |
| rawData | Number | No |

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

### **inc**

Increases a number by one.

| Argument | Evaluated Type | Required |
| :--- | :--- | :--- |
| targetSelector | Number | Yes |
| eventSelector | _Not used_ | No |
| targetFilter | Filter expression | No |
| targetFilter | _Not used_ | No |
| rawData | _Not used_ | No |

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

### **dec**

Decreases a number by one.

| Argument | Evaluated Type | Required |
| :--- | :--- | :--- |
| targetSelector | Number | Yes |
| eventSelector | _Not used_ | No |
| targetFilter | Filter expression | No |
| targetFilter | _Not used_ | No |
| rawData | _Not used_ | No |

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

## Adding lookup via references

You can make your projections

### **setref**

Marks a projection field as a reference which makes it filterable in the API.

| Argument | Evaluated Type | Required |
| :--- | :--- | :--- |
| targetSelector | String/Number/Date | Yes |
| eventSelector | _Not used_ | No |
| targetFilter | _Not used_ | No |
| targetFilter | _Not used_ | No |
| rawData | _Not used_ | No |

```javascript
{
  "eventType": "TicketReleasedEvent",
  "functions": [
    {
      "function": "setref",
      "targetSelector": "$.projection.releaseDate"
    }
  ]
}
```

### **clearref**

Clears the reference for the projection, removing the ability to filter.

| Argument | Evaluated Type | Required |
| :--- | :--- | :--- |
| targetSelector | _Not used_ | No |
| eventSelector | _Not used_ | No |
| targetFilter | _Not used_ | No |
| targetFilter | _Not used_ | No |
| rawData | _Not used_ | No |

```javascript
{
  "eventType": "TicketReleasedEvent",
  "functions": [
    {
      "function": "clearref"
    }
  ]
}
```

## Filters

Functions can also provide two filters: `targetFilter` and `eventFilter`.

To add a filter to a selector you provide a `[?]` in the selector text, as described in the JsonPath documentation. The filter for the selector is then applied for the given function. This is useful for matching on ids in nested lists or to apply conditional logic for when/how to process events.

All Filter Operators that are described [here](https://github.com/json-path/JsonPath) are supported.

