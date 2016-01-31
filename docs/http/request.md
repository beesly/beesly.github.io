## Request

The `Beesly.Request` object represents an HTTP request.

```js
new Request(url, content, headers = {});
```

The attributes are exposed via accessors with the same name as the
constructor arguments:

| Attribute | Description |
| --- | --- |
| `url` | The url for the HTTP request |
| `content` | The raw content of the HTTP request |
| `headers` | An object representing the HTTP headers of the request |

Additionally, the following methods are available to manage headers:

| Method | Description |
| --- | --- |
| `hasHeader(key)` | Returns whether a given header exists |
| `getHeader(key)` | Retrieves the header by key, or returns undefined if not defined |
| `setHeader(key, value)` | Sets or replaces a header value |
