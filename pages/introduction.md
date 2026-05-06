---
transition: fade-out
---

# HTTP clients

> Pretty sure you have used some HTTP clients in your projects (I mean, everybody has).

---
transition: slide-up
level: 2
---

# Maybe `axios`?

```js
import axios from 'axios';
const response = await axios.get('https://api.sampleapis.com/coffee/hot');
console.log(response.data);
```

---
transition: slide-up
level: 2
---
# Could be `got`?

```js
import got from 'got';
const response = await got('https://api.sampleapis.com/coffee/hot');
console.log(response.json());
```

---
transition: slide-up
layout: two-cols
level: 2
---

## `node:http(s)`?

::right::
```js
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
level: 2
---

# What does all of them have in common?

---
transition: slide-left
layout: quote
level: 2
---

# Performance

<v-click>
<quote>Performance is a feature - <b>Matteo Collina</b></quote>
</v-click>

