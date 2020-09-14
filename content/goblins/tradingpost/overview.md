---
title: 'Overview and usage'
date: 2020-07-04
weight: 10
---

The goblin tradingpost allow you to serve goblin quests on an http API. You can
add multiple goblin on the trading post with the quest below :

```js
yield tradingAPI.addGoblinApi({
  goblinId: 'super-goblin',
  allowedCommands: ['toggleGodMode'],
  secure: null, // Not implement for the moment
});
```

When you are ready to serve your goblin(s) quests on the server, just start the
server :

```js
const address = yield tradingAPI.start();
```

{{% notice info %}}There is a swagger server in dev environnement to see and
test all quests exposed on the fastify server. Add just `/docs` to the base url
returned by the quest **`start`**{{% /notice %}}

You can remove quests exposed from a goblin by passing the goblinId :

```js
yield tradingAPI.removeGoblinApi({
  goblinId: 'super-goblin',
});
```

Or remove all goblin quests :

```js
yield tradingAPI.removeAllGoblinApi();
```

If you remove goblin quests you have to restart server for the changes to take
effect :

```js
yield tradingAPI.restart();
```

When you are done with the tradingpost, you can call the close quest :

```js
yield tradingAPI.close();
```
