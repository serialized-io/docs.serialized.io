# List all projections

```text
https://api.serialized.io/projections
```

## Single projections

Single projections creates one projection document per aggregate id in your events, which makes it suitable for “flattening” the events for a particular aggregate making it easier to query.

The projected result will be available below the path `/single/<projection-name>/<id>` eg.

```text
https://api.serialized.io/projections/single/orders
```

and

```text
https://api.serialized.io/projections/single/orders/4e82e594-6409-4f82-bf64-3a6d82a469e9
```

### Aggregated projections

Aggregated projections creates one combined projection from all aggregates in the same event feed. This makes it easy to implement things such as statistics and other kind of reports.

The projected result will be available below the path `/aggregated/<projection-name>` eg.

```text
https://api.serialized.io/projections/aggregated/order-totals
```

