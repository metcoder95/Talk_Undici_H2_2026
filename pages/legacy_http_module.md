---
layout: image-right
transition: slide-up
image: https://cdn.hashnode.com/res/hashnode/image/upload/v1713781273707/f04b7285-b8e9-4081-b2ee-74e78379ac32.png
backgroundSize: contain
---

### Let's take a look at some numbers...

<v-click>
<quote> This is just for HTTP/1.1. </quote>
</v-click>
<br>
<v-click>
<quote class="text-sm"> Took from Platformatic 2025 </quote>
</v-click>

---
transition: slide-up
layout: statement
---

## They all suffer from the same performance issues.

<v-click>
Then, why's that?
</v-click>
<br>
<v-click>
What does it make them like that, what are the limitations?
</v-click>

---
transition: slide-up
layout: fact
---

# They are three key factors

<v-click>
<h3> 1. The Ergonomic Tradeoff </h3>
</v-click>

---
transition: slide-up
layout: two-cols-header
---

## The Ergonomic Tradeoff

::left::
<v-click>
<div class="text-md">
The node:http manage all the orchestration by a pool of sockets and a set of event listeners.
</div>
</v-click>
<br>
<v-click>
<div class="text-md">
Every request involves the creation of several objects and managing listeners, leading to CPU Overhead and Garbage Collection Pressure.
</div>
</v-click>

::right::
```js {10-17}
import { request } from 'node:https';

const options = {
  hostname: 'encrypted.google.com',
  port: 443,
  path: '/',
  method: 'GET',
};

const req = request(options, (res) => {
  console.log('statusCode:', res.statusCode);
  console.log('headers:', res.headers);

  res.on('data', (d) => {
    console.log(d.toString());
  });
});
```

---
transition: slide-up
layout: fact
---

# 2. Throughput Limitations

---
transition: slide-up
layout: two-cols-header
zoom: 1.2
---

### !Pipelining === ~Throughput
::left::
<img src="https://miro.medium.com/v2/resize:fit:706/format:webp/1*1KnIRWFYwLFUoucjI0z9bw.png" class="mx-auto" />

::right::
<v-click>
<div class="text-md">
Pipelining is a feature of HTTP/1.1 that allows multiple HTTP requests to be sent over a single TCP connection without waiting for the corresponding responses
</div>
</v-click>
<br>
<v-click>
<div class="text-sm">
Source: https://medium.com/@0xbughunter/http-pipelining-multiplexing-82a6d173b390
</div>
</v-click>

---
transition: slide-up
layout: fact
---

# 3. Two Worlds Colliding

---
transition: slide-up
backgroundSize: statemement
---

## Jumping between worlds is not cheap

<v-click>
<div class="text-md">
The <b>node:http</b> uses <b>llhttp</b> (written in C) under the hood as HTTP Parser, interacting with JavaScript through the native layer of Node.js.
</div>
</v-click>
<br>
<v-click>
<div class="text-md">
The boundary crossing for data exchange between them incurs in a good chunk of overhead leading to a ripple effect on overall performance.
</div>
</v-click>

---
transition: slide-up
layout: statement
---

# So, let's fix them, isn't it?

<v-click>
<div class="text-md">
Well, is not like it hasn't been tried before...
</div>
</v-click>
<br>
<v-click>
<div class="text-md">
doing it requires a lot of effort, and the ecosystem is already built around it, so it's not an easy task.
</div>
</v-click>
<br>
<v-click>
<div class="text-sm">
Remember the <i>SmooshGate</i> with <b>MooTools</b> back in 2018?
</div>
</v-click>

---
transition: slide-left
layout: statement
---

# Then, what's the solution?

<v-click>
<div class="text-md">
Let's build it from scratch
</div>
</v-click>