# beesly

Beesly is a JavaScript library for consuming Hypermedia APIs following the
[HAL specification](http://stateless.co/hal_specification.html).

Beesly provides a means for representing resources, resource collections,
following embedded paths and links.

## A Quick Example

```js
import {Resource} from 'beesly';
import {Customer, LineItem} from './resources';

export class Order extends Resource {
  static get url() {
    return '/order/{id}'
  }

  setup() {
    this.hasOne('customer', {class: Customer});
    this.hasMany('line_item', {class: LineItem, accessor: 'lineItems'});
  }
}
```

```js
import Order from './resources';

var order = Order.get({id: 1000});

console.log(`Order for ${order.customer().name}:`);

order.lineItems().forEach((item) => {
  console.log(`${item.product().name}, ${item.quantity} units`);
})
```

## Table of Contents

 1. [Installation](docs/installation.md)
 2. [Resources](docs/resources/README.md)
   1. [Basics](docs/resources/basics.md)
   2. [Making Requests](docs/resources/making-requests.md)
   3. [Managing Relationships](docs/resources/managing-relationships.md)

## LICENSE

Beesly and its documentation are licensed under the [MIT License](LICENSE.md).
