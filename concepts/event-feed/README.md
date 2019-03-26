# Event Feed

## Using the Event Feed API

The Event Feed API automatically exposes a unique feed of events for each aggregate type that you can use to build queryable data or implement side-effects to connect your system to other services. In a CQRS architecture it typically means operations performed by the **query**-side of the application.

### Sequence numbers

When an event batch is persisted in the store it is assigned a sequence number. Each feed has its own ever increasing sequence number. This number is what the feed subscribers use to keep track of how much of the feed has been consumed. The number is guaranteed to be a unique, ever increasing, positive integer.

