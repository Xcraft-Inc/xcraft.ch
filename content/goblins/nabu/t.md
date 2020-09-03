---
title: 'Function T'
date: 2020-07-24
weight: 20
draft: true
---

## Magic with T

![T](/img/nabu.t.png?width=800px)

`T( )` est une fonction. Lorsqu'un [widget](/goblins/gadgets) contenant un texte
est affiché (par la fonction `render`), la fonction `T( )` est appelée et Nabu
cherche à ce moment-là quel est la `string` qu'il faut afficher. Le `context` de
l'application détermine quelle est la langue à utiliser.

{{% notice info %}} Le remplacement des la `string` donnée dans la fonction `T`
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
mot hors de son contexte. Le deuxième paramètre permet de donner une aide au
traducteur:

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

Il est possible de passer des values pour construire une `string`, dans un
`map`:

```jsx
const min = 0:
const max = 999:
const error = T('Enter a value (from {min} to {max})', '', {min, max});

```

Lors du `render`, le texte **'Enter a value (from 0 to 999)'** sera généré.

### ICU

Souvent, les textes doivent s'adapter à un contexte, suivre les règles en vigeur
concernant les pluriels, l'accord des verbes, etc. L'[ICU][1] permet de résoudre
une multitudes de problèmes.

| Parameters                  | Result                                              |
| --------------------------- | --------------------------------------------------- |
| `1`, `"table"`              | Create one customer in **a** table                  |
| `2`, `"table"`              | Create 2 customer**s** in **a** table               |
| `47`, `"empty dictionnary"` | Create 47 customer**s** in **an** empty dictionnary |

Voici le parfait exemple de ce qu'il ne faut pas faire:

```jsx
render() {
    let description
    if (this.props.count === 0) {
        description = T('Empty');
    } else {
        description = T('Full');
    }
    return <Label text={description} />;
}
```

Ceci n'est guère mieux:

```jsx
render() {
    const description = this.props.count === 0 ? T('Empty') : T('Full');
    return <Label text={description} />;
}
```

Heureusement, l'[ICU][1] est là pour résoudre ce genre de problème élégamment:

```jsx
render() {
    const description = T(
      `{n, plural,
        =0 {Empty}
        other {Full}
      }`,
      '',
      {n: this.props.count}
    );
    return <Label text={description} />;
}
```

{{% notice info %}} Cela peut sembler compliqué pour un exemple si simple. Mais
nous vous encourageons à coder ainsi. {{% /notice %}}

{{% notice warning %}} Attention à l'usage du **back tick** pour spécifier la
`string` passée comme premier paramètre à la fonction `T( )`. {{% /notice %}}

Supposons maintenant que vous voulez afficher ceci, en fonction du nombre
d'utilisateurs contenu dans `this.props.count`:

| `this.props.count` | Displayed  |
| ------------------ | ---------- |
| `0`                | "No user"  |
| `1`                | "One user" |
| `2`                | "2 users"  |
| `99`               | "99 users" |

Là encore, l'[ICU][1] résoud le problème:

```jsx
render() {
    const description = T(
      `{n, plural,
        =0 {No user}
        one {One user}
        other {# users}
      }`,
      '',
      {n: this.props.count}
    );
    return <Label text={description} />;
}
```

{{% notice info %}} Il est possible d'écrire `one {One user}` ou `=1 {One user}`
indifféremment. {{% /notice %}}

Et voici un exemple de function qui retourne un texte dépendant de 2 paramètres
`count` et `item`:

```jsx
function getDescription(count, item) {
  return T(
    `{count, plural,
        =0 {No {item}}
        one {One {item}}
        other {# {item}s}
      }`,
    '',
    {count, item}
  );
}
```

| `count` and `item` | Result         |
| ------------------ | -------------- |
| `0`, `"customer"`  | "No customer"  |
| `1`, `"customer"`  | "One customer" |
| `2`, `"customer"`  | "2 customers"  |
| `0`, `"invoice"`   | "No invoice"   |
| `1`, `"invoice"`   | "One invoice"  |
| `2`, `"invoice"`   | "2 invoices"   |

[1]: https://nodejs.org/api/intl.html
