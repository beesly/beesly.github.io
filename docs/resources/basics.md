## Basics

A `Resource` represents a single HTTP resource, although it may have nested
relationships. A resource is the result of an API call. For instance:

```text
GET /customer/300
```

```text
HTTP/1.1 200 OK
Content-Type: application/hal+json

{
  {
    "_links": {
      "self": {
        "href": "/customer/300"
      }
    }
  },
  "id": 300,
  "name": "ABC Corporation",
  "owner": "Daffy Duck"
}
```

### Defining Resources

A resource can be defined by extending the `Beesly.Resource` class. For
instance, if we wanted to define a resource for our customer response
above:

```js
import {Resource} from 'beesly';

class Customer extends Resource {
  // ...
}
```

### Loading Resources with Data

Once a resource is defined, its constructor will accept an object of data and
hydrate itself:

```js
let customer = new Customer({
  id: 300,
  name: 'ABC Corporation',
});

console.log(customer.name);
// ABC Corporation
```

This data may come from an HTTP request, or you may be building the resource on
the fly in preparation of saving it.

### Accessing Links

If your API returns [hypermedia links](https://tools.ietf.org/html/draft-kelly-json-hal-07#section-4.1.1),
you can retrieve those links via the `getLink()` method, which accepts a name
as an argument.

Using our example above, you could retrieve the `self` link like so:

```js
console.log(customer.getLink('self').href);
// /customer/300
```

The `getLink()` method will return a `Link` object with any variables from the
response hydrated as attributes of the `Link` object. Additionally, the `Link`
object has a `toString()` method such that it can be used as a string.

Finally, the `Link` object contains a `fill(params)` method that will expand
any templated links:

```js
let link = new Beesly.Link({href: '/foo/{bar}'});
console.log(link.fill({foo: '88'}));
// /foo/88
```

### Accessing Embedded Data

If your API returns [embedded resources](https://tools.ietf.org/html/draft-kelly-json-hal-07#section-4.1.2),
you can retrieve those resources by using the name of the relationship as an
accessor method.

For instance, if your API returns:

```json
{
  "id": 300,
  "name": "ABC Corporation",
  "_embedded": {
    "location": [
      {
        "id": 10,
        "address": "123 Main St"
      }
    ]
  }
}
```

You can retrieve a customer's collection of locations using the `location()`
method which is added automatically when an embedded resource with the key
of `location` is encountered:

```js
console.log(customer.location());
// [{"id": 10, "address": "123 Main St"}]
```

If a customer will only ever have one location, it gets pretty awkward to do
`customer.location()[0]` all the time.

Learn how to [manage relationships](managing-relationships.md) better.
