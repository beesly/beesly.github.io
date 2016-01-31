## Response

The `Beesly.Response` object represents an HTTP response.

```js
new Response(code, body, headers);
```

The attributes are exposed via accessors with the same name as the
constructor arguments:

| Attribute | Description |
| --- | --- |
| `code` | The status code for the HTTP response |
| `body` | The body of the HTTP response |
| `headers` | An object representing the HTTP headers of the response |

Additionally, the following accessors are available:

| Method | Description |
| --- | --- |
| `json` | Attempts to transform the response body to JSON |
| `text` | Returns the raw response body |
