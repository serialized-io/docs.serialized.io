# Aggregating data to a list

## Example Events

A common use-case is collecting some data across all aggregates for a given type. In the example below we want to be able to get a list of all registered Customers from a single query to a Projection.

Let's say two Customers have been registered in our system with the fo

```javascript
  {  
     "aggregateId":"723ecfce-14e9-4889-98d5-a3d0ad54912f",
     "events":[  
        {  
           "eventId":"127b80b5-4a05-4774-b870-1c9a2e2a27a3",
           "eventType":"CustomerRegisteredEvent",
           "data":{  
              "customer": "Acme Inc"
           }
        }
     ]
  }
```

```javascript
  {  
     "aggregateId":"cf26e6df-a883-4fbc-929b-47052f0acdb8",
     "events":[  
        {  
           "eventId":"982f8747-ba7d-470e-beca-1582c48d3181",
           "eventType":"CustomerRegisteredEvent",
           "data":{  
              "customer": "The Trading Company"
           }
        }
     ]
  }
```

## Projection definition

We can create an aggregated Projection that pushes all Customer names to a field in our Aggregated Projection called `registered-customers`.

```javascript
{
  "aggregated": true,
  "projectionName": "registered-customers",
  "feedName": "customer",
  "handlers": [
    {
      "eventType": "CustomerRegisteredEvent",
      "functions": [
        {
          "function": "push",
          "eventSelector": "$.event.customer",
          "targetSelector": "$.projection.customers"
        }
      ]
    }
  ]
}
```

## Results

When we query the Aggregated Projection we will now get the following result.

{% code-tabs %}
{% code-tabs-item title="/projections/aggregated/registered-customers" %}
```javascript
{
  "projectionId" : "registered-customers",       
  "createdAt" : 1523518145532,                                   
  "updatedAt" : 1523518146253,                                   
  "data": {
    "customers": ["Acme Inc", "The Trading Company"]
  }
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

