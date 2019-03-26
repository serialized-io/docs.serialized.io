---
description: >-
  Projections can also be configured to merge events from different aggregates
  which makes it easy to build simple reporting functionality. Below is an
  example of an aggregated projection that counts th
---

# Aggregating events

## Creating aggregated projections

We use a `POST` command to create a new projection definition:

{% tabs %}
{% tab title="cURL" %}
```bash
curl -i https://api.serialized.io/projections/definitions \
  --header "Content-Type: application/json" \
  --header "Serialized-Access-Key: <YOUR_ACCESS_KEY>" \
  --header "Serialized-Secret-Access-Key: <YOUR_SECRET_ACCESS_KEY>" \
  --data '
  {
    "projectionName": "order-totals",
    "feedName": "order",
    "aggregated": true,
    "handlers": [
      {
        "eventType": "OrderPlacedEvent",
        "functions": [
          {
            "function": "add",
            "targetSelector": "$.projection.orderAmount",
            "eventSelector": "$.event.orderAmount"
          },
          {
            "function": "inc",
            "targetSelector": "$.projection.orderCount"
          }
        ]
      }
    ]
  }
  '
```
{% endtab %}
{% endtabs %}

## Query an aggregated projection

Since aggregated projections are combinations of multiple aggregates they don’t have any id’s. We query them directly by their name:

{% tabs %}
{% tab title="cURL" %}
```bash
curl -i \
  --header "Serialized-Access-Key: <YOUR_ACCESS_KEY>" \
  --header "Serialized-Secret-Access-Key: <YOUR_SECRET_ACCESS_KEY>" \
  https://api.serialized.io/projections/aggregated/order-totals
```

{% code-tabs %}
{% code-tabs-item title="Response" %}
```bash
HTTP/1.1 200 OK
Content-Type: application/json
Date: Wed, 20 Sep 2017 11:45:27 GMT
ETag: "7b2b05dab4a33088d9aee48b90c4a786"
Last-Modified: Tue, 19 Sep 2017 19:53:08 GMT
Serialized-RateLimit-Limit: 10000
Serialized-RateLimit-Remaining: 9995
Serialized-RateLimit-Reset: 1505911185
Vary: Accept-Encoding
Content-Length: 91
Connection: keep-alive

{
  "projectionId": "order-totals",
  "updatedAt": 1505850788368,
  "data": {
    "orderAmount":"1000",
    "orderCount":"2"
  }
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}
{% endtab %}
{% endtabs %}

Given that we have two `OrderPlacedEvent` that together sum up to 1000 we should get the response above-

