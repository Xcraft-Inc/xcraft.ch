---
title: 'La naissance'
date: 2022-12-09
weight: 10
tags: ['devel', 'goblin', 'elf']
pre: '<b>3.2 </b>'
---

La naissance d'un Elfe peut mener à deux choix distincts. Soit votre Elfe vivra
seul pour toujours, soit il se multipliera pour prospérer et dominer le m....
Soyons sérieux, ici il est question de singletons et des autres.

## Un Elfe seul

Un service de ce type est très simple à créer car il suffit de dériver sa class
du type `Elf.Alone`. Vous pouvez alors y adjoindre les quêtes que vous souhaitez
de la même manière que pour un service instanciable plusieurs fois. Dans le cas
d'un singleton, vous ne devez pas avoir de quête `create` ni de quête `delete`.
Par contre il est d'usage d'y adjoindre une quête `init` faisant office de
constructeur.

```js
const {Elf} = require('xcraft-core-goblin');

class Valinor extends Elf.Alone {
  async init() {
    /* await quelque chose ... */
  }
}

module.exports = Valinor;
```

## Une horde d'Elfes

Les service d'instances sont un petit peu différents car il doivent dériver du
type `Elf`. Les quêtes fonctionnent de la même manière que pour les Elfes seuls.
Néanmoins deux quêtes remplacent la quête `init` présenté avec les singletons.
La quête `create` doit être utilisé comme constructeur pour le service, et la
quête `delete` comme "destructeur" ou dit autrement, comme quête de "dispose".

```js
const {Elf} = require('xcraft-core-goblin');

class Elrond extends Elf {
  async create(id, desktopId = null) {
    /* await quelque chose ... */
  }

  delete() {
    /* quelque chose ... */
  }
}

module.exports = Elrond;
```

Il est fortement recommandé de toujours utiliser des quêtes de `delete` 100%
synchrones. Des effets de bords indésirables peuvent survenir si vous effectuez
du code asynchrone dans une quête de type `delete`. En effet, le scheduler
Goblin ne s'attend pas à devoir gérer de l'asynchrone dans un `delete` quand un
Goblin de même ID doit être instancié au même moment.

> Ici on parle de Goblin, car ce comportement est généralisé.

## Mener votre Elfe aux Terres Immortelles

N'oubliez pas de les y emmener en plaçant le fichier de service Xcraft à la
racine d'un des arbre de Valinor. Uhm, pardon, j'veux dire à la racine de votre
module Goblin bien entendu. Il n'y a aucune différence entre l'exportation
Xcraft pour un Elfe de type `Elf.Alone` et un Elfe de type `Elf`.

```js
const {Elf} = require('xcraft-core-goblin');
const Elrond = require('./lib/elrond.js');

exports.xcraftCommands = Elf.birth(Elrond);
```
