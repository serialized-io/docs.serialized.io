# Event Sourcing

## Event batches

When you update an aggregate you do so by saving batches of events. The reason for this is to ensure atomicity in the case when a command results in multiple events being emitted simultaneously.

## Aggregate versioning  <a id="aggregate-versioning"></a>

All aggregates have a current version. Each batch of events that is saved to the aggregate increments the aggregate version by one, regardless of whether the batch contains a single or multiple events.

## Concurrency

There are two ways to handle concurrency. You can either use our built in optimistic concurrency control support or make sure you’re always writing data to one aggregate using a single writer thread. Both methods have their pros and cons and are perfectly viable solutions.

### Optimistic Concurrency-Control \(OCC\)

To guarantee consistent writes to an aggregate in a multi-threaded scenario you can use the field `expectedVersion`when posting event batches. When creating a new aggregate the field should be set to `0` to guarantee that no previous events have been stored for that id.

When loading events for an aggregate the response will contain the field `aggregateVersion`. That number is what you are supposed to use as the `expectedVersion` in your next aggregate update. If the aggregate was updated by some other process concurrently, ie. the server-side version check fails, you will get a HTTP 409 \(Conflict\) response back. You will then have to load the aggregate once again, re-apply your change and try to save it again.

### Single-writer

If you are reading and writing aggregates from a single thread or process \(eg. like in the Actor model\) you don’t have to care about the version fields at all.

