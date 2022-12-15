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
l'on nomme `reducer`. Il est possible ainsi de réaliser des tests efficacement
sur les `states` sans faire intervenir les effets de bords qui doivent être
produits **uniquement par les quêtes**. De plus, les `states` sont immutables
par nature, ce qui donne de nombreux avantages pour le fonctionnement du
framework. Néanmoins, concernant les Elfes, les `states` sont mutables
contrairement aux Goblins.

> Les `states` doivent également être utilisé pour la persistance des données
> contrairement aux propriétés.

Bien qu'on parle toujours de reducer, les Elfes n'utilisent pas le même
prototype qu'un reducer habituel. Vous connaissez bien le reducer
`(state, action) => state.set()`. Avec les Elfes, l'action est décomposé en
arguments et le `state` devient le `this` de la fonction pour ressembler à
`(val1, val2, ..., valN) => this.set()`.

Dans l'exemple ci-dessous vous pouvez voir `this.set()`. En effet, ici, vous
avez bien un `state` de type `Shredder` mais celui-ci est mutable. Bien entendu,
en dehors du reducer, le `state` est toujours immutable.

```js
class Elrond extends Elf {
  static initialState = {
    yearsOfLife: 0,
  };

  async create(id, desktopId = null) {
    this.do();
    return this;
  }

  /* Le reducer (pur) */
  static nextYear() {
    const previousYears = this.get('yearsOfLife');
    this.set('yearsOfLife', previousYears + 1);
  }

  /* La quête (effets de bord) */
  async nextYear() {
    this.do();
    return this.state.get('yearsOfLife');
  }
}
```

Regardez bien l'exemple ci-dessus. Le reducer correspond à la quête car il a le
**même nom**. La différence fondamentale vient du fait que le reducer `nextYear`
est une fonction `static` et non une méthode d'instance comme pour la quête.
Contrairement aux Goblins, vous pouvez ainsi écrire les reducers directement
avant ou après la définition de la quête; ce qui peut être un plus non
négligeable pour la lecture du code.

> Si vous avez bien observé, le `create` fait un appel sur `this.do()`. Sachez
> que les quêtes `create` ont un reducer par défaut qui sert uniquement à
> insérer l`id` dans le `state` de l'Elfe, si aucun reducer n'est défini dans la
> classe dérivée.

A propos de l'état initial, vous devez simplement rajouter une propriété static
`initialState` à votre Elfe.

Etant donné que le `state` vu depuis le reducer est mutable, parfois il est
nécessaire d'avoir accès au state immutable pour réaliser (par exemple) des
comparaisons. Tous les reducers elfiques recoivent le `state` immutable comme
dernier argument du reducer :

```js
static nextYear(val1, val2, ..., valN, immState) {}
```

En entrant dans le reducer, `this` et `immState` contiennent les mêmes valeurs.
Néanmoins les mutations sur `this` n'affectent pas `immState` qui peut alors
être utilisé pour faire des comparaisons pendant l'exécution du reducer.

## Comment injecter des propriétés supplémentaires dans le reducer ?

Comme vous le savez bien avec les Goblins, tous les paramètres donnés à une
quête sont forcément disponibles dans l'`action` passé au reducer. Pour ce qui
est des Elfes, c'est exactement la même chose, et comme avec les Goblins, il est
possible d'en donner d'autres.

L'exemple ci-dessous permet de montrer que l'on peut récupérer directement `id`
depuis le reducer, mais également `age` qui ne fait pas partie du prototype de
la quête `create`. Ce n'est pas un problème car `age` est donné explicitement
avec l'appel sur le `do()`.

```js
class Elrond extends Elf {
  static create(id, age) {
    this.set('id', id).set('age', age);
  }

  async create(id, desktopId = null) {
    this.do({age: 143});
    return this;
  }
}
```

> Si on avait aussi spécifiez `desktopId` dans le prototype de la méthode static
> `create`, on aurait également pu le récupérer directement sans le spécifier
> avec l'appel `this.do()`.

## Où est l'auto-complétion des states ?

Pour gérer de la complétion avec les `states`, il faut créer des modèles et les
mapper sur des classes ou alors sur des définitions typescripts. Cette
problématique est en cours d'étude mais rien n'est encore implémenté.
