# Working with aggregates

## Saving events

In this example we will start by saving events for an order. We don’t need to create the order aggregate or define the type explicitly. If an event is the first event for a specific `aggregateId`, it will automatically create a corresponding aggregate type for that id, in this example the aggregate type is `order`.

If we provide multiple events in the API call they will be saved atomically.

The following call will create an aggregate of type `order` with ID `723ecfce-14e9-4889-98d5-a3d0ad54912f` using a HTTP `POST` command:

{% tabs %}
{% tab title="cURL" %}
```bash
curl -i https://api.serialized.io/aggregates/order/723ecfce-14e9-4889-98d5-a3d0ad54912f/events \
  --header "Content-Type: application/json" \
  --header "Serialized-Access-Key: <YOUR_ACCESS_KEY>" \
  --header "Serialized-Secret-Access-Key: <YOUR_SECRET_ACCESS_KEY>" \
  --data '
  {  
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
  }
  '
```
{% endtab %}
{% endtabs %}

This call should result in the following response:

```bash
HTTP/1.1 200 OK
Content-Length: 0
```

## Load aggregate by ID

We can now load our `order` by its `aggregateId`.

{% tabs %}
{% tab title="cURL" %}
```bash
curl -i \
  --header "Serialized-Access-Key: <YOUR_ACCESS_KEY>" \
  --header "Serialized-Secret-Access-Key: <YOUR_SECRET_ACCESS_KEY>" \
  https://api.serialized.io/aggregates/order/723ecfce-14e9-4889-98d5-a3d0ad54912f
```
{% endtab %}
{% endtabs %}

This call should result in the following response:

```bash
HTTP/1.1 200 OK
Content-Type: application/json
Vary: Accept-Encoding
Content-Length: 251

{  
  "aggregateId": "723ecfce-14e9-4889-98d5-a3d0ad54912f",
  "aggregateType": "order",
  "aggregateVersion": 1,
  "events": [  
    {  
       "eventId": "127b80b5-4a05-4774-b870-1c9a2e2a27a3",
       "eventType": "OrderPlacedEvent",
       "data": {  
         "customerId": "some-test-id-1",
         "orderAmount": 12345
       }
    }
  ]
}
```

The events returned can be used to materialize the current state for the order aggregate. How to create an event sourced aggregate is a topic of another guide.

## Appending another event

Let’s pretend this order has been fully paid, so it’s time to add an `OrderPaidEvent`!

We use a `POST` command to add events:

{% tabs %}
{% tab title="cURL" %}
```bash
curl -i https://api.serialized.io/aggregates/order/723ecfce-14e9-4889-98d5-a3d0ad54912f/events \
  --header "Content-Type: application/json" \
  --header "Serialized-Access-Key: <YOUR_ACCESS_KEY>" \
  --header "Serialized-Secret-Access-Key: <YOUR_SECRET_ACCESS_KEY>" \
  --data '
  {  
     "events":[  
        {  
           "eventId":"c8b90e06-f3c0-46aa-93a6-c0b281ef3ac5",
           "eventType":"OrderPaidEvent"
        }
     ]
  }   
  '
```
{% endtab %}
{% endtabs %}

This expected response is:

```bash
HTTP/1.1 200 OK
Content-Length: 0
```

