---
title: 'Backends'
weight: 50
tags: ['devel', 'bus']
---

> Use horses when traveling in the lands, but prefer more faster and appropriate
> transports when talking to your neighbours

There are two backends in the transport layer. The first one is named **EE** and
it's based on the `EventEmitter` of nodejs. The second one is named **Axon** and
implements a particular protocol on TCP/IP. The base idea is simple. A message
where the destination is in the same process, uses the **EE** backend. But a
message where the destinations are on external processes, will use the **Axon**
backend. Even if base idea is simple, the implementation is much more complexe.
The events are routed accordingly to the destinations and a lot of events are
just used in broadcasting. These events are sent on all backends (**EE** and
**Axon**).

{{% notice warning %}} It's important to understand that the **objects passed
via EE are not cloned and then, the functions are not stripped**. It's forbidden
to mute an object from a message and to depend of these mutations from the
caller. Providing functions as value in the Xcraft messages is forbidden. There
is no cloning or cleaning only for performance reasons. {{% /notice %}}

The use of the transport is done via the routers. These routers use the backends
accordingly to the routing table and the topics.
