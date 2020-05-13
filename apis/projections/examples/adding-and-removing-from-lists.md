# Adding and removing from lists

## Example Events

Let's say we're developing support meetings, where participants can be invited and later accept or reject the invitation. In this scenario a reasonable Projection would be to view all participants that currently have accepted their invitation.

An example of a events for two participants that accepted their invitations could look like this:

```javascript
{  
     "aggregateId":"723ecfce-14e9-4889-98d5-a3d0ad54912f",
     "events":[  
        {  
           "eventId":"127b80b5-4a05-4774-b870-1c9a2e2a27a3",
           "eventType":"InvitationAcceptedEvent",
           "data":{  
              "userId": "673b3926-3667-4daf-9db7-03c08adc5ffd"
           }
        }
     ]
  }
```

```javascript
{  
     "aggregateId":"723ecfce-14e9-4889-98d5-a3d0ad54912f",
     "events":[  
        {  
           "eventId":"1a20dfac-ae44-49ac-9491-bcb6d418c4d0",
           "eventType":"InvitationAcceptedEvent",
           "data":{  
              "userId": "fa0d1d7c-9c64-4d65-b000-e1ace3a67bb2"
           }
        }
     ]
  }
```

Later, one of the participants rejects the invitation:

```javascript
  {  
     "aggregateId":"723ecfce-14e9-4889-98d5-a3d0ad54912f",
     "events":[  
        {  
           "eventId":"127b80b5-4a05-4774-b870-1c9a2e2a27a3",
           "eventType":"InvitationRejectedEvent",
           "data":{  
              "userId": "673b3926-3667-4daf-9db7-03c08adc5ffd"
           }
        }
     ]
  }
```

## Projection definition

```javascript
{
  "aggregated": false,
  "projectionName": "meetings",
  "feedName": "meeting",
  "handlers": [
    {
      "eventType": "InvitationAcceptedEvent",
      "functions": [
        {
          "function": "append",
          "targetSelector": "$.projection.participants"
        }
      ]
    },
    {
      "eventType": "InvitationRejectedEvent",
      "functions": [
        {
          "function": "remove",
          "targetSelector": "$.projection.participants[?]",
          "targetFilter": "[?(@.userId == $.event.userId)]"
        }
      ]
    }
  ]
}
```

## Results

{% code title="/projections/single/meetings/723ecfce-14e9-4889-98d5-a3d0ad54912f" %}
```javascript
{
  "projectionId" : "723ecfce-14e9-4889-98d5-a3d0ad54912f",       
  "createdAt" : 1523518145532,                                   
  "updatedAt" : 1523518146253,                                   
  "data": {
    "participants": [{
      "userId": "fa0d1d7c-9c64-4d65-b000-e1ace3a67bb2"
    }]
  }
}
```
{% endcode %}

