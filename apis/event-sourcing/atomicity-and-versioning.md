---
description: This page describes atomicity and versioning of Aggregate modifications.
---

# Atomicity and Versioning

## Event Batches

When you update an aggregate you do so by saving batches of events. The reason for this is to ensure atomicity in the case when a command results in multiple events being emitted simultaneously.

## Aggregate versioning   <a id="aggregate-versioning"></a>

All aggregates have a current version. Each batch of events that is saved to the aggregate increments the aggregate version by one, regardless of whether the batch contains a single or multiple events.

