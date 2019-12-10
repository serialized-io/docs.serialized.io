---
description: >-
  Deleting data might be considered unorthodox in the world of event sourcing
  but Serialized is pragmatic regarding this and allows users to delete
  aggregates.
---

# Deleting Aggregates

Serialized supports deleting aggregates including all events, either by a single aggregateId or by aggregate type. The HTTP `DELETE` request will return a `deleteToken` valid for ten minutes.

{% code title="Example deletion of an aggregate" %}
```bash
curl -i \
  --header "Serialized-Access-Key: <YOUR_ACCESS_KEY>" \
  --header "Serialized-Secret-Access-Key: <YOUR_SECRET_ACCESS_KEY>" \
  -X DELETE https://api.serialized.io/aggregates/order

HTTP/1.1 200 OK
Content-Type: application/json
Date: Tue, 12 Jun 2018 15:27:25 GMT
...

{
  "deleteToken": "3c159b36-2840-480b-b436-fa69ac21d5e4"
}
```
{% endcode %}

To permanently delete the aggregate\(s\), re-issue the `DELETE` request with the `deleteToken` appended as a query parameter.

{% code title="Delete aggregate permanently" %}
```bash
curl -i \
  --header "Serialized-Access-Key: <YOUR_ACCESS_KEY>" \
  --header "Serialized-Secret-Access-Key: <YOUR_SECRET_ACCESS_KEY>" \
  -X DELETE https://api.serialized.io/aggregates/order?deleteToken=3c159b36-2840-480b-b436-fa69ac21d5e4
```
{% endcode %}

