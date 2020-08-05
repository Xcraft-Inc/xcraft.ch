---
title: 'Backends'
date: 2020-08-05
weight: 50
draft: true
---

There are two backends in the transport layer. The first one is named **EE** and
it's based on the `EventEmitter` of nodejs. The second one is named **Axon** and
implements a particular protocol on TCP/IP. The base idea is simple. A message
where the destination is the same process, uses the **EE** backend. But a
message where the destinations are on external processes, will use the **Axon**
backend. Even the base idea is simple, the implementation is much complexe. The
events are routed accordingly to the destination and a lot of events are just
used in broadcasting. These events are sent on all backends (**EE** and
**Axon**).

> It's important to understand that the **objects passed via EE are not cloned
> and then, the functions are not stripped**. It's forbidden to mute an object
> from a message and to depends of these mutations from the caller. Providing
> functions as value in the Xcraft messages is forbidden too. There is no
> cloning of cleaning only for performances reason.

The use of the transport is done vis the routers. These routers use the backends
accordingly to the routing table and the topics.
