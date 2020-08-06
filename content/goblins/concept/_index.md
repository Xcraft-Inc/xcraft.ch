---
title: 'Concept'
date: 2020-08-05
weight: 10
chapter: true
pre: '<b>2.1 </b>'
---

### Chapter 2.1

# Concept

The goblin idea match perfectly with the actor model of Carl Hewitt designed
in 1973. We don't know this model before designing this framework, after some
search on wikipedia we discover that our work match the following definition
perfectly.

> The actor model in computer science is a mathematical model of concurrent
> computation that treats actor as the universal primitive of concurrent
> computation. In response to a message it receives, an actor can: make local
> decisions, create more actors, send more messages, and determine how to
> respond to the next message received. Actors may modify their own private
> state, but can only affect each other indirectly through messaging (removing
> the need for lock-based synchronization). -- Wikipedia [source][1]

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
