---
title: 'Client'
date: 2020-08-05
weight: 5
draft: true
---

There is two levels of accesses to the buses of an Xcraft server. There is the
low level (for Orcs), and it's the Xcraft API. For the Goblins, you will use an
higher level of API based on the Quest context.

> Xcraft level? Goblins level? But what the hell?  
> -- Human without [Mana][1]

The Goblins live in a land governed by the Orcs... Okay, I've lost you, sorry..
The Xcraft's name is a nod to the WarCraft game, but it's not just that. X is
for "cross" because it's cross-platform. And "craft" is for "crafting".

Now that you understand the origin, the low level layer is the first layer which
has been built. The servers, the communications, the first services are based on
the Xcraft layer. After some years, a new layer has appeared; the Goblins layer.
In this layer, you write reducers and quests (redux) and you play mostly with
immutable states.

This is why there are two differents API. But note that the Goblins API uses the
Xcraft API.

## Xcraft API

...

## Goblin API

```js
quest.cmd(); /* send a command (use bus client in push mode) */
quest.evt(); /* send an event (use local server in pub mode) */
quest.sub(); /* subscribe to an event (use bus client in sub mode) */
quest.sub.wait(); /* -> like sub but with barrier feature */
quest.sub.callAndWait(); /* -> like sub but subscribe and call before waiting on the event */
```

[1]: https://en.wikipedia.org/wiki/Magic_(game_terminology)
