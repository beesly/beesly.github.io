## Making Requests

Beesly has built in mechanisms to retrieve, save, and remove resources
by proxying to HTTP calls.

In order to use these mechanisms, you must define a static `url` property
on your resource class:

```js
import {Resource} from 'beesly';

class Customer extends Resource {
  static get url() {
    return '/customer/{id}';
  }
}
```

This directs Beesly which URL to hit when retrieving and saving your resource.
Of course, we don't have a full URL defined above, so the request won't work.
You can define a full URL:

```js
static get url() {
  return 'https://example.com/customer/{id}';
}
```

Or define an Http Request interceptor to modify the URL before making the
request. This latter method allows you to define the base URL differently
depending on which environment you are running on (development, production,
etc).

### GET: Retrieving Data

As a `GET` request is not an operation on an existing resource, it is
called statically on your `Resource` and returns an instance of your
`Resource` class using the `get()` method:

```js
let customer = Customer.get({id: 300});
```

The argument to the `get()` method is an object of parameters that
are applied to the URL. Remember in our example URL, we provided
`/customer/{id}`. When we provide `{id: 300}`, the URL then becomes
`/customer/300`.

Any parameters provided that are not variables within the URL are ignored.

### GET: Retrieving Collections

A `GET` request that returns a
[collection](http://phlyrestfully.readthedocs.org/en/latest/halprimer.html#collections)
should be accessed through the `getCollection()` static method of your
`Resource` class:

```js
let customers = Customer.getCollection();
```



Note that this method, too, can accept an argument of URL parameters to
expand into `url`.
