---
title: 'Client'
date: 2020-08-05
weight: 5
draft: true
---

There is two levels of accesses to the buses of an Xcraft server. There is the
low level (for Orcs), and it's the Xcraft API. For the Goblins, you will use an
higher level of API based on the Quest context.

> Xcraft level? Orcs? Goblins level? But what the hell?  
> -- Human without [Mana][1]

The Goblins live in a land governed by the Orcs... Okay, I've lost you, sorry..
The Xcraft's name is a nod to the WarCraft game, but it's not just that. **X**
is for "cross" because it's cross-platform. And **craft** is for "crafting".

Now that you understand the origin, the low level layer is the first layer which
has been built. The servers, the communications, the first services are based on
the Xcraft layer. After some years, a new layer has appeared; the Goblins layer.
In this layer, you write reducers and quests (redux) and you play mostly with
immutable states.

This is why there are two differents API. But note that the Goblins API uses the
Xcraft API.

## Xcraft API

When using this API, you must connect to a server with a busClient. The server
will assign to your client an identity (an orcName). This is with this identity
that you can send commands and receive dedicated events. When connecting to a
server, two buses are used. One bus is dedicated to the commands, and a second
bus is dedicated to the events. It means that you need two sockets when you are
communicating between processes.

{{% notice info %}} With just a busClient, you can't send events. Only a server
can send events and it's a question of patterns. There are two patterns used
here: **push** / **pull** (for the command bus) and **pub** / **sub** (for the
event bus). The server implements the **pull** in order to receive the commands
that are **push**ed by the clients, and **pub**lishs events for the clients. The
client implements the **push** of commands, and **sub**scribes to events.
{{% /notice %}}

The server works as **pull** / **pub** and the client as **push** / **sub**.

### Connect

...

### Send commands

...

### Send events

...

### Subscribe to events

...

### Examples

...

## Goblin API

With the Goblin API it's simpler because more stuff is hidden in the goblins
layer. It's no longer necessary to connect to a server because you are already
connected. It's no longer necessary to subscribe explicitly to the dedicated
command events `finished` and `error` because it's already handled by the
goblins layer. The magic is in the `quest` context provided by the goblins layer
when a quest is executed.

### Send commands

```js
quest.cmd(); /* send a command (use bus client in push mode) */
```

...

### Send events

```js
quest.evt(); /* send an event (use local server in pub mode) */
```

...

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
