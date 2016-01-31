## HTTP Library

Beesly ships with a naive HTTP library that wraps the native
[XMLHttpRequest](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest)
functionality. This library is used internally by Beesly, but is exposed via
the projects exports in case it is needed (for instance, to allow `instanceof`
checks when writing interceptors).

Once the [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API)
matures and gains more support within browsers and Node, Beesly will likely
remove its HTTP library and opt to use the Fetch API instead.

Consideration was made to use several Fetch polyfills, however, at the time
of initial development, none of them supported the `Request` and `Response`
objects, only `fetch()` itself. The lack of which made it difficult to
provide the HTTP interceptors.

### The Http Class

The `Http` class has several convenience methods for making HTTP requests:

| Method | Description |
| --- | --- |
| `get()` | Performs a HTTP `GET` request |
| `post()` | Performs a HTTP `POST` request |
| `patch()` | Performs a HTTP `PATCH` request |
| `put()` | Performs a HTTP `PUT` request |
| `delete()` | Performs a HTTP `DELETE` request |

Each of these methods accepts a `Request` object, and each returns a `Promise`
that will either be resolved or rejected with a `Response` object.

A response is resolved whenever the status code of the response is >= 200 and
< 400. If the response's status code falls outside of this range, the promise
is rejected.

The `Http` class also exposes a `send(method, request)` method which works the
same as the individual verb methods outlined above.

<div class="notice">
Before the 1.0 stable release, the individual verb methods will
likely be removed in favor of placing the method on the <code>Request</code>
object and simply calling <code>send()</code>.
</div>
