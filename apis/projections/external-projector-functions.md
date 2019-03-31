---
description: >-
  This page describes how to write your own custom handler function to make it
  possible to implement more complex projections.
---

# Custom Functions

You can write your own projector code and deploy it as a function wherever you prefer. AWS Lambda, Google Cloud Functions and Azure Functions are all viable alternatives.

## Executing custom functions

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

### Request to the external function

**&lt;todo&gt;**

