# Rate limiting

The returned HTTP headers of any API request show your current rate limit status:

```bash
HTTP/1.1 200 OK
Content-Type: application/json
Serialized-RateLimit-Limit: 10000
Serialized-RateLimit-Remaining: 9999
Serialized-RateLimit-Reset: 1506939198
...

{
  "aggregateId": "..."
}
```

| Header name | Description |
| :--- | :--- |
| Serialized-RateLimit-Limit | The maximum number of requests you are permitted to make per hour. |
| Serialized-RateLimit-Remaining | The number of requests remaining in the current rate limit window. |
| Serialized-RateLimit-Reset | The time at which the current rate limit window resets in [UTC epoch seconds](https://en.wikipedia.org/wiki/Unix_time). |

## Exceeding the limits

If you exceed the rate limit, an error response returns:

```bash
HTTP/1.1 429 Too Many Requests
Content-Type: application/json
Serialized-RateLimit-Limit: 10000
Serialized-RateLimit-Remaining: 0
Serialized-RateLimit-Reset: 1506939198

{
  "code": 429,
  "message": "You have requested this resource too often. Slow down."
}
```

