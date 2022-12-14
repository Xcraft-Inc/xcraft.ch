---
title: 'Son état'
date: 2022-12-14
weight: 30
tags: ['devel', 'goblin', 'elf', 'state']
pre: '<b>3.4 </b>'
---

Est-ce que votre Elfe est malade ? Il est dans quel état ? Ici on ne traite pas
de ses caractéristiques, mais bien de son état général.

## Donner un état à votre Elfe

Maintenant que vous avez vu à quoi ressemblent les propriétés, qui sont
l'équivalent des `setX` Goblins, voyons comment faire pour donner un état
(`state`) à un Elfe. Pour rappel, le `state` doit contenir uniquement des
valeurs sérialisables (il peut y avoir des exceptions non documentées ici). Les
différences entre les propriétés et les `states` ne se limitent pas au
`protected` / `public` mais à ce qu'on désire faire avec le service. Le `state`
permet de gérer les mutations dans une fonction pure (sans effet de bord) que
l'on nomme comme `reducer`. Il est possible ainsi de réaliser des tests
efficacement sur les `states` sans faire intervenir les effets de bords qui
doivent être produits **uniquement par les quêtes**. De plus, les `states` sont
immutables par nature, ce qui donne de nombreux avantages pour le fonctionnement
du framework. Néanmoins, concernant les Elfes, les `states` sont mutables
contrairement au Goblins.

> Les `states` doivent également être utilisé pour la persistance des données
> contrairement aux propriétés.

Dans l'exemple ci-dessous vous pouvez voir un `state.set()` où la valeur de
retour n'est pas récupérée. En effet, ici, vous avez bien un `state` de type
`Shredder` mais celui-ci est mutable. Bien entendu, en dehors du reducer, le
`state` est toujours immutable.

```js
class Elrond extends Elf {
  static initialState = {
    yearsOfLife: 0,
  };

  async create() {}

  static nextYear(state) {
    const previousYears = state.get('yearsOfLife');
    state.set('yearsOfLife', previousYears + 1);
  }

  async nextYear() {
    this.do();
    return this.state.get('yearsOfLife');
  }

  delete() {}
}
```

Regardez bien l'exemple ci-dessus. Le reducer correspond à la quête car il a le
**même nom**. La différence fondamentale vient du fait que le reducer `nextYear`
est une fonction `static` et non une méthode d'instance comme pour la quête.
Contrairement aux Goblins, vous pouvez ainsi écrire les reducers directement
avant ou après la définition de la quête; ce qui peut être un plus non
négligeable pour la lecture du code.

A propos de l'état initial, vous devez simplement rajouter une propriété static
`initialState` à votre Elfe.

Etant donné que le `state` vu depuis le reducer est mutable, parfois il est
nécessaire d'avoir accès au state immutable pour réaliser (par exemple) des
comparaisons. Tous les reducers elfiques recoivent alors trois arguments comme
présenté ci-dessous :

```js
  /* ... */
  static nextYear(state, action, immState) {}
  /* ... */
```

En entrant dans le reducer, `state` et `immState` contiennent les mêmes valeurs.
Néanmoins les mutations sur `state` n'affectent pas `immState` qui peut alors
être utilisé pour faire des comparaisons pendant l'exécution du reducer.

## Où est l'auto-complétion des states ?

Pour gérer de la complétion avec les `states`, il faut créer des modèles et les
mapper sur des classes ou alors sur des définitions typescripts. Cette
problématique est en cours d'étude mais rien n'est encore implémenté.
