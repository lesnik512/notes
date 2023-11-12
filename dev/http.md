## Status codes
- 3хх - additional action required
    - 304 Not Modified - client should use cache
- 4xx - client did something wrong, its better to include information about error
    - 410 Gone - resource was deleted
- 5xx - problem on the server side

### Success responses
- GET: success response - 200
- PUT: success response - 200, 201 (id created)
- DELETE: success response - 204 without body
- POST: success response - 200 or 201 (usually with Location to URL of created resource).

### Additional info
- status 401 Unauthorized must be accompanied by a header WWW-Authenticate and therefore can be used only with HTTP-authentification; in all other cases 403 Forbidden should be used;

# HTTP
## Cache-Control
Tricky example:
```http
Cache-Control: private, no-cache
```

- no-cache - cache always, but check with `If-Match` or `If-Modified-Since`
- private - can be cached in browser, but not in CDN or Proxy

How to forbid cache and remove old cache:

```http
Cache-Control: no-store, max-age=0
```

## HTTP Trailers

Trailers - additional meta data after response body

```http
HTTP/1.1 200 OK
Transfer-Encoding: chunked
Trailer: My-Trailer-Field

[...chunked response body...]

My-Trailer-Field: some-extra-metadata
```
