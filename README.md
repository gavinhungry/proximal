proximal
========
Minimal JSON RPC over HTTP with Proxy/Promise interface.

Installation
------------

    $ npm install proximal

Usage
-----

### Server
```javascript
const proximal = require('proximal');

let rpc = new proximal.Server({
  modules: {
    foo: require('foo'),
    bar: require('bar')
  }
});
```

#### Middleware
```javascript
const express = require('express');
let app = express();

app.post('/rpc', rpc.middleware());
app.listen(8888);
```

### Client
```javascript
let rpc = new proximal.Client({
  url: 'http://example.tld:8888/rpc'
});

let foo = rpc.getModule('foo');

foo.doSomething('arg1', 'arg2').then(result => {
  // your result from remote `foo.doSomething` method here
});

foo.doesNotExist('arg1').then(null, err => {
  // Error: method not found
});

let doesNotExist = rpc.getModule('doesNotExist');
doesNotExist.doSomething().then(null, err => {
  // Error: module not found
});
```

#### Node.js
The client object is designed for use in the browser. However, pass an optional
`XMLHttpRequest` constructor to the `proximal.Client` constructor to use it in
a Node.js environment:

```javascript
let XMLHttpRequest = require('xmlhttprequest').XMLHttpRequest;

let rpc = new proximal.Client({
  url: 'http://example.tld:8888/rpc',
  xhr: XMLHttpRequest
});
```

License
-------
This software is released under the terms of the **MIT license**. See `LICENSE`.
