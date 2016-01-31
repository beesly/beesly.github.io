## Using Interceptors

Interceptors allow for intercepting an HTTP request and modifying the
`Request` object before it is dispatched to the server.

### Registering an Interceptor

Interceptors are registered as a callback with the `Http` object:

```js
import {Http} from 'beesly';

Http.addIntercept((request) => {
  request.url = `http://api.dev${request.url}`;
});
```

When an HTTP request is made, the `Request` object is passed to each
interceptor, allowing it to perform necessary modifications to the
request.

Interceptors are run in a first-in, first-out (FIFO) basis:

```js
import {Http, Request} from 'beesly';

Http.addIntercept((request) => {
  request.url = `http://api.dev${request.url}`;
});

Http.addIntercept((request) => {
  request.url = `https://production.com${request.url}`;
});

let request = new Request('delete', '/foobar');
new Http().send(request);

console.log(request.url);
// https://production.com/foobar
```

### Use Cases

Interceptors are great for modifying the base URL, as shown in the examples
above. If you have separate environments with configuration files, you can
easily have resources that are ignorant of the environment, but are supplied
a full URL through interceptors:

```js
import {Http} from 'beesly';
import config from './config';

Http.addIntercept((request) => {
  request.url = `${config.baseUrl}${request.url}`;
});
```

You can also supply authorization headers for logged in users to pass along
tokens to the API:

```js
import {Http} from 'beesly';
import me from './me';

Http.addIntercept((request) => {
  request.headers.Authorization = `Bearer ${me.accessToken}`;
});
```

### Bad Use Cases

You should not use interceptors to modify specific requests. Instead, use
a [data transformer](/docs/resources/data-transformers.md) for the specific
`Resource` you are working with.
