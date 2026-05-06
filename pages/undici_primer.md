---
transition: slide-up
layout: intro
---

# Say _ciao_ to `undici`

<v-click>
<quote> yes a reference to <i>Eleven</i> from Stranger Things
</quote>
</v-click>
<br>
<v-click>
<quote> yes, HTTP/1.1 <- altogether.
</quote>
</v-click>

---
transition: slide-up
---

# A Primer

<v-click>
Let me explain a bit of the key differences and reasons why <i>undici</i> goes brrrrrrr 💨.
</v-click>

<v-click>
<div class="text-sm">
For the rest you can ask <b>Matteo</b>
</div>
</v-click>

---
transition: slide-up
layout: two-cols-header
---

### State Machine vs Event Emitter

::left::
<v-click>
The <i>Dispatcher</i> is the baseline API that allows to react to every state change on the lifetime of an HTTP Request and Response on a callback based approach.
</v-click>

::right::
<v-click>
<div class="text-align-center">
<i>Easier to grasp, less events.</i>
</div>
</v-click>

---
transition: slide-up
background: contain
zoom: 0.8
---

```js {all|1|3-9|9-27}
const client = new Client(`http://localhost:${server.address().port}`)

client.dispatch({
  path: '/',
  method: 'GET',
  headers: {
    'x-foo': 'bar'
  }
}, {
  onRequestStart: () => {
    console.log('Connected!')
  },
  onResponseError: (_controller, error) => {
    console.error(error)
  },
  onResponseStart: (_controller, statusCode, headers) => {
    console.log(`onResponseStart | statusCode: ${statusCode} | headers: ${JSON.stringify(headers)}`)
  },
  onResponseData: (_controller, chunk) => {
    console.log('onResponseData: chunk received')
  },
  onResponseEnd: (_controller, trailers) => {
    console.log(`onResponseEnd | trailers: ${JSON.stringify(trailers)}`)
    client.close()
    server.close()
  }
})
```

---
transition: slide-up
layout: section
---

### `pipelining` + `keep-alive` === brrr 💨


<div class="text-align-left">
<b>pipelining</b> is customizable allowing you to manage concurrency,
while <b>keep-alive</b> is the default connection behavior (saving some roundtrips), and can be disabled if needed.
</div>

<br>
```js {all|1|2-3|all}
const client = new Client(`http://localhost:${server.address().port}`, {
  pipelining: 4, // default is 1
  keepAliveTimeout: 1000 // default is 600_000ms
})
```

---
transition: slide-up
layout: image-right
image: https://upload.wikimedia.org/wikipedia/commons/thumb/1/1f/WebAssembly_Logo.svg/1280px-WebAssembly_Logo.svg.png
backgroundSize: contain
---

## Powered by WASM

WASM allows `undici` to embedd `llhttp` in an easier way and reduce the overall overhead cost of the C/C++(JS) transmission.

<v-click>
<div class="text-sm">
<i>What a perfect use case for WASM isn't it?</i>
</div>
</v-click>

---
transition: slide-up
layout: section
---

# Want a connection pooling?

---
transition: slide-up
---

## Booom! `Pool`

```js
new Pool('http://myserver.com', {
  connections: 10, // default is 1
})
```

---
transition: slide-up
layout: section
---

# Want smart retrying?

---
transition: slide-up
layout: section
---

## Booom! `interceptors.retry`

```js
const { Client, interceptors } = require("undici");
const { retry } = interceptors;

const client = new Client("http://service.example").compose(
  retry({
    maxRetries: 3,
    minTimeout: 1000,
    maxTimeout: 10000,
    timeoutFactor: 2,
    retryAfter: true,
  })
);

```
---
transition: slide-up
layout: section
---

# Want smart DNS caching?

---
transition: slide-up
layout: section
zoom: 0.65
---

## Booom! `interceptors.dns`

```js {all|2-3|7-25|27-29|all}
const { Client, interceptors } = require("undici");
const QuickLRU = require("quick-lru");
const { dns } = interceptors;

const lru = new QuickLRU({ maxSize: 100 });

const lruAdapter = {
  get size() {
    return lru.size;
  },
  get(origin) {
    return lru.get(origin);
  },
  set(origin, records, { ttl }) {
    lru.set(origin, records, { maxAge: ttl });
  },
  delete(origin) {
    lru.delete(origin);
  },
  full() {
    // For LRU cache, we can always store new records,
    // old records will be evicted automatically
    return false;
  }
}

const client = new Agent().compose([
  dns({ storage: lruAdapter })
])
```
---
transition: slide-up
layout: section
---

# Want smart `SOCKS5` Support?

---
transition: slide-up
layout: section
---

## Booom! `Socks5ProxyAgent`

```js
import { Socks5ProxyAgent } from 'undici'

const socks5Proxy = new Socks5ProxyAgent('socks5://localhost:1080')
// or with authentication
const socks5ProxyWithAuth = new Socks5ProxyAgent('socks5://user:pass@localhost:1080')
// or with options
const socks5ProxyWithOptions = new Socks5ProxyAgent('socks5://localhost:1080', {
  username: 'user',
  password: 'pass',
  connections: 10
})

```
---
transition: slide-left
layout: section
---

### Do you know that `undici` also supports powers `fetch` in Node.js?

<v-click>
<div class="text-align-left">
So yes, you can have all of these mounted on top of <b>fetch</b> as well.
</div>
</v-click>