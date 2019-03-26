---
description: >-
  Projections makes it easy to use your events to build models that can be used
  to present your event data.
---

# Getting started

## Projecting events using projections

To present your event data in multiple ways you can define projections that processes your events and generates data structures that can be fetched, listed and searched for in the Serialized API.

### Projection definitions

The first part of projecting data is creating a projection definition. A definition defines what events should be processed and how they should be processed into a data structure that is more suitable for the client to query. 

{% hint style="info" %}
It is a good idea to create multiple projection definitions for different use cases. Projections are cheap views and making them tailored for the client makes your data easy to use.
{% endhint %}

#### Creating your first projection definition

If you performed the you now have a stream of `order` events. To be able to query the current state of a particular order, without having to load all its events, we could configure a projection.

We use a `POST` command to create our projection definition:

{% tabs %}
{% tab title="cURL" %}
```bash
curl -i https://api.serialized.io/projections/definitions \
  --header "Content-Type: application/json" \
  --header "Serialized-Access-Key: <YOUR_ACCESS_KEY>" \
  --header "Serialized-Secret-Access-Key: <YOUR_SECRET_ACCESS_KEY>" \
  --data '
  {
  "projectionName": "orders",
  "feedName": "order",
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
    },
    {
      "eventType": "OrderPaidEvent",
      "functions": [
        {
          "function": "set",
          "targetSelector": "$.projection.status",
          "rawData": "PAID"
        }
      ]
    },
    {
      "eventType": "OrderShippedEvent",
      "functions": [
        {
          "function": "set",
          "targetSelector": "$.projection.status",
          "rawData": "SHIPPED"
        }
      ]
    },
    {
      "eventType": "OrderCancelledEvent",
      "functions": [
        {
          "function": "set",
          "targetSelector": "$.projection.status",
          "rawData": "CANCELLED"
        }
      ]
    }
  ]
}
'
```
{% endtab %}
{% endtabs %}

#### Query our projection

Great! We can now access the projection and check the current state of our order!

{% tabs %}
{% tab title="cURL" %}
```bash
curl -i \
  --header "Serialized-Access-Key: <YOUR_ACCESS_KEY>" \
  --header "Serialized-Secret-Access-Key: <YOUR_SECRET_ACCESS_KEY>" \
  https://api.serialized.io/projections/single/orders/723ecfce-14e9-4889-98d5-a3d0ad54912f
```

{% code-tabs %}
{% code-tabs-item title="Response" %}
```bash
HTTP/1.1 200 OK
Content-Type: application/json
ETag: "c604ac5bd1a51a798accc5717bd5109f"
Last-Modified: Mon, 18 Sep 2017 17:01:23 GMT
Vary: Accept-Encoding
Content-Length: 125

{
  "projectionId":"723ecfce-14e9-4889-98d5-a3d0ad54912f",
  "updatedAt":1505754083976,
  "data": {
    "orderAmount":"12345",
    "status":"PAID"
  }
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}
{% endtab %}
{% endtabs %}

#### Creating aggregated projections

Projections can also be configured to merge events from different aggregates which makes it easy to build simple reporting functionality. Below is an example of an aggregated projection that counts the amount of paid orders and their total amount.

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

#### Query an aggregated projection <a id="query-an-aggregated-projection"></a>

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

