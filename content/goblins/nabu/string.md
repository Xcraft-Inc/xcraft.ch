---
title: 'String Builder'
weight: 30
---

## Simple texts with StringBuilder

![string](/img/nabu.string.png?width=600px)

Si vous devez combiner 2 `string` dans un même texte, rien de plus simple:

```jsx
const t1 = 'Hello';
const t2 = 'World';
const result = t1 + ' ' + t2;
```

Mais si les textes sont des functions `T( )`, cela se complique. Vous ne pouvez
pas faire ceci:

```jsx
const t1 = T('Hello');
const t2 = T('World');
const result = t1 + ' ' + t2;
```

{{% notice warning %}} `t1` et `t2` sont des fonctions. Il est donc évident
qu'on ne peut pas construire la `string` ainsi! {{% /notice %}}

Pour cela, il faut utilser `StringBuilder`, et écrire:

```jsx
import StringBuilder from 'goblin-nabu/lib/string-builder.js';

const t1 = T('Hello');
const t2 = T('World');
const result = StringBuilder.combine(t1, ' ', t2);
```

### StringBuilder

Si nous voulons construire la description d'une personne à partir de son prénom
et de son nom, on peut être tenté d'écrire:

```jsx
function getName(firstName, lastName) {
  return firstName + ' ' + lastName;
}
```

Mais il y a un problème si le prénom et/ou le nom n'existent pas:

| `firstName` and `lastName` | Result                                                       |
| -------------------------- | ------------------------------------------------------------ |
| `"John"`, `"Difool"`       | "John Difool" _(perfect)_                                    |
| `""`, `"Difool"`           | " Difool" _(problem, there is a space in front of the name)_ |
| `null`, `"Difool"`         | " Difool" _(problem, there is a space in front of the name)_ |
| `"John"`, `""`             | "John " _(problem, there is a space after the name)_         |
| `"John"`, `null`           | "John " _(problem, there is a space after the name)_         |

Heureusement, `StringBuilder` a ce qu'il faut:

```jsx
function getName(firstName, lastName) {
  return StringBuilder.joinWords(firstName, lastName);
}
```

Cette function accepte également un tableau:

```jsx
const array = ["It's", 'magic!'];
const text = StringBuilder.joinWords(array);
```

{{% notice info %}} `joinWords` fonctionne bien entendu avec des `string` ou des
fonctions `T( )`. {{% /notice %}}

{{% notice info %}} Il existe plusieurs autres fonctions utiles pour appondre
des textes. Citons `joinSentences`, `joinHyphen` et `joinLines`. Quant à lui,
`join` permet de choisir le séparateur. {{% /notice %}}

### Summary

| Function        | Action                                                          |
| --------------- | --------------------------------------------------------------- |
| `joinWords`     | Appends several texts, separated by spaces.                     |
| `joinSentences` | Appends several texts, separated by commas and spaces (`", "`). |
| `joinHyphen`    | Appends several texts, separated by hyphens (U+2014).           |
| `joinLines`     | Appends several lines.                                          |
| `join`          | Appends several texts, with a separator of your choice.         |
| `combine`       | Appends sevral texts as is.                                     |

{{% notice note %}} `StringBuilder` est une classe statique. Il n'est donc pas
nécessaire de l'instancier. {{% /notice %}}
