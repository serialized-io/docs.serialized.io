# Subscribing to event feeds

## Listing available feeds

As soon as there exists an event for an aggregate type a feed is created. We can see that a feed for our `order`aggregate type is created by listing all available feeds:

{% tabs %}
{% tab title="cURL" %}
```bash
curl -i \
  --header "Serialized-Access-Key: <YOUR_ACCESS_KEY>" \
  --header "Serialized-Secret-Access-Key: <YOUR_SECRET_ACCESS_KEY>" \
  https://api.serialized.io/feeds
```
{% endtab %}
{% endtabs %}

```bash
HTTP/1.1 200 OK
Content-Type: application/json
Vary: Accept-Encoding
Content-Length: 19

{
  "feeds": [ {
    "aggregateType": "order",
    "aggregateCount": 1,
    "batchCount": 2,
    "eventCount": 2
  }
}
```

## Polling order feed for updates

Let’s take a look at our order feed and list all events for all orders

{% tabs %}
{% tab title="cURL" %}
```bash
curl -i \
  --header "Serialized-Access-Key: <YOUR_ACCESS_KEY>" \
  --header "Serialized-Secret-Access-Key: <YOUR_SECRET_ACCESS_KEY>" \
  https://api.serialized.io/feeds/order
```
{% endtab %}
{% endtabs %}

The response below shows us all events \(only two for now\), grouped in batches \(remember that it’s possible to save more than one event at a time\). Each batch has a unique ever-increasing sequence number you can use for tracking which events you have processed. See our [sample code](https://github.com/serialized-io/samples-java/tree/master/event-feed) for an example of how to poll a feed periodically.

```bash
HTTP/1.1 200 OK
Date: Tue, 15 Aug 2017 14:03:55 GMT
Content-Type: application/json
Vary: Accept-Encoding
Content-Length: 484

{  
   "entries":[  
      {  
         "sequenceNumber":1,
         "aggregateId":"723ecfce-14e9-4889-98d5-a3d0ad54912f",
         "timestamp":1504023145574,
         "events":[  
            {  
               "eventId":"127b80b5-4a05-4774-b870-1c9a2e2a27a3",
               "eventType":"OrderPlacedEvent",
               "data":{  
                  "customerId":"some-test-id-1",
                  "orderAmount":12345
               }
            }
         ]
      },
      {  
         "sequenceNumber":2,
         "aggregateId":"723ecfce-14e9-4889-98d5-a3d0ad54912f",
         "timestamp":1504023255370,
         "events":[  
            {  
               "eventId":"c8b90e06-f3c0-46aa-93a6-c0b281ef3ac5",
               "eventType":"OrderPaidEvent"
            }
         ]
      }
   ],
   "hasMore":false
}
```

The query parameter `since` can be appended to the URL to reduce the feed to only include “new” events.

```text
https://api.serialized.io/feeds/order?since=123
```

The field `hasMore` in the feed response indicates whether the returned result got limited \(because of size\) or not. If it’s set to `true` you could immediately send a new poll request, with an updated `since` field, to keep on consuming events.

