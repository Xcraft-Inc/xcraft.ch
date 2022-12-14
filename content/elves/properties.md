---
title: 'Les caractéristiques'
date: 2022-12-09
weight: 10
tags: ['devel', 'goblin', 'elf']
pre: '<b>3.3 </b>'
---

Chaque Elfe peut avoir ses propres caractéristiques. Ici on parle de propriétés protégées de l'instance du service. Vous devriez connaître les méthodes `setX`, `getX` et `delX` des Goblins. Pour les Elfes c'est un peu différents car les registres Goblins ne sont plus directement utilisés; mais l'idée est la même.

## Déclarer des propriétés

Il est vivement recommandé de déclarer ces propriétés avec la convention du préfixe `_` pour indiquer qu'elles sont `protected`. En effet, inutile d'essayer d'utiliser la syntaxe `#` pour espérer créer des propriétés `private` car le fonctionnement des Goblins ne le **permet pas**. De même, il n'est pas judicieux de les déclarer comme publiques (sans le préfixe `_`) afin de ne pas être tenté de les appeler depuis un autre Goblin faisant référence à celui-ci. En effet, ces propriétés sont exclusivement `protected` et uniquement des quêtes pourraient permettre des les rendre accessibles depuis l'extérieur.

```js
class Elrond extends Elf {
  _yearsOfLife = 0;

  async create() {}

  async nextYear() {
    this._yearsOfLife++;
    return this._yearsOfLife;
  }
  
  delete() {}
}
```

Il n'est pas possible de réaliser un `delX` comme avec les Goblins. Les Elfes sont plus proches des classes habituelles. Oubliez le `delX` et contentez-vous de garder vos propriétés cohérentes dans votre Elfe. Il est toujours possible d'assigner la valeure `null` à une propriéter au lieu de la supprimer comme le fait `delX` des Goblins.

{{% notice info %}} Les Elfes sont une abstraction sur les Goblins, mais les propriétés elfiques ne sont en aucun cas simulées par des `setX` et `getX` Goblins. {{% /notice %}}

## Donner un état à votre Elfe

Maintenant que vous avez vu à quoi ressemblent les propriétés, qui sont l'équivalent des `setX` Goblins, voyons comment faire pour donner un état (`state`) à un Elfe. Pour rappel, le `state` doit contenir uniquement des valeurs sérialisables (il peut y avoir des exceptions non documentées ici). Les différences entre les propriétés et les `states` ne se limitent pas au `protected` / `public` mais à ce qu'on désire faire avec le service. Le `state` permet de gérer les mutations dans une fonction pure (sans effets de bord) que l'on nomme comme `reducer`. Il est possible ainsi de réaliser des tests efficacement sur les `states` sans faire intervenir les effets de bords qui doivent être produits **uniquement par les quêtes**. De plus, les `states` sont immutables par nature, ce qui donne de nombreux avantages pour le fonctionnement du framework. Néanmoins, concernant les Elfes, les `states` sont mutables contrairement au Goblins. Dans l'exemple ci-dessous vous pouvez voir un `state.set` où la valeur de retour n'est pas récupérées. En effet, ici, vous avez bien un `state` de type `Shredder` mais celui-ci est mutable. Bien entendu, en dehors du reducer, le state est toujours immutable.

```js
class Elrond extends Elf {
  static initialState = {
    yearsOfLife: 0
  };

  async create() {}

  static nextYear(state) {
    state.set('yearsOfLife', state.get('yearsOfLife') + 1);
  }

  async nextYear() {
    this.do();
    return this.state.get('yearsOfLife');
  }
  
  delete() {}
}
```

Regardez bien l'exemple ci-dessus. Le reducer correspond à la quête car il a le **même nom**. La différence fondamentale vient du fait que le reducer `nextYear` est une fonction `static` et non une méthode d'instance comme pour la quête. Contrairement aux Goblins, vous pouvez ainsi écrire les reducers directement avant ou après la définition de la quête; ce qui peu être un plus non négligeable pour la lecture du code.

Etant donné que le `state` vu depuis le reducer est mutable, parfois il est nécessaire d'avoir accès au state immutable pour réaliser (par exemple) des comparaisons.Tous les reducers elfiques recoivent alors trois arguments comme présenté ci-dessous :

```js
  /* ... */

  static nextYear(state, action, immState) {}

  /* ... */
```

En entrant dans le reducer, `state` et `immState` contiennent les mêmes valeurs. Néanmoins les mutations sur `state` n'affectent pas `immState` qui peut alors être utilisé pour faire des comparaisons pendant l'exécution du reducer.

## Oû est l'auto-complétion des `states` ?

Pour gérer de la complétion avec les `states`, il faut créer des modèles et les mapper sur des classes ou alors sur des définitions typescripts. Cette problématique est en cours d'étude mais rien n'est encore implémenté.