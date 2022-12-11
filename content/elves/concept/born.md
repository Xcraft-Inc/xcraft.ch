---
title: 'La naissance'
date: 2022-12-09
weight: 10
tags: ['devel', 'goblin', 'elf']
---

La naissance d'un Elfe peut mener à deux choix distincts. Soit votre Elfe vivra seul pour toujours, soit il se multipliera pour prospérer et dominer le m.... Soyons sérieux, ici il est question de singletons et des autres.

## Un Elfe seul

Un service de ce type est très simple à créer car il suffit de dériver sa class du type `Elf.Alone`. Vous pouvez alors y adjoindre les quêtes que vous souhaitez de la même manière que pour un service instanciable plusieurs fois. Dans le cas d'un singleton, vous ne devez pas avoir de quête `create` ni de quête `delete`. Par contre il est d'usage d'y adjoindre une quête `init` faisant office de constructeur.

```js
const {Elf} = require('xcraft-core-goblin');

class Universe extends Elf.Alone {
  async init() {
    /* await something ... */
  }
}

module.exports = Universe;
```
