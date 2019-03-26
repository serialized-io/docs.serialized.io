# Reading events created within two points in time



It’s possible to get all events within a given period by specifying the from/to date/time range.

```text
?from=2018-01-01&to=2018-02-02
```

Default value for `from` is `1970-01-01` and `to` is `now`, if not specified in the request. The string should follow the `ISO 8601` format specified in [RFC 3339, section 5.6](https://tools.ietf.org/html/rfc3339#section-5.6)

