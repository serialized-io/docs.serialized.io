# Custom Projection IDs

## Understanding Projection IDs

By default, all Projections use the Aggregate ID from the feed they are created from as the ID of the Projection \(except for aggregated Projections which don't have IDs\).

### Example

Let's say we have an event for an order:

```javascript
{
  "eventId": "f2c8bfc1-c702-4f1a-b295-ef113ed7c8be",
  "eventType": "OrderRegistered",
  "data": {
    "orderId": "ABC-123",
    "amount": 1000,
    "currency": "SEK"
  }
}
```

The Aggregate ID is not shown in the Projection data but you will see it if you take an entry from the Feed instead:

```javascript
{
  "sequenceNumber": 12314,
  "aggregateId": "22c3780f-6dcb-440f-8532-6693be83f21c",
  "timestamp": 1503386583474,
  "events": [
    {
      "eventId": "f2c8bfc1-c702-4f1a-b295-ef113ed7c8be",
      "eventType": "OrderRegistered",
      "data": {
        "orderId": "ABC-123",
        "amount": 1000,
        "currency": "SEK"
      }
    }
  ]
}
```

If we would create a Projection from this order feed we would get a Projection for the Aggregate ID `22c3780f-6dcb-440f-8532-6693be83f21c`. The URL for accessing the Projection data would be:

```text
https://api.serialized.io/projections/single/orders/22c3780f-6dcb-440f-8532-6693be83f21c
```

## Customizing the Projection ID

You can control what ID you want to use for your projections. You can use any primitive field that is in the Projection data as an ID instead of the default Aggregate ID.

To change the ID of your Projections, you use the `idField` value in the Projection definition that you use to configure your Projection. If you want different ID:s for different event types, you can put the `idField` on Handler level instead.

### Example

To use `orderId` from the example above, we could use the following Projection definition:

```javascript
{
  "projectionName": "orders",
  "feedName": "order",
  "idField": "orderId",
  "handlers": [
    {
      "eventType": "OrderPlacedEvent",
      "functions": [
        {
          "function": "set",
          "targetSelector": "$.projection.status",
          "rawData": "PLACED"
        },
        {
          "function": "set",
          "targetSelector": "$.projection.orderAmount",
          "eventSelector": "$.event.orderAmount"
        }
      ]
    }
  ]
}
```

Or, if you're using a custom projection function:

```javascript
{
  "projectionName": "orders",
  "feedName": "order",
  "idField": "orderId",
  "handlers": [
    {
      "eventType": "OrderPlacedEvent",
      "functionUri": "https://example.com/generate-orders-projection"
    }
  ]
}
```

This would result in your Projection being available from the following URL instead:

```text
https://api.serialized.io/projections/single/orders/ABC-123
```

