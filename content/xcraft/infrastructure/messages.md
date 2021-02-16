---
title: 'Messages'
date: 2020-08-05
weight: 20
tags: ['devel', 'bus']
---

## Serializer

The Xcraft messages (by command like by event) know how to send
[Immutable.js][1] objects (and [Shredder][2]) as far as these objects are
provided correctly. In order to be serializable, it must be compliant with one
of the following points:

1. As value of `msg.data`
2. As value in an array provided in `msg.data`
3. As value in an object provided in `msg.data`

> where `msg` is the Xcraft envelop and `data` the user payload

**Compliant:**

```js
/* literal */
data = 1;

/* JSON object */
data = {};

/* Just one Immutable.js */
data = ImmutableJS;

/* Just one Shredder */
data = Shredder;

/* Array of Immutable.js */
data = [ImmutableJS, ImmutableJS];

/* Array of Shredder */
data = [Shredder, Shredder];

/* Mix of Immutable.js and Shredder */
data = [ImmutableJS, Shredder];

/* One JS objet which contains Immutables.js and Shredder */
data = {
  key1: ImmutableJS,
  key2: Shredder,
};
```

**Not compliant:**

```js
/* Deep Immutable.js / Shredder */
data = {
  key1: {
    subkey1: ImmutableJS,
    subkey2: Shredder,
  },
};
```

Everything that are not JSON like functions for example.

But it's not a fatality, the functions are just "lost" because not serialized
during the transport.

## Streams

> Streams are like the rivers. You need a lot of rocks? Then use your boats and
> not your horses.

The previous section shows us that we can easily transport immutables like
non-immutable data. But what about the streams that are not in these categories?
It's very important to support the streams in Xcraft in order to transfer large
data. It's not appropriate to send large payloads on the buses, the streams are
on the rescue.

The transport layer has a mechanism which is called the streamer. This one works
with the Xcraft commands and events in order to send transparently the streams
via chunks (the chunk size is around 4 KB (the default nodejs size)).

According to the way how works an Xcraft network (topology), the streaming from
a client (portal) to a server (thrall) or the reverse, must use the commands or
the events. When creating the streamer, it's necessary to specify the mode,
**download** or **upload**.

To make an upload (from a client to a server) we provide the stream in the
Xcraft command message via the key `xcraftUpload`.

```js
quest.cmd(cmd, {xcraftUpload: myStream});
```

In order to download (a send from the server to the client), the server must
provide an `xcraftStream` (this name will be changed to `xcraftDownload` in the
future).

```js
quest.evt(topic, {xcraftStream: myStream});
```

When these keys are specified, a streamer will automatically be built in order
to handle the transfers between the servers (yes, a client can be seen like a
server too, [see here][3]).

The receiver which wants to retrieve the stream, must use the received object
(via the Xcraft message), and calls the function `streamer()`. For example, when
there is a `xcraftStream` in a quest parameter (upload case), the client must
call the function `xcraftStream.streamer()` in order to start the streaming.

**Like here:**

```js
/* in the quest handler */
const stream = fs.createWriteStream(outputFile);
yield xcraftStream.streamer(appId, stream, null, next);
```

{{% notice info %}} The `appId` is necessary for that the right sub-server
receives the commands. The reason is that the transport layer exports the
commands on the buses. Because the transport layer exists in all Xcraft servers,
in order to have visible commands (no overload), the `appId` must be provided in
the name (it's like a namespace). ![image](/img/transport.png?classes=shadow)
{{% /notice %}}

### Multi-streams

TODO

[1]: https://immutable-js.github.io/immutable-js/
[2]: https://github.com/Xcraft-Inc/xcraft-core-shredder
