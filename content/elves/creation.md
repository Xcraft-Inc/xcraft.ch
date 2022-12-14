---
title: 'La création'
date: 2022-12-14
weight: 40
tags: ['devel', 'goblin', 'elf', 'create']
pre: '<b>3.5 </b>'
---

La création d'Elfes doit se faire à l'aide de la méthode `create()`. Voici des
exemples simples pour se familiariser avec l'écriture.

## De nouveaux Elfes

Imaginons des Elfes où le premier (`Valinor` qui est un singleton) veut créer
les Elfes (`Elrond` et `Galadriel`) sous la forme d'instances.

```js
class Galadriel extends Elf {
  async create(id, desktopId = null) {
    this.do();
    return this;
  }

  async hi(who) {
    this.log.dbg(`Hi ${who}`);
  }
}
```

```js
class Elrond extends Elf {
  async create(id, desktopId = null) {
    this.do();
    return this;
  }

  async hello(who) {
    this.log.dbg(`Hello ${who}`);
  }
}
```

```js
class Valinor extends Elf.Alone {
  async init() {
    const desktopId = 'valinor@system';
    try {
      const elrond = await new Elrond(this).create('elrond@valinor', desktopId);
      const galadriel = await new Galadriel(this).create(
        'galadriel@valinor',
        desktopId
      );
      await elrond.hello('Galadriel');
      await galadriel.hi('Elrond');
    } finally {
      await this.kill(elrond.id);
      await this.kill(galadriel.id);
    }
  }
}
```

Cet exemple présente plusieurs caractéristiques importantes. Ici nous voyons
deux Elfes d'instance, ainsi qu'un Elfe de type singleton. Le singleton
`Valinor` créer les deux Elfes `Elrond` et `Galadriel`, les utilise puis les
dispose. Ce mécanisme se nomme le `create`/ `kill` pattern et n'a rien de bien
sorcier. Il faut néanmoins bien comprendre que le `kill` est explicite avec un
singleton, car justement, personne ne tue un singleton et nous ne désirons pas
avoir une fuite d'Elfes.

## Et si un Elfe d'instance créait un autre Elfe

Quand une instance créer un Elfe, il n'est pas nécessaire d'appliquer le
`create` / `kill` pattern car dans ce cas, le fait de tuer le premier Elfe va
automatiquement tuer les Elfes sous-jacents (bien entendu, pour autant qu'aucun
autre Elfe en dépende également).

```js
class Galadriel extends Elf {
  async create(id, desktopId = null) {
    this.do();
    return this;
  }

  async hi(who) {
    this.log.dbg(`Hi ${who}`);
  }
}
```

```js
class Elrond extends Elf {
  async create(id, desktopId = null) {
    this.do();
    return this;
  }

  async hello(who) {
    this.log.dbg(`Hello ${who}`);
  }

  async awake() {
    const galadriel = await new Galadriel(this).create('galadriel@valinor');
    await galadriel.hi('Elrond');
    await this.hello('Galadriel');
  }
}
```

```js
class Valinor extends Elf.Alone {
  async init() {
    const desktopId = 'valinor@system';
    try {
      const elrond = await new Elrond(this).create('elrond@valinor', desktopId);
      await elrond.awake();
    } finally {
      await this.kill(elrond.id);
    }
  }
}
```

Dans cet exemple, `Elrond` va créer `Galadriel` puis communiquer avec cette
Elfe. Mais cet exemple introduit également une autre particularité. Quand un
Elfe souhaite exécuter ses propres quêtes, il peut simplement les appeler via
`this` comme dans cet exemple avec `await this.hello()`.

> Attention, n'oubliez pas ce qui est expliqué dans le chapitre **Qui
> sont'ils**. Les classes elfiques ne sont pas des classes javascript standards,
> et les appels sur les méthodes ne sont pas directs mais passent par les bus
> Xcraft.
