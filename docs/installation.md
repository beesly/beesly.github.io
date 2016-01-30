## Installation

Beesly can be installed via npm:

```text
npm i --save beesly
```

Beesly exposes itself via a global variable `Beesly` when in a web browser
environment:

```js
console.log(Beesly);
```

And available via Node's `require`:

```js
var Beesly = require('Beesly');
```

Or using ES2015's module system:

```js
import Beesly from 'beesly';
```

Or

```js
import {Resource} from 'beesly';
```
