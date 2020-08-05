---
title: 'Quest'
date: 2020-08-05
weight: 5
draft: true
---

## Xcraft API

...

## Goblin API (high level)

```js
quest.cmd(); /* send a command (use bus client in push mode) */
quest.evt(); /* send an event (use local server in pub mode) */
quest.sub(); /* subscribe to an event (use bus client in sub mode) */
quest.sub.wait(); /* -> like sub but with barrier feature */
quest.sub.callAndWait(); /* -> like sub but subscribe and call before waiting on the event */
```
