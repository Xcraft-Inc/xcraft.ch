---
title: 'Async and Watt'
weight: 15
tags: ['async', 'await', 'watt']
---

## Utilisation de l'asynchrone dans les quêtes

Une quête `watt` peut faire du `yield` sur un `async` et inversément, une quête
`async` peut faire un `await` sur un `watt`.

Si vous vous demandez pourquoi il est possible de mixer les deux, la réponse est
simple. les générateurs `watt` retournent des `Promise`. Une fonction `async`
javascript n'est "presque" rien de plus qu'une `Promise`.

Maintenant on peut se demander :

> Mais pourquoi conserver les générateurs `watt` ?

La question est légitime et le fait de les conserver aussi. Les générateurs
`watt` on des avantages sur les fonctions `async`. Un avantage vient du fait
qu'avec `watt` il est possible de transformer un appel de fonction à callback en
générateur (et donc en `Promise` `watt`) et sans rien faire d'autre que de
passer le `next` de `watt` en tant que callback.

Un autre avantage : avec `watt` il est très facile de faire des attentes sur
plusieurs appels asynchrones via le `next.parallel()`. Par contre avec des
fonctions `async`, cela demande de gérer des tableaux de `Promise` puis de faire
un `await` sur un `Promise.all` par exemple, ce qui rajoute plus de code. Code
que l'on pourrait aussi abstraire ceci dit.

### Conclusion

Vous pouvez alors choisir la méthode qui convient le mieux selon les situations
étant donné qu'il est possible de mélanger les deux façons de faire.

## Exemple d'implémentations en `async` / `await`

```js
Goblin.registerQuest(goblinName, 'fire', (quest) => {
  quest.log.dbg('>>> fire');
  quest.defer(() => quest.log.dbg('<<< fire'));

  quest.evt('fired');
});

Goblin.registerQuest(goblinName, 'onFired', (quest) => {
  quest.log.dbg('>>> onFired');
  quest.defer(() => quest.log.dbg('<<< onFired'));

  quest.log.dbg('>>>> fired');
});

Goblin.registerQuest(goblinName, 'callOnDefer', async (quest) => {
  quest.log.dbg('>> callOnDefer');
  quest.defer(() => quest.log.dbg('<< callOnDefer'));

  /* async/await quest */
  await quest.me.fire();
});

Goblin.registerQuest(goblinName, 'asyncQuest', async (quest) => {
  quest.log.dbg('> asyncQuest');
  quest.defer(() => quest.log.dbg('< asyncQuest'));

  /* async/await sub */
  quest.defer(
    quest.sub(`${goblinName}.fired`, async () => await quest.me.onFired())
  );

  /* async/await defer */
  quest.defer(async () => await quest.me.callOnDefer());

  /* async/await sub callAndWait */
  await quest.sub.callAndWait(
    async () => await quest.me.fire(),
    `${goblinName}.fired`
  );
});
```
