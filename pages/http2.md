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
layout: image-right
image: https://media.giphy.com/media/v1.Y2lkPTc5MGI3NjExZm1hN3NxN3I3dmJ4cmsyOGQyY3Z0cjFmaW15NHBsMDVnb251YmRlayZlcD12MV9naWZzX3NlYXJjaCZjdD1n/mcsPU3SkKrYDdW3aAU/giphy.gif
background: contain
---

### Let's crush some numbers

---
transition: slide-up
zoom: 0.8
---

### Numbers goes here

```shell {all|4|7|8|6|5}
┌─────────┬───────────────┬────────────────────┬───────────────────┬────────────────────────┬────────────────────────┬─────────┐
│ (index) │ Task name     │ Latency avg (ns)   │ Latency med (ns)  │ Throughput avg (ops/s) │ Throughput med (ops/s) │ Samples │
├─────────┼───────────────┼────────────────────┼───────────────────┼────────────────────────┼────────────────────────┼─────────┤
│ 0       │ 'native'      │ '6172.1 ± 3.32%'   │ '3000.0 ± 209.00' │ '314513 ± 0.03%'       │ '333333 ± 24833'       │ 1620558 │
│ 1       │ 'axios'       │ '1213542 ± 26.47%' │ '249479 ± 66104'  │ '3948 ± 0.79%'         │ '4008 ± 1199'          │ 8244    │
│ 2       │ 'got'         │ '792135 ± 12.89%'  │ '183458 ± 30250'  │ '5055 ± 0.56%'         │ '5451 ± 1018'          │ 12691   │
│ 3       │ 'undiciAgent' │ '410468 ± 7.09%'   │ '102916 ± 10374'  │ '8032 ± 0.47%'         │ '9717 ± 1074'          │ 24433   │
│ 4       │ 'undiciFetch' │ '181892 ± 3.27%'   │ '132417 ± 5625.0' │ '7340 ± 0.12%'         │ '7552 ± 325'           │ 54995   │
└─────────┴───────────────┴────────────────────┴───────────────────┴────────────────────────┴────────────────────────┴─────────┘
```

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
