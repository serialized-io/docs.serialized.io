# Table of contents

* [Serialized API reference](README.md)

## Basics

* [Accounts](basics/untitled.md)
* [Projects](basics/projects/README.md)
  * [Multi-tenant projects](basics/projects/multi-tenant-projects.md)
* [Authentication](basics/authentication.md)

## Concepts

* [Event Sourcing](concepts/event-sourcing/README.md)
  * [Storing events](concepts/event-sourcing/storing-events.md)
  * [Loading an aggregate](concepts/event-sourcing/loading-an-aggregate.md)
  * [Deleting aggregates](concepts/event-sourcing/deleting-aggregates.md)
* [Event Feed](concepts/event-feed/README.md)
  * [List event feeds](concepts/event-feed/list-event-feeds.md)
  * [Read events by aggregate type](concepts/event-feed/read-events-by-aggregate-type.md)
  * [Using the \_all feed](concepts/event-feed/using-the-_all-feed.md)
  * [Reading all events since the beginning](concepts/event-feed/reading-all-events-since-the-beginning.md)
  * [Reading new events since last received event](concepts/event-feed/reading-new-events-since-last-received-event.md)
  * [Reading events created within two points in time](concepts/event-feed/reading-events-created-within-two-points-in-time.md)
* [Projection](concepts/projection/README.md)
  * [Create projection definitions](concepts/projection/create-projection-definitions.md)
  * [List all projections](concepts/projection/list-all-projections.md)
  * [Defining event handlers](concepts/projection/defining-event-handlers.md)
  * [External projector functions](concepts/projection/external-projector-functions.md)
* [Reaction](concepts/reaction.md)

## Reference

* [Event Sourcing](reference/event-sourcing/README.md)
  * [Loading aggregate events](reference/event-sourcing/load-all-events-for-an-aggregate.md)
  * [Store events](reference/event-sourcing/store-events.md)
  * [Check if an aggregate exists](reference/event-sourcing/check-if-an-aggregate-exists.md)
  * [Delete an aggregate](reference/event-sourcing/delete-an-aggregate.md)
  * [Delete all aggregates by type](reference/event-sourcing/delete-all-aggregates-by-type.md)
* [Feeds](reference/feeds/README.md)
  * [List feeds](reference/feeds/list-feeds.md)
  * [Get feed of all events](reference/feeds/get-all-feed.md)
  * [Get current global sequence number](reference/feeds/get-global-sequence-number.md)
  * [Get current sequence number](reference/feeds/get-sequence-number.md)
  * [Get feed of events](reference/feeds/get-feed.md)
* [Reactions](reference/reactions/README.md)
  * [List reaction definitions](reference/reactions/list-reaction-definitions.md)
  * [Create reaction definitions](reference/reactions/create-reaction-definition.md)
  * [Delete reaction definitions](reference/reactions/delete-reaction-definition.md)
  
## Tools

* [Projection Tester](https://console.serialized.io/projection-tester)

