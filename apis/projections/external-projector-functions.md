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

## Implementing a custom function

### Handling the request

To implement an custom function you need to implement a publicly available backend service that can receive a JSON payload that looks like this:

```javascript
{
  "metadata": {
    "aggregateId": "a341b64c-b01f-43fb-907c-50c0067df672",
    "createdAt": 1535442699551,
    "updatedAt": 1535442699551
  },
  "currentState": {},
  "event": {
    "eventId": "d710b9b1-063a-4b65-98be-0d46de443bdd",
    "eventType": "UserLoggedInEvent",
    "data": {
      "userId": "618f5a47-9d4c-4f42-9380-ca47c12087a1"
    }
  }
}
```

### Producing a response

The response from the custom function should be the new updated state of the projection. The format of the updated state must look like this:

```javascript
{
    "updatedState": {
        "loginCount": 1
    }
}
```

### **Securing external function endpoints**

The endpoints can be secured with Basic Authentication by including user:password in the `functionUri` like this:

```javascript
{
  "eventType": "<YOUR_EVENT_TYPE>",
  "functionUri": "https://user:password@example.com/your-function"
}
```

### Sample code

For a full example of an implementation of an external custom function using AWS Lambda see [our Github repository](https://github.com/serialized-io/samples-java/tree/master/event-projector-lambda).

