---
transition: slide-up
layout: fact
---

# What about HTTP/2?

<v-click>
Didn't you set as the title of the talk "<i>Implementing HTTP/2 for the Modern Web</i>"?
</v-click>
<br>
<v-click>
What does <b>Undici</b> have to do with <i>HTTP/2</i>?
</v-click>

---
transition: slide-up
layout: fact
---

# Did you know that `undici` has supported HTTP/2 since 2023?

---
transition: slide-up
layout: fact
---

# Is true, check this out:

```js
import { Client } from 'undici';
const client = new Client('https://nghttp2.org/httpbin/json', {
  allowH2: true, // default is false
});
```
<br>
<v-click>
First <a href="https://github.com/nodejs/undici/commit/a8a5d0a3b8638c9b0a65b5adb4defe0e9bc4c63e">commit</a> was introduced in Sep 8, 2023, but it was only available as an experimental feature.
</v-click>


---
transition: slide-up
layout: fact
---

# That's nice, but why?
<v-click>
Doesn't <b>undici</b> literally hints HTTP/1.1 in its name?
</v-click>

---
transition: slide-up
layout: fact
---

# Why not?
<v-click>
Efforts were done before and community requests were there.
</v-click>
<br>
<v-click>
Gave it a look and decided to give it a try and see how far I can go. 
</v-click>
<br>
<br>
<v-click>
After almost ~6 months of work, it got materialized.
</v-click>

---
transition: slide-up
layout: fact
zoom: 0.55
---

# Keep everything the same, but add HTTP/2 support

```js {2|all}
const client = new Client('https://nghttp2.org', {
  allowH2: true, // default is false
});

client.dispatch({
  path: '/httpbin/json',
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
layout: two-cols-header
---

## Same primitives, same API, but now with HTTP/2 support under the hood

::left::
<v-click>
<div class="text-align-left">
<b>Client</b> === <b>Session</b>
<br>
<br>
<b>Pipelining</b> ~= <b>Multiplexing</b>
</div>
</v-click>


::right::
<v-click>
<div class="text-align-center">
<i> Streams are managed under the hood.</i>
</div>
</v-click>

---
transition: slide-up
layout: fact
---

# But that's not it, there's more!

---
transition: slide-up
layout: fact
---

# WebSockets also works over HTTP/2
```js {all|3|all}
import { Agent } from 'undici'

const agent = new Agent({ allowH2: true })

const ws = new WebSocket('wss://echo.websocket.events', {
  dispatcher: agent,
  protocols: ['echo', 'chat']
})
```
<br>
<v-click>
Marked as experimental as per October 2025.
</v-click>


---
transition: slide-up
layout: fact
---

## This paves the way for a future HTTP/3 support

<v-click>
<h5> but that's a topic for another talk </h5>
</v-click>
<br>
<v-click>
<div class="text-sm">
This <a href="https://github.com/nodejs/node/commit/cf91d181fbd00899790852c510bcf4522e8084ea">commit</a> landed as per May 6th, 2026 but still in experimental.
</div>
</v-click>

---
transition: slide-up
---

### Let's crush some numbers

---
transition: slide-up
---

### Numbers goes here

---
transition: slide-left
layout: fact
---

### HTTP/2 is enabled by default in  `undici@8`!

<v-click>
Included in Node.js v26!
</v-click>
<br>
<v-click>
Yes that means that <b>fetch</b> will support it out of the box as well!
</v-click>
<br>
<br>
<v-click>
<div class="text-align-center text-sm">
<i>What a time to be alive isn't it?</i>
</div>
</v-click>
