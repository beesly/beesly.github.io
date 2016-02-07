## Managing Relationships

Beesly allows you to declare relationships on resources. Embedded data is
considered a relationship to the root resource. Beesly doesn't make any
distinctions about how resources are related, just that they are and their
cardinality.

### The `setup()` method

Each `Resource` can provide a `setup()` method which will be run each time
a resource is instantiated. It is within this method that we can define the
resources relationships.

```js
import {Resource} from 'beesly';

class Customer extends Resource {
  setup() {
    console.log('setup()');
  }
}
```

### One-to-Many Relationships

Beesly allows for mapping One-to-Many relationships through embedded resources
in the response. Each are defined using the `hasMany(key)` method:

```js
setup() {
  this.hasMany('contact');
}
```

The `key` argument is the key in the embedded collection representing this
relationship:

```json
{
  "_embedded": {
    "contact": [
      {
        "name": "Carl"
      },
      {
        "name": "Jill"
      }
    ]
  }
}
```

This relationship is now accessible through the method that matches the `key`
name:

```js
let customer = Customer.get({id: 300});
console.log(customer.contact());
// [Resource, Resource]
```

The return value will be an array of `Resource` objects. You can customize this
by supplying a [second argument of options](#relationship-options) to
`hasMany()`.

### One-to-One, Many-to-One Relationships

Beesly does not make a distinction between One-to-One and Many-to-One
relationships. Each are defined using the `hasOne(key)` method:

```js
setup() {
  this.hasOne('billing_address');
}
```

The `key` argument is the key in the embedded collection representing this
relationship:

```json
{
  "_embedded": {
    "billing_address": [
      {
        "postal_code": 12345
      }
    ]
  }
}
```

This relationship is now accessible through the method that matches the `key`
name:

```js
let customer = Customer.get({id: 300});
console.log(customer.billing_address());
// Resource
```

The return value will be a singular `Resource` object. You can customize this
by supplying a [second argument of options](#relationship-options) to
`hasMany()`. If more than one resource is stored at `key`, only the first one
will be returned.

Learn more about [using relationships](using-relationships.md).

### Relationship Options

Both `hasOne()` and `hasMany()` accept a second argument of `options`. This
object can have the following keys:

| Option | Description |
| --- | --- |
| `class` | The class to instantiate for this relationship. For `hasOne()`, this will result in a single instantiated object of type `class` returned. For `hasMany()`, this will result in an  array of instantiated objects of type `class` returned. |
| `accessor` | Changes the method name of the `Resource` class used to access a related resource. |

For example, if we wanted to access our `billing_address` with `billingAddress`
instead, and have it instantiate an `Address` resource:

```js
import {Resource} from 'beesly';
import {Address} from './resources';

class Customer extends Resource {
  setup() {
    this.hasOne(
      'billing_address',
      {class: Address, accessor: 'billingAddress'}
    );
  }
}
```

```js
import {Customer} from './resources';

let customer = Customer.get({id: 300});

console.log(customer.billingAddress());
// Address
```
