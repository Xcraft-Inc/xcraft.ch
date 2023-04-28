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
produits **uniquement par les quêtes**. De plus, les `states` sont immuables
(immutables) par nature, ce qui donne de nombreux avantages pour le
fonctionnement du framework. Néanmoins, concernant les Elfes, les `states` sont
mutables (du point de vue du consomateur) contrairement aux Goblins.

Les `states` doivent également être utilisés pour la persistance des données
contrairement aux propriétés.

> Bien qu'on parle toujours de reducer, les Elfes n'utilisent pas le même
> prototype qu'un reducer habituel. Vous connaissez bien le reducer :

```js
(state, action) => state.set();
```

> Avec les Elfes, l'action est décomposée en arguments et le `state` est injecté
> dans le `this`. et devient `this.state` pour ressembler à :

```js
(val1, val2, ..., valN) => this.state.valN = valN;
```

Dans l'exemple ci-dessous vous pouvez voir `this.state.valN = valN`. Connaissant
bien les Goblins, vous devriez être surpris. Il n'y a plus besoin d'effectuer un
`.set()` pour assigner une valeur. De même avec la lecture, le `.get()` n'existe
plus.

Bien entendu, en dehors du reducer, le `state` est toujours immutable. Voici un
exemple complet.

### Les Elfes se doivent d'être beau

Un Elfe est un être typé. Il connait son état dans ses moindres détails.

```js
const {string, number} = require('xcraft-core-stones');

class ElrondShape {
  id = string;
  name = string;
  yearsOfLife = number;
}

class ElrondState extends Elf.Sculpt(ElrondShape) {}
```

L'exemple ci-dessus est un _shape_. Ce _shape_ permet de décrire précisément à
quoi doit ressembler un _state_. Une fonction elfique particulière,
`Elf.Sculpt()`, permet de sculpter un _state_ elfique à partir d'un _shape_.

Pour bien comprendre les termes, voyez la classe _shape_ comme une définition,
et la classe _state_ comme un type. C'est ce type qui va permettre d'instancier
un _state_ à notre Elfe.

### Les Elfes ont de la logique

Quand un Elfe réfléchit, il se fie à sa mémoire structurée et bien définie.
Cette mémoire est une instance du type décrit précédemment.

```js
class ElrondLogic extends Elf.Spirit {
  /* Instance du state Elrond avec des valeurs initiales */
  state = new ElrondState({
    name: 'Elrond',
    yearsOfLife: 0,
  });

  /* Reducer pour la quête create */
  create(id, desktopId) {
    this.state.id = id;
  }

  /* Reducer pour la quête nextYear */
  nextYear() {
    this.state.yearsOfLife++;
  }
}
```

La logique de l'Efle doit dérivé de l'esprit Elfe `Elf.Spirit`. En effet, tous
les Elfes partagent les mêmes fondamentaux.

Nous sommes ici dans la définitions des reducers et de l'état initial. La
propriété `state` est réservée à ce but et doit **toujours** se nommer ainsi. Le
`this` des reducers est un peu particulier. Comprennez bien que le `this` ne
représente pas le `this` de la classe Elfe. Les reducers n'ont pas vraiment
changé par rapport aux Goblins. Vous êtes ici dans le résultat d'un _dispatch_
Redux où tout est pure. C'est depuis ce `this` que vous pouvez récupérer (si
vous le souhaitez) le _state_ immutable via `this.immutable`.

### Les Elfes partent en quêtes

La classe de l'Elfe ...

```js
class Elrond extends Elf {
  state = new ElrondState();

  /* La quête (constructeur) */
  async create(id, desktopId = null) {
    this.do();
    return this;
  }

  /* La quête (effets de bord) */
  async nextYear() {
    this.do();
    return this.state.yearsOfLife;
  }
}
```

L'Elfe décrit dans sa classe, en plus des propriétés, ses quêtes. Les reducers
sont séparés volontairement et peuvent alors être testés indépendamment de
l'Elfe.

Pour atteindre l'état de l'Elfe, il suffit d'utiliser la propriété `this.state`.
Notez bien que pour y avoir accès, vous devez la déclarer comme propriété de
l'Elfe.

> Peut-être que vous trouvez celà étrange car vous avez aussi déclaré une
> propriété `state` dans la classe `ElrondLogic`. Je vous propose qu'on garde un
> peu de magie. Faites comme expliqué dans cette documentation et tout ira bien.
> N'utilisez pas l'instance de `state` dans l'Elfe pour y passer les valeurs
> initiales (ça ne fonctionnera pas).

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
class ElrondLogic extends Elf.Spirit {
  /* ... */

  create(id, age) {
    this.state.id = id;
    this.state.age = age;
  }
}

class Elrond extends Elf {
  /* ... */

  async create(id, desktopId = null) {
    this.do({age: 143});
    return this;
  }
}
```

> Si on avait aussi spécifié `desktopId` dans le prototype de la méthode
> `create` de la logique, on aurait également pu le récupérer directement sans
> le spécifier avec l'appel `this.do()`.

## Exporter la logique, sinon votre Elfe va rester stupide

C'est à la naissance de l'Elfe que sa logique est construite. Les bébé Elfes ne
sont pas comme les autres espèces.

```js
const {Elf} = require('xcraft-core-goblin');
const Elrond = require('./lib/elrond/service.js');
const ElrondLogic = require('./lib/elrond/logic.js');

exports.xcraftCommands = Elf.birth(Elrond, ElrondLogic);
```

Voir aussi : [](/elves/born)
