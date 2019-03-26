# Table of contents

* [Serialized API reference](README.md)

## Basics

* [Accounts](basics/accounts.md)
* [Projects](basics/projects/README.md)
  * [Multi-tenant projects](basics/projects/multi-tenant-projects.md)
* [Authentication](basics/authentication.md)

## Projections

* [Getting started](projections/getting-started.md)
* [Create projection definitions](projections/create-projection-definitions.md)
* [List all projections](projections/list-all-projections.md)
* [Defining event handlers](projections/defining-event-handlers.md)
* [External projector functions](projections/external-projector-functions.md)
* [Examples](projections/examples.md)

## Concepts

* [Event Sourcing](concepts/event-sourcing/README.md)
  * [Loading an aggregate](concepts/event-sourcing/loading-an-aggregate.md)
  * [Deleting aggregates](concepts/event-sourcing/deleting-aggregates.md)
* [Event Feed](concepts/event-feed/README.md)
  * [Using the \_all feed](concepts/event-feed/using-the-_all-feed.md)
  * [Reading all events since the beginning](concepts/event-feed/reading-all-events-since-the-beginning.md)
  * [Reading new events since last received event](concepts/event-feed/reading-new-events-since-last-received-event.md)
  * [Reading events created within two points in time](concepts/event-feed/reading-events-created-within-two-points-in-time.md)
* [Projection](concepts/examples.md)
* [Reaction](concepts/reaction.md)

## Reference

* [Event Sourcing API](reference/event-sourcing/README.md)
  * [Loading aggregate events](reference/event-sourcing/load-all-events-for-an-aggregate.md)
  * [Store events](reference/event-sourcing/store-events.md)
  * [Check if an aggregate exists](reference/event-sourcing/check-if-an-aggregate-exists.md)
  * [Delete an aggregate](reference/event-sourcing/delete-an-aggregate.md)
  * [Delete all aggregates by type](reference/event-sourcing/delete-all-aggregates-by-type.md)
* [Feeds API](reference/feeds/README.md)
  * [List feeds](reference/feeds/list-feeds.md)
  * [Get feed of all events](reference/feeds/get-all-feed.md)
  * [Get current global sequence number](reference/feeds/get-global-sequence-number.md)
  * [Get current sequence number](reference/feeds/get-sequence-number.md)
  * [Get feed of events](reference/feeds/get-feed.md)
* [Reactions API](reference/reactions/README.md)
  * [Create reaction definition](reference/reactions/create-reaction-definition.md)
  * [Get reaction definition](reference/reactions/get-reaction-definition.md)
  * [Update reaction definition](reference/reactions/update-reaction-definition.md)
  * [Delete reaction definition](reference/reactions/delete-reaction-definition.md)
  * [List reaction definitions](reference/reactions/list-reaction-definitions.md)
* [Projections API](reference/projections/README.md)
  * [Create projection definition](reference/projections/create-projection-definition.md)
  * [Get projection definition](reference/projections/get-projection-definition.md)
  * [Update projection definition](reference/projections/update-projection-definition.md)
  * [Delete projection definition](reference/projections/delete-projection-definition.md)
  * [Get projections overview](reference/projections/get-projections-overview.md)
  * [List projection definitions](reference/projections/list-projection-definitions.md)
  * [List single projections](reference/projections/list-single-projections.md)
  * [Delete/recreate single projections](reference/projections/delete-single-projections.md)
  * [List aggregated projections](reference/projections/list-aggregated-projections.md)
  * [Get aggregated projection](reference/projections/get-aggregated-projection.md)
  * [Delete/recreate aggregated projections](reference/projections/delete-aggregated-projections.md)

## Tools

* [Projection Tester](https://console.serialized.io/projection-tester)

