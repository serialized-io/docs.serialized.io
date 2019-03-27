---
description: >-
  The Event Feed API automatically exposes a unique feed of events for each
  aggregate type that you can use to build queryable data or implement
  side-effects to connect your system to other services.
---

# Feeds API

In a CQRS architecture it typically means operations performed by the **query**-side of the application.

## Feeding events from Serialized

### Reading all events since the beginning

Simply pass a zero, or exclude the parameter from the request to receive everything since the beginning.

```text
?since=0
```

### Reading all events since the beginning

Append the `sequenceNumber` of the last received event as a `since` parameter to limit the response to only include events since then.

```text
?since=1234
```

### Reading events between two points in time

It’s possible to get all events within a given period by specifying the from/to date/time range.

```text
?from=2018-01-01&to=2018-02-02
```

Default value for `from` is `1970-01-01` and `to` is `now`, if not specified in the request. The string should follow the `ISO 8601` format specified in [RFC 3339, section 5.6](https://tools.ietf.org/html/rfc3339#section-5.6)

