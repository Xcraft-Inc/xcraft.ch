---
title: 'Languages'
date: 2022-12-14
weight: 60
tags: ['devel', 'goblin', 'elf', 'jsdoc']
pre: '<b>3.7 </b>'
---

Quand on est un Elfe, on doit soigner son langage.

## Etre un guerrier ne signifie pas être grossier

Les Orcs et les Goblins peuvent parfois paraître assez grossier. Les Elfes se
doivent de ne pas se laisser aller, et s'exprimer correctement, nécessite de la
rigueur.

Est-ce que vous imagineriez Galadriel s'exprimer comme un Goblin ? Vous vous
dîtes peut-être que parce que c'est une guerrière, il ne faut pas s'attendre à
cette rigueur dans le langage. Si c'est le cas, vous avez tord, alors apprennez
à exploiter le [JSDoc][1] pour élever la qualité des échanges entre les Elfes.

```js
/**
 * Galadriel est certainement une des plus grande guerrière Elfe que
 * la Terre du Milieu est connu.
 */
class Galadriel extends Elf {
  /**
   * Créer une nouvelle Galadriel prête à se battre.
   *
   * @param {SmartId} id - ID de la nouvelle Galadriel
   * @param {SmartId} [desktopId] - ID du feed
   * @returns {Galadriel} la nouvelle Galadriel
   */
  async create(id, desktopId = null) {
    this.do();
    return this;
  }

  /**
   * Engage un combat contre un opposant.
   *
   * @param {SmartId} opposantId - ID de l'opposant à combattre
   * @param {Weapon} weapon - Arme utilisée pour le combat
   * @returns {Health} l'état de santé de Galadriel
   */
  async fight(opposantId, weapon) {
    /* ... */
    return health;
  }
}
```

Les possibilités sont très larges et la documentation officielle de [JSDoc][1]
pourra beaucoup vous aider. Dans l'exemple ci-dessus, des types `SmartId`,
`Weapon` et `Health` sont utilisés. Bien entendu pour que celà fonctionne
correctement, il faut les avoir déclaré quelque part par exemple sous forme de
classe. Mais il est aussi possible de le faire avec des objets javascript, typé
par [JSDoc][1].

## Pourquoi ne pas exploiter typescript à la place de [JSDoc][1] ?

Un chapitre traitera de typescript, mais il faut garder en tête un élément
important. Les classes elfiques sont décritent en ES2015, et non en typescript.
De ce fait, il n'est pas possible de documenter le code directement avec
typescript, mais il faut utiliser une définition dans un fichier annexe. Le fait
de créer un fichier externe fait perdre la vision globale de l'Elfe. En
consultant la définition typescript, on ne voit plus les implémentations. Et en
consultant les implémentations, on ne peut voir les définitions qu'à travers
l'auto-complétion.

Pour que le typescript soit une vraie solution, il faut écrire les Elfe
directement typescript et mettre en place la passe de build pour l'ES2015. Ceci
n'est pas implémenté.

[1]: https://jsdoc.app/
