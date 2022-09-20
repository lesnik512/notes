## Status codes

- 3хх - additional action required
    - 304 Not Modified - client should use cache
- 4xx - client did something wrong, its better to include information about error
    - 410 Gone - resource was deleted
- 5xx - problem on the server side

### Success responses

- GET: success response - 200
- PUT: success response - 200, 201 (id created)
- DELETE: success reponse - 204 without body
- POST: success reponse - 200 or 201 (usually with Location to URL of created resource).

### Additional info

- status 401 Unauthorized must be accompanied by a header WWW-Authenticate and therefore can be used only with HTTP-authentification; in all other cases 403 Forbidden should be used;

## [Capability URLs](https://w3ctag.github.io/capability-urls/)

- for secret parts of URLs should be used strong generator like UUID 4 and should be used sth like `md5(username)`
- HTTPS only
- must be excluded from indexation by robots
- user must be able to revoke URLs
- URLs must have limited period of validity
- not third-party libraries and scripts on these pages, no referers
- its good practice to change location after page loading

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

## HTTP 1XX codes

- `HTTP 100` - server tells by this that client can continue to transfer

It's usefull when client send `Expect: 100-continue`. And this means that client will continue only after server confirm it by `HTTP 100`.

- `HTTP 101` - used for switching between protocols. From HTTP/1.1 to HTTP/2 or to websockets

Client sends:
```http
Connection: upgrade
Upgrade: websocket
```

Server answers:

```http
HTTP/1.1 101 Switching Protocols
Upgrade: websocket
Connection: upgrade
```

- `HTTP 102` - rarely used when response already sent and server is processing it and tells client that everything is OK

- `HTTP 103` - brand new header

## Sources:
- [HTTPWTF. Необычное в обычном протоколе](https://m.habr.com/ru/company/flant/blog/553318/)