# Adding and removing from lists

Let's say we're developing support meetings, where participants can be invited and later accept or reject the invitation. In this scenario a reasonable Projection would be to view all participants that currently have accepted their invitation.

An example of an event for an accepted invitation could look like this:

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

And a corresponding rejected invitation:

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

### Projecting the list with push and remove

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
          "function": "push",
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

The result of this example would render a meeting projection that looks like this:

```javascript
{
  "projectionId" : "723ecfce-14e9-4889-98d5-a3d0ad54912f",       
  "createdAt" : 1523518145532,                                   
  "updatedAt" : 1523518146253,                                   
  "data": {
    "participants": [{
      "userId": "673b3926-3667-4daf-9db7-03c08adc5ffd"
    }]
  }
}
```



