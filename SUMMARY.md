# Table of contents

* [Welcome to Serialized](README.md)

## Basics

* [Accounts](basics/accounts.md)
* [Projects](basics/projects/README.md)
  * [Multi-tenant projects](basics/projects/multi-tenant-projects.md)
* [Authentication](basics/authentication.md)
* [Getting started](basics/getting-started/README.md)
  * [Working with aggregates](basics/getting-started/working-with-aggregates.md)
  * [Projecting events](basics/getting-started/projections.md)
  * [Aggregating events](basics/getting-started/aggregating-events.md)
  * [Reacting to events](basics/getting-started/reacting-to-events.md)
  * [Subscribing to event feeds](basics/getting-started/subscribing-to-event-feeds.md)

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

## Reference

* [Event Sourcing API](reference/event-sourcing/README.md)
  * [Loading aggregate events](reference/event-sourcing/load-all-events-for-an-aggregate.md)
  * [Store events](reference/event-sourcing/store-events.md)
  * [Check if an aggregate exists](reference/event-sourcing/check-if-an-aggregate-exists.md)
  * [Delete an aggregate](reference/event-sourcing/delete-an-aggregate.md)
  * [Delete all aggregates by type](reference/event-sourcing/delete-all-aggregates-by-type.md)
* [Feeds API](reference/feeds/README.md)
  * [Reference](reference/feeds/reference/README.md)
    * [List feeds](reference/feeds/reference/list-feeds.md)
    * [Get feed of all events](reference/feeds/reference/get-all-feed.md)
    * [Get current global sequence number](reference/feeds/reference/get-global-sequence-number.md)
    * [Get current sequence number](reference/feeds/reference/get-sequence-number.md)
    * [Get feed of events](reference/feeds/reference/get-feed.md)
* [Reactions API](reference/reactions/README.md)
  * [Configuring execution time](reference/reactions/configuring-execution-time.md)
  * [Actions](reference/reactions/reaction.md)
  * [Reference](reference/reactions/reference/README.md)
    * [Create reaction definition](reference/reactions/reference/create-reaction-definition.md)
    * [Get reaction definition](reference/reactions/reference/get-reaction-definition.md)
    * [Update reaction definition](reference/reactions/reference/update-reaction-definition.md)
    * [Delete reaction definition](reference/reactions/reference/delete-reaction-definition.md)
    * [List reaction definitions](reference/reactions/reference/list-reaction-definitions.md)
* [Projections API](reference/projections/README.md)
  * [Event handlers](reference/projections/event-handlers.md)
  * [External projector functions](reference/projections/external-projector-functions.md)
  * [Reference](reference/projections/reference/README.md)
    * [Create projection definition](reference/projections/reference/create-projection-definition.md)
    * [Get projection definition](reference/projections/reference/get-projection-definition.md)
    * [Update projection definition](reference/projections/reference/update-projection-definition.md)
    * [Delete projection definition](reference/projections/reference/delete-projection-definition.md)
    * [Get projections overview](reference/projections/reference/get-projections-overview.md)
    * [List projection definitions](reference/projections/reference/list-projection-definitions.md)
    * [List single projections](reference/projections/reference/list-single-projections.md)
    * [Delete/recreate single projections](reference/projections/reference/delete-single-projections.md)
    * [List aggregated projections](reference/projections/reference/list-aggregated-projections.md)
    * [Get aggregated projection](reference/projections/reference/get-aggregated-projection.md)
    * [Delete/recreate aggregated projections](reference/projections/reference/delete-aggregated-projections.md)

## Tools

* [Projection Tester](https://console.serialized.io/projection-tester)

