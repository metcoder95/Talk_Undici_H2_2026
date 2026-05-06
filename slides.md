---
# try also 'default' to start simple
theme: eloc
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: https://unsplash.com/photos/bicycles-parked-on-a-bridge-over-a-canal-in-amsterdam-f7xGAjyXehU
# some information about your slides (markdown enabled)
title: "Under the Hood of Undici: Implementing HTTP/2 for the Modern Web"
info: |
  # Under the Hood of Undici: Implementing HTTP/2 for the Modern Web
  For years, Node.js developers relied on the legacy node:http module, a tool designed for a simpler era of the web. As performance requirements grew, its limitations—clunky event-emitter patterns, high memory overhead, and lack of native support for modern protocols—became apparent. Enter Undici: a high-performance HTTP/1.1 and HTTP/2 client written from scratch for Node.js.
# apply UnoCSS classes to the current slide
class: text-left
# https://sli.dev/features/drawing
drawings:
  persist: false
# slide transition: https://sli.dev/guide/animations.html#slide-transitions
transition: slide-left
# enable Comark Syntax: https://comark.dev/syntax/markdown
comark: true
# duration of the presentation
duration: 30min
---

# Under the Hood of Undici

Implementing HTTP/2 for the Modern Web

<div class="abs-br m-6 text-xl">
<!-- TODO: add link to github repo of the slides -->
  <a href="https://github.com/slidevjs/slidev" target="_blank" class="slidev-icon-btn">
    <carbon:logo-github />
  </a>
</div>


---
src: ./pages/introduction.md
hide: false
---

---
src: ./pages/myself.md
hide: false
---

---
src: ./pages/legacy_http_module.md
hide: false
---

---
src: ./pages/undici_primer.md
hide: false
---

---
src: ./pages/http2.md
hide: false
---

---
src: ./pages/closure.md
hide: false
---

