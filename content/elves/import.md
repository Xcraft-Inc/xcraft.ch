---
title: 'Voyager'
date: 2022-12-14
weight: 50
tags: ['devel', 'goblin', 'elf', 'require']
pre: '<b>3.6 </b>'
---

Tous les Elfes doivent pouvoir voyager afin de communiquer entre eux. Connaître
les autres Elfes demande de retrouver leurs quêtes et ce chapitre va vous
permettre de comprendre comment mettre tout cela correctement en place.

## Le partage du savoir

Implémenter un Elfe demande d'implémenter sa classe et de l'exporter. Chaque
classe devrait utiliser son propre fichier source. En effet, il devrait toujours
être possible d'importer un seul Elfe. Imaginons l'exemple suivant; un module
Goblin contient les Elfes Galadriel et Elrond. Chacun existe dans son propre
fichier source.

> `goblin-valinor/lib/galadriel/service.js`

```js
const {Elf} = require('xcraft-core-goblin');
const GaladrielState = require('./logic.js');

class Galadriel extends Elf {
  state = new GaladrielState();

  async create(id, desktopId = null) {
    this.do();
    return this;
  }
}

module.exports = Galadriel;
```

> `goblin-valinor/lib/elrond/service.js`

```js
const {Elf} = require('xcraft-core-goblin');
const ElrondState = require('./logic.js');

class Elrond extends Elf {
  state = new ElrondState();

  async create(id, desktopId = null) {
    this.do();
    return this;
  }

  async helloFrom(whoId) {
    this.log.dbg(`${whoId} says hello to ${this.id}`);
  }
}

module.exports = Elrond;
```

## Quand Galadriel rencontre Elrond

Maintenant imaginons que Galadriel souhaite communiquer avec Elrond. On peut
créer une quête qui prendrait par exemple l'ID d'Elrond, puis Galadriel
appellerait Elrond pour lui donner son propre ID.

```js
const {Elf} = require('xcraft-core-goblin');
const GaladrielState = require('./logic.js');
const Elrond = require('../elrond/service.js');

class Galadriel extends Elf {
  state = new GaladrielState();

  async create(id, desktopId = null) {
    this.do();
    return this;
  }

  async speakTo(elrondId) {
    const elrond = new Elrond(this).api(elrondId);
    await elrond.helloFrom(this.id);
  }
}

module.exports = Galadriel;
```

Cette communication nous permet de mettre en avant la fonction `api()` qui
n'avait pas encore été présentée. Etant donné que vous connaissez bien les
Goblins, vous connaissez alors la fonction `getAPI()` du contexte `quest`. Ici
c'est exactement la même chose. Au lieu de créer l'Elfe (ou de s'attacher à
celui-ci), on essaie de directement l'appeler tout en acceptant de recevoir une
exception s'il n'existe pas.

Le deuxième point important dans cet exemple, est la manière de récupérer
l'interface d'Elrond. C'est très simple, il n'y a qu'à importer sa classe et le
tour est joué. Cela signifie que le module Goblin qui contient l'Elfe doit
exporter proprement les classes des Elfes que l'on considère comme étant
publiques. Idéalement, faire un `require('goblin-valinor')` devrait permettre de
retrouver Elrond et Galadriel sans difficulté.
