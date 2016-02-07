## Using Relationships

In the previous section, we learned how to [manage relationships](managing-relationships.md).
Once we've defined a relationship like so:

```js
import {Resource} from 'beesly';

class Order extends Resource {
  setup() {
    this.hasOne('customer');
    this.hasMany('items');
  }
}
```

We can access these relationships using accessor methods:

```js
const order = new Order({...});
console.log(`Order for ${order.customer().name}`);
console.log(`Total items: ${order.items().length}`);
```

These accessor methods act as both a setter and a getter for the embedded resource data.

### Using `hasOne()` Accessors

When we define a One-to-One or Many-to-One relationship using `hasOne()`, we get an accessor
that returns a single resource (even though in HAL, it's represented as an array):

```js
const order = new Order({
  _embedded: {
    customer: [
      {id: 100}
    ]
  }
});
console.log(order.customer().id);
// 100
```

Beesly simply grabs the first item of the internal array and returns it.

If we wanted to set the customer data:

```js
order.customer({id: 900});
console.log(order.customer().id);
// 100
```

Beesly replaces the entire `customer` embedded data with an array containing just the item
you pass it to the setter.

If we were to dump the object:

```js
console.log(order.asHalJson());
```

We'd get something like this:

```json
{
  "_embedded": {
    "customer": [
      {
        "id": 900
      }
    ]
  }
}
```

### Using `hasMany()` Accessors

When we define a One-to-Many relationship using `hasMany()`, we get an accessor
that returns the collection of embedded resources:

```js
const order = new Order({
  _embedded: {
    items: [
      {id: 10},
      {id: 20},
      {id: 25}
    ]
  }
});
console.log(order.orders().map(o => o.id));
// [10, 20, 25]
```

If we wanted to add an order to this collection, we'd pass an object to the accessor method:

```js
order.orders({id: 37}).map(o => o.id));
// [10, 20, 25, 37]
```

When a non-array, non-undefined value is passed to a `hasMany()` accessor, the method is treated
as an adder.

If we passed an array to the accessor:

```js
order.orders([{id: 37}]).map(o => o.id));
// [37]
```

The method is treated as a setter, and replaces the entire collection with what is provided.

### Removing Data

If you wanted to remove embedded resources, you'd simply pass `null` to the accessor for both
types of relationships:

```js
order.customer(null);
order.items(null);
console.log(order.customer());
// undefined
console.log(order.items());
// []
```

If you wanted to remove a single element of a `hasMany()` relationship, you can retrieve the
collection, make the necessary mutations, and reset it in its entirety:

```js
const order = new Order({
  _embedded: {
    items: [
      {id: 10},
      {id: 20},
      {id: 25}
    ]
  }
});

const items = order.items();
order.items(items.slice(1));
console.log(order.orders().map(o => o.id));
// [20, 25]
```
