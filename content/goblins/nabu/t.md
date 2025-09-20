---
title: 'Function T'
weight: 20
---

## Magic with T

![T](/img/nabu.t.png?lightbox=false)

`T( )` est une fonction. Lorsqu'un [widget](/goblins/gadgets) contenant un texte
est affiché (par la fonction `render`), la fonction `T( )` est appelée et Nabu
cherche à ce moment-là quelle est la `string` qu'il faut afficher. Le `context`
de l'application détermine quelle est la langue à utiliser.

{{% notice info %}} Le remplacement de la `string` donnée dans la fonction `T`
s'effectue donc le plus tard possible. C'est là l'un des secrets de la puissance
de Nabu. {{% /notice %}}

```jsx
import T from 't';

render() {
    return <Label text={T('Bonjour')} />;
}
```

### Les paramètres de la fonction T

Le premier paramètre de la fonction `T( )` est la `key` servant à affectuer la
recherche dans le dictionnaire (`'Bonjour'` dans l'exemple ci-dessus). D'autres
paramètres optionnels peuvent être spécifiés:

| Rank   |                                                                    |
| ------ | ------------------------------------------------------------------ |
| First  | Key used to look up the translation in the dictionary.             |
| Second | Translation help text, giving an indication of the context of use. |
| Third  | Map of values to insert in the string to build.                    |

#### Second parameter of T( )

Lorsque le traducteur fait son travail, il est souvent difficile de traduire un
mot hors de son contexte. Le deuxième paramètre donne une aide au traducteur:

```jsx
const text = T('M', 'Letter indicating a month in a date in short format');
```

Une autre solution est de donner ces informations directement dans la `key`:

```jsx
const text = T('M|short|month');
```

Dès que le texte `'M|short|month'` sera traduit, il n'apparaîtra plus dans la
UI.

#### Third parameter of T( )

Il est possible de passer des values pour construire une `string`. Ces values
sont encapsulées dans un `map`:

```jsx
const min = 0:
const max = 999:
const error = T('Enter a value (from {min} to {max})', '', {min, max});

```

Lors du `render`, le texte **'Enter a value (from 0 to 999)'** sera construit et
visible dans la UI.

### ICU

Souvent, les textes doivent s'adapter à un contexte, suivre les règles en vigeur
concernant les pluriels, l'accord des verbes, etc. L'[ICU][1] permet de résoudre
une multitudes de problèmes. Par exemple:

| Parameter | Result                        |
| --------- | ----------------------------- |
| `0`       | Your table is empty           |
| `1`       | Your table contains one value |
| `2`       | Your table contains 2 values  |
| `3`       | Your table contains 3 values  |
| `47`      | Your table is full            |

Voici le parfait exemple de ce qu'il ne faut pas faire:

```jsx
const count = 15;

let text;
if (count === 0) {
  text = T('Empty');
} else {
  text = T('Full');
}
```

Ceci n'est guère mieux:

```jsx
const count = 15;

const text = count === 0 ? T('Empty') : T('Full');
```

Heureusement, l'[ICU][1] est là pour résoudre ce genre de problème élégamment:

```jsx
const count = 15;

const text = T('{count, plural, =0 {Empty} other {Full}}', '', {count});
```

Cette façon d'écrire offre l'avantage de regrouper dans une seule `string` les 2
textes **`Empty`** et **`Full`**. Cela facilitera le travail du traducteur.

| Locale  |                                              |
| ------- | -------------------------------------------- |
| English | `'{count, plural, =0 {Empty} other {Full}}'` |
| French  | `'{count, plural, =0 {Vide} other {Plein}}'` |

Lorsque la syntaxe [ICU][1] devient complexe, il est plus clair de séparer les
lignes:

```jsx
const count = 15;

const text = T(
  `{count, plural,
    =0 {Empty}
    other {Full}
  }`,
  '',
  {count}
);
```

{{% notice warning %}} Attention à l'usage du **back tick** pour spécifier la
`string` passée comme premier paramètre à la fonction `T( )`. Cela est
nécessaire pour pouvoir fragmenter la `string` sur plusieurs lignes. C'est un
problème de syntaxe du JS, cela n'a rien à voir avec l'ICU. {{% /notice %}}

Supposons maintenant que vous voulez afficher ceci, en fonction du nombre
d'utilisateurs contenu dans `count`:

| `count` | Displayed  |
| ------- | ---------- |
| `0`     | "No user"  |
| `1`     | "One user" |
| `2`     | "2 users"  |
| `99`    | "99 users" |

Là encore, l'[ICU][1] résoud le problème:

```jsx
const count = 15;

const text = T(
  `{count, plural,
    =0 {No user}
    =1 {One user}
    other {# users}
  }`,
  '',
  {count}
);
```

Le caractère `#` peut être remplacé par le nom de la variable, entre `{ }`.
Cette façon d'écrire est équivalente:

```jsx
const count = 15;

const text = T(
  `{count, plural,
    =0 {No user}
    =1 {One user}
    other {{count} users}
  }`,
  '',
  {count}
);
```

Et voici un exemple qui construit un texte dépendant de 2 paramètres `count` et
`item`:

```jsx
const count = 15;
const item = 'customer';

const text = T(
  `{count, plural,
    =0 {No {item}}
    =1 {One {item}}
    other {# {item}s}
  }`,
  '',
  {count, item}
);
```

| `count` and `item` | Result         |
| ------------------ | -------------- |
| `0`, `"customer"`  | "No customer"  |
| `1`, `"customer"`  | "One customer" |
| `2`, `"customer"`  | "2 customers"  |
| `0`, `"invoice"`   | "No invoice"   |
| `1`, `"invoice"`   | "One invoice"  |
| `2`, `"invoice"`   | "2 invoices"   |

### Respecter l'orthographe avec ICU et Nabu

Cette expression [ICU][1] contient une règle orthographique courante en anglais
et en français. Au pluriel, "table" s'écrit "table**s**":

```jsx
{count, plural,
  =0 {No table}
  =1 {One table}
  other {# tables}
}
```

Il n'en va pas de même en allemand, où "Tisch" s'écrit "Tisch**e**" au pluriel.
Avec Nabu, on traduit des textes fixes, mais également des expressions [ICU][1]
entières. Donc, pour résoudre ce problème, il suffit de traduire l'expression
ainsi:

```jsx
{count, plural,
  =0 {Kein Tisch}
  =1 {Ein Tisch}
  other {# Tische}
}
```

![Dico](/img/nabu.dico.icu.png?width=700px&lightbox=false)

[1]: http://userguide.icu-project.org/
