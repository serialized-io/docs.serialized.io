# Adding filer support to Projections

## Example Events

Given some events that models planning of shipment routes for a cargo shipping company, we could have an event that looks like this:

```javascript
{  
     "aggregateId":"723ecfce-14e9-4889-98d5-a3d0ad54912f",
     "events":[  
        {  
           "eventId":"127b80b5-4a05-4774-b870-1c9a2e2a27a3",
           "eventType":"RoutePlannedEvent",
           "data":{  
              "shipmentCode": "ABC123"
           }
        }
     ]
  }
```

## Projection definition

To make a field filterable you tell Serialized it should be a reference field. You do this by adding a `setref` function to the field you want to be able to filter on.
In this case we want to be able to filter shipments by their shipment code:

```javascript
{
  "aggregated": false,
  "projectionName": "shipments",
  "feedName": "shipment",
  "handlers": [
    {
      "eventType": "RoutePlannedEvent",
      "functions": [
        {
          "function": "merge"
        },
        {
          "function": "setref",
          "targetSelector": "$.projection.shipmentCode"
        }
      ]
    }
  ]
}
```

## Results

You can now query your shipments Projection with a reference that will result i a match for any projection that matches shipmentCode field.

{% hint style="info" %}
The `reference` query parameter is optional
{% endhint %}

{% code title="/projections/single/shipments?reference=ABC123" %}
```javascript
{                                                                  
  "projections" : [ {                                              
    "projectionId" : "723ecfce-14e9-4889-98d5-a3d0ad54912f",       
    "createdAt" : 1523518145532,                                   
    "updatedAt" : 1523518146253,                                   
    "data" : {                                                     
      "shipmentCode": "ABC123"           
    }
  } ],
  "totalCount" : 1
}

```
{% endcode %}

