# Headers

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