# "Under the Hood of Undici: Implementing HTTP/2 for the Modern Web"

For years, Node.js developers relied on the legacy node:http module, as performance requirements grew, its limitations like clunky event-emitter patterns, and high memory overhead became apparent. Enter Undici: a high-performance HTTP/1.1 (and now HTTP/2) client written from scratch for Node.js.

In this session, we’ll peel back the layers of Undici’s architecture to understand how it achieves up to more throughput than traditional clients. We’ll explore:

The Internals: How Undici replaces the overhead of http.Agent with a more efficient connection pool and dispatcher model.
Powering Native fetch: A look at how Undici serves as the engine behind the global fetch API in Node.js, bringing browser-standard networking to the server with zero external dependencies.
The HTTP/2 Leap: The technical journey of implementing HTTP/2 support, including the transition from simple request-response cycles to binary framing, multiplexing, and concurrent streams.
Real-World Benefits: Why shifting to an HTTP/2-capable client matters for modern microservices.


Whether you're a performance enthusiast or just curious about the future of the Node.js internals, join us to see how we're rebuilding the network stack for the modern web.