# APIs: Server Side Implementation Notes

_(c) AMWA 2023, CC Attribution-NoDerivatives 4.0 International (CC BY-ND 4.0)_

## Cross-Origin Resource Sharing (CORS)

In order to permit web-based control interfaces to be hosted remotely, all NMOS APIs MUST implement valid CORS HTTP headers in responses to all requests.

In addition to this, where highlighted in the API specifications servers MUST respond to HTTP pre-flight `OPTIONS` requests. Servers MAY additionally support HTTP `OPTIONS` requests made to any other API resource.

In order to simplify development, the following headers MAY be returned in order to remove these restrictions as far as possible. Note that these are very relaxed and possibly not suitable for a production deployment.

```http
Access-Control-Allow-Origin: *
Access-Control-Allow-Methods: GET, PUT, POST, PATCH, HEAD, OPTIONS, DELETE
Access-Control-Allow-Headers: Content-Type, Accept
Access-Control-Max-Age: 3600
```

To ensure compatibility, the response `Access-Control-Allow-Headers` could be set from the request's `Access-Control-Request-Headers`.
