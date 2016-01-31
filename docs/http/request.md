## Request

The `Beesly.Request` object represents an HTTP request.

```js
new Request(url, data, headers = {});
```

The attributes are exposed via accessors with the same name as the
constructor arguments:

| Attribute | Description |
| --- | --- |
| `url` | The url for the HTTP request |
| `data` | The raw data of the HTTP request |
| `headers` | An object representing the HTTP headers of the request |

<div class="notice">
Before the 1.0 stable release, <code>data</code> attribute will likely be
renamed to <code>content</code>.
</div>
