---
description: >-
  There are two ways to handle concurrency. You can either use our built in
  optimistic concurrency control support or make sure you’re always writing data
  to one aggregate using a single writer thread.
---

# Concurrency

Both methods have their pros and cons and are perfectly viable solutions.

## Optimistic Concurrency-Control \(OCC\)

To guarantee consistent writes to an aggregate in a multi-threaded scenario you can use the field `expectedVersion`when posting event batches. When creating a new aggregate the field should be set to `0` to guarantee that no previous events have been stored for that id.

When loading events for an aggregate the response will contain the field `aggregateVersion`. That number is what you are supposed to use as the `expectedVersion` in your next aggregate update. If the aggregate was updated by some other process concurrently, ie. the server-side version check fails, you will get a HTTP 409 \(Conflict\) response back. You will then have to load the aggregate once again, re-apply your change and try to save it again.

## Single-writer

If you are reading and writing aggregates from a single thread or process \(eg. like in the Actor model\) you don’t have to care about the version fields at all.

