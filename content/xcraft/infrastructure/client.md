---
title: 'Client'
weight: 10
tags: ['devel', 'bus']
---

There are two levels of accesses to the buses of an Xcraft server. There is the
low level (for Orcs), and it's the Xcraft API. For the Goblins, you will use an
higher level of API based on the Quest context.

> Xcraft level? Orcs? Goblins level? But what the hell?  
> -- Human without [Mana][1]

The Goblins live in a land governed by the Orcs... Okay, I've lost you, sorry..
The Xcraft's name is a nod to the WarCraft game, but it's not just that. **X**
is for "cross" because it's cross-platform. And **craft** is for "crafting".

Now that you understand the origin, the low level layer is the first layer which
has been built. The servers, the communications, the first services are based on
the Xcraft layer. After some years, a new layer has appeared; the Goblin layer.
In this layer, you write reducers and quests (redux) and you play mostly with
immutable states.

This is why there are two differents API. But note that the Goblin API uses the
Xcraft API.

## Xcraft API

When using this API, you must connect to a server with a busClient. The server
will assign to your client an identity (an orcName). This is with this identity
that you can send commands and receive dedicated events. When connecting to a
server, two buses are used. One bus is dedicated to the commands, and a second
bus is dedicated to the events. It means that you need two sockets when you are
communicating between processes.<a name="pure"></a>

{{% notice info %}} With just a busClient, you can't send events. Only a server
can send events and it's a question of patterns. There are two patterns used
here: **push** / **pull** (for the command bus) and **pub** / **sub** (for the
event bus). The server implements the **pull** in order to receive the commands
that are **push**ed by the clients, and **pub**lishs events for the clients. The
client implements the **push** of commands, and **sub**scribes to events.
{{% /notice %}}

The server works as **pull** / **pub** and the client as **push** / **sub**.

### Connect

![infrastructure.connect](/img/infrastructure.connect.png?width=300px)

You can connect to your own server (in-process) by this way:

```js
watt(function* () {
  const {BusClient} = require('xcraft-core-busclient');
  const busClient = new BusClient();
  yield busClient.connect('ee', null, next);
})();
```

It's the most simpler connection. In this case, we are connecting to the server
and we are processing only the events which are dedicated to our client.

The constructor can take some arguments. The first argument is the bus settings.
You can specify these settings when it's necessary like in the case of servers
in an other process. When passing `null`, then it uses the current server
settings. The second argument is an array of event subscriptions. By passing
`*::*` we will receive even the events of other clients.<a name="axon"></a>

```js
watt(function* () {
  const {BusClient} = require('xcraft-core-busclient');
  const busClient = new BusClient(
    {
      host: '127.0.0.1',
      commanderPort: 10000,
      notifierPort: 20000,
    },
    '*::*'
  );
  yield busClient.connect('axon', null, next);
})();
```

As explained in the [backends](/xcraft/infrastructure/backends) section, there
are two backends: **EventEmitter** and **Axon**. It makes sense to always use
the `ee` backend when we are connecting to the server of our own process. But of
course, in the case of an other process, `axon` is mandatory (see the previous
[code sample](#axon)).

The `connect(backend, token)` method has the backend for first argument and the
server token for the second argument. You should **never** use the second
argument. It's especially used by Xcraft in order to open a connection while the
server is starting and the autoconnect stuff is still not available. Just set to
`null`, then something like an autoconnect will be engaged. After this call, you
are connected and you can send commands, events (if you are not a pure client;
like explaind [here](#pure)) and receive data.

### Send commands

![infrastructure.command](/img/infrastructure.command.png?width=200px)

Once connected, you can send a command to a service in order to do something.

```js
watt(function* () {
  const {BusClient} = require('xcraft-core-busclient');
  const busClient = new BusClient();
  yield busClient.connect('ee', null, next);
  const msg = yield busClient.command.send(
    'peon.work',
    {ressources: ['or', 'wood', 'rock']} /* payload */,
    null,
    next
  );
})();
```

The first argument is the full command name. Most of time, a command begins with
a namespace which is the service's name. In this example, it's the service
`"peon"` and we want to run the `"work"` command. The second argument is the
payload sent with this command. In our example, the `work()` handler will
receive an object with an array of `ressources`. The third agrument is the
`orcName`. Please, do not use this argument (always pass `null`). It's used for
very special cases where it's necessary to use a `busClient` for a different
`orcName` (something like routing).

After the `next` callback, it's possible to pass options. Only one option is
really used, it's `{forceNested: true}` and it's only used internally. I will
explain in details in [an other section // TODO](TODO).

### Send events

![infrastructure.events](/img/infrastructure.events.png?width=256px)

You can send events when your `busClient` is provided by an Xcraft server. It's
always the case when your are implementing a command handler. From this handler
you can send commands too.

To send events, it's a bit like the commands with some differences:

```js
peon.work = function (msg) {
  const busClient = require('xcraft-core-busclient').getGlobal();
  const {ressources} = msg.data;
  try {
    const res = [];
    /* Do a lot of work and populate res */
    busClient.events.send(`${msg.orcName}::peon.work.${msg.id}.finished`, res);
  } catch (ex) {
    busClient.events.send(`${msg.orcName}::peon.work.${msg.id}.error`, ex);
  }
};
```

In this example, you can see the "common" implementation of an Xcraft command
handler. When the stuff is done without error, you must send the `.finished`
event. But if an error occures, then you must send an `.error` event with the
reason (mostly the exception object).

When you send an event, you must pass the namespace in the topic when you use
`busClient.events.send`. The namespace is mostly the client `orcName` which has
make the call on this command handler.

An handler receives a `resp` object as second argument. With this `resp` object,
a small wrapper adds the appropriate namespace for you when you send an event.

For example (same behaviour but simpler):

```js
peon.work = function (msg, resp) {
  const {ressources} = msg.data;
  try {
    const workList = [];
    /* Do a lot of work and populate workList */
    resp.events.send(`peon.work.${msg.id}.finished`, workList);
  } catch (ex) {
    resp.events.send(`peon.work.${msg.id}.error`, ex);
  }
};
```

It's still not a Goblin thing. The `resp` object is just a small wrapper over
`busClient`. In the [Goblin API](#goblin-api) section, you will learn a more
high level API.

### Subscribe to events

![infrastructure.subscription](/img/infrastructure.subscription.png?height=300px)

Maybe you want to receive events and in this case, you must subscribe to a
topic. Don't forget to unsubscribe when you don't want to receive this sort of
event anymore (the system can not guess that you don't want this event, then it
**leaks** without call to the unsubscribe function).

```js
watt(function* () {
  const {BusClient} = require('xcraft-core-busclient');
  const busClient = new BusClient();
  yield busClient.connect('ee', null, next);

  let unsubscribe = null;
  try {
    /* subscribe to an event */
    unsubscribe = busClient.events.subscribe(
      `${msg.orcName}::peon.work.done`,
      (msg) => {
        /* can be called multiple times */
      }
    );

    const msg = yield busClient.command.send(
      'peon.work',
      {ressources: ['or', 'wood', 'rock']} /* payload */,
      null,
      next
    );
  } finally {
    if (unsubscribe) {
      unsubscribe();
    }
  }
})();
```

You can find more informations about the events at this
[section](/xcraft/infrastructure/events).

## Goblin API

With the Goblin API it's simpler because more stuff is hidden in the Goblin
layer. It's no longer necessary to connect to a server because you are already
connected. It's no longer necessary to subscribe explicitly to the dedicated
command events `finished` and `error` because it's already handled by the Goblin
layer. The magic is in the `quest` context provided by the Goblin layer when a
quest is executed.

### Send commands

It's really simple to send a command to a service since the `quest` context
wraps `busClient` in an high level API.

```js
Goblin.registerQuest(goblinName, 'engage', function* (quest) {
  /* send a command (use bus client in push mode) */
  const result = yield quest.cmd(
    'peon.work',
    {ressources: ['or', 'wood', 'rock']} /* payload */
  );
});
```

In this example, we send a command to a singleton (because no `id` is
specified). Then there is an other way which is more object oriented and can
improve the readability.

```js
Goblin.registerQuest(goblinName, 'engage', function* (quest) {
  const peon = quest.getAPI('peon');
  const result = yield peon.work(
    {ressources: ['or', 'wood', 'rock']} /* payload */
  );
});
```

If possible, prefer the `getAPI` way because it's more high level. You should
use `quest.cmd` only when you want to create dynamic calls (where the service
and/or the command names are not already known).

### Send events

When you send an event, your `goblinName` (namespace) is automatically added to
the event. Of course, you can send an event without payload. For more
informations about the command namespaces, please look
[here](/xcraft/infrastructure/commands).

```js
Goblin.registerQuest(goblinName, 'engage', function* (quest) {
  const myWork = yield* iAmWorkingVeryHard();
  /* send an event (use local server in pub mode) */
  quest.evt('work.done', myWork.status);
  return myWork;
});
```

Here we are in a quest (it's called by a command handler generated by the Goblin
layer). Do not try to send a `.finished` or `.error` event because it's handled
by the Goblin layer. If you have an error, just `throw`. And for the result,
just `return`.

{{% notice info %}}It's forbidden (in the case of the Goblin API) to send events
with the `.finished` or the `.error` suffixes because it's reserved to the
commands. These events are always using the
[unicast routing](/xcraft/infrastructure/routing).{{% /notice %}}

### Subscribe to events

```js
quest.sub(); /* subscribe to an event (use bus client in sub mode) */
quest.sub.wait(); /* -> like sub but with barrier feature */
quest.sub.callAndWait(); /* -> like sub but subscribe and call before waiting on the event */
```

...

### Examples

...

[1]: https://en.wikipedia.org/wiki/Magic_(game_terminology)
