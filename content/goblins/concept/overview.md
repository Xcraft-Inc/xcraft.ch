---
title: 'Overview'
date: 2020-08-14
weight: 1
---

Goblins use xcraft messages to send command's and event's to others, example:

```js
//somewhere in the codebase of a goblin...
function*(quest){
  yield coolGoblin.doSomething({content: 'Hello Goblins!'});
}
```

And in response, the cool goblin can say something with a hypotetic vocal
synthetizer goblin:

```js
//somewhere in the codebase of a the coolGoblin...
//In response to a message it receives...
doSomething = function* (quest, content) {
  //create more actors
  const myCoolSynthAPI = quest.create('synth-goblin', {
    id: 'synth-goblin@cool-goblin',
  });
  //use API for doing side-effects
  yield myCoolSynthAPI.play({content});

  //Use loaded goblins for requesting results
  const coolFactorAPI = quest.getAPI('cool-factor');
  const result = yield coolFactorAPI.analyse({content});

  //actors may modify their own private
  quest.do({result});
  const newState = quest.goblin.getState();

  //make local decisions
  if (newState.get('isCoolContent')) {
    //send more messages
    quest.evt('cool-content.received');
  }
};
```

[1]: https://en.wikipedia.org/wiki/Actor_model
