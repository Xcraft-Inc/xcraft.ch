---
title: 'Make your own gadgets'
date: 2020-07-24
weight: 40
draft: true
---

## Make your own gadgets

![smile](/img/smile5.png?width=250px)

Nous allons créer un widget `smiley` qui affiche un indice de satisfaction à
partir d'une valeur comprise entre 0 (triste) et 100 (content). Pour cela, il
faut créer un nouveau dossier `smiley` dans le dossier `widgets` de votre app,
qui contiendra 2 fichiers:

| File                       | Content                   |
| -------------------------- | ------------------------- |
| `widgets/smiley/widget.js` | Class of the widget.      |
| `widgets/smiley/styles.js` | Styles CSS of the widget. |

Le squelette de notre fichier `widgets/smiley/widget.js` doit contenir ceci:

```jsx
import React from 'react';
import Widget from 'goblin-laboratory/widgets/widget';
import * as styles from './styles';

export default class Smiley extends Widget {
  constructor() {
    super(...arguments);
    this.styles = styles;
  }

  render() {
    return null;
  }
}
```

Pour débuter simplement, créons un widget qui affiche un rond jaune:

```jsx
render() {
    return <div className={this.styles.classNames.smiley} />
}
```

Le fichier `widgets/smiley/styles.js` doit contenir le style CSS nommé `smiley`:

```jsx
export default function styles() {
  const smiley = {
    width: '200px',
    height: '200px',
    borderRadius: '200px',
    border: '10px solid black',
    background: 'yellow',
  };

  return {
    smiley,
  };
}
```

{{% notice warning %}} La valeur de `200px` pour le rayon n'est pas une erreur,
mais une simplification. En toute logique, il faudrait donner la moitié du
diamètre (`100px`) plus l'épaisseur du border (`10px`), mais heureusement, CSS
tronque la valeur si elle est trop grande, et on obtient donc bien un cercle.
{{% /notice %}}

{{% notice info %}} La gestion des styles utilise un cache pour plus
d'efficacité, avec la lib [aphrodite](https://github.com/Khan/aphrodite).
{{% /notice %}}

Dans le widget parent qui instancie le smiley, il faut écrire ceci:

```jsx
import Smiley from 'goblin-gadgets/widgets/smiley/widget';

...
render() {
    return (
        <Container>
            ...
            <Smiley />
            ...
        </Container>
    )
}
```

Et sans surprise, nous obtenons le résultat suivant:

![step 1](/img/gadgets.doit.step1.png?width=200px)

## Avec des yeux et un sourire

Ajoutons deux yeux et un sourire dans `widgets/smiley/widget.js`:

```jsx
render() {
    return (
        <div className={this.styles.classNames.smiley}>
            <div className={this.styles.classNames.leftEye} />
            <div className={this.styles.classNames.rightEye} />
            <div className={this.styles.classNames.smile} />
        </div>
    )
}
```

Dans `widgets/smiley/styles.js`, il nous faut ajouter 3 nouveaux styles CSS
`leftEye`, `rightEye` et `smile`:

```jsx
export default function styles() {
  const smiley = {
    position: 'relative',
    width: '200px',
    height: '200px',
    borderRadius: '200px',
    border: '10px solid black',
    background: 'yellow',
  };

  const _eye = {
    position: 'absolute',
    top: '60px',
    width: '20px',
    height: '20px',
    borderRadius: '20px',
    backgroundColor: 'black',
    transform: 'scaleY(2)',
  };

  const leftEye = {
    ..._eye,
    left: '70px',
  };

  const rightEye = {
    ..._eye,
    right: '70px',
  };

  const smile = {
    position: 'absolute',
    bottom: '40px',
    left: '30px',
    right: '30px',
    height: '60px',
    borderRadius: '0 0 150px 150px / 0 0 150px 150px',
    border: '10px solid black',
    borderTop: 0,
  };

  return {
    smiley,
    leftEye,
    rightEye,
    smile,
  };
}
```

{{% notice info %}} C'est une bonne pratique de nommer le premier style (celui
de la `div` racine) avec le même nom que le composant (ici `smiley`). Lorsque
vous inspecterez le DOM, il sera ainsi plus facile de repérer le widget. Les
styles des enfants (ici `leftEye`, `rightEye` et `smile`) peuvent être nommés
librement. {{% /notice %}}

Nous obtenons le magnifique résultat suivant:

![step 2](/img/gadgets.doit.step2.png?width=200px)

## The properties

Notre smiley a une taille fixe de `200px`. Il serait cool de pouvoir spécifier
sa taille à l'aide des propriétés, et permettre d'écrire ceci dans le widget
parent qui instancie le smiley:

```jsx
render() {
    return <Smiley size="500px" />
}
```

Pour cela, il faut améliorer `widgets/smiley/styles.js` en déclarant la
propriété `size`:

```jsx
export const propNames = ['size'];

export default function styles(theme, props) {
  const smiley = {
    width: props.size,
    height: props.size,
    borderRadius: props.size,
    border: '10px solid black',
    background: 'yellow',
  };

  return {
    smiley,
  };
}
```

Si la propriété `size` est omise, il est bien utile d'avoir une valeur par
défaut. On peut écrire ceci:

```jsx
export const propNames = ['size'];

export default function styles(theme, props) {
  const {size = '200px'} = props;

  const smiley = {
    width: size,
    height: size,
    borderRadius: size,
    border: '10px solid black',
    background: 'yellow',
  };

  return {
    smiley,
  };
}
```

## And more...

Avec un peu de travail dans le CSS, et vous pourrez obtenir ceci:

![step 3](/img/gadgets.doit.step3.png?width=200px)

Dans le widget définitif, il sera possible d'ajouter une propriété
`satisfaction` (on voir ici les résultats avec les valeurs `100`, `75`, `50`,
`25` et `0`):

![step 4](/img/gadgets.doit.step4.png)

Et pourquoi pas une propriété `color`?

![step 5](/img/gadgets.doit.step5.png)

Pour obtenir ces variantes, il suffit d'écrire:

```jsx
render() {
    return (
        <Container>
            <Smiley size="200px" satisfaction={100} mainColor="#0ff" />
            <Smiley size="200px" satisfaction={100} mainColor='#f0f' topColor='#0ff' />
            <Smiley size="200px" satisfaction={75} mainColor='#08f' topColor='#0ff' />
            <Smiley size="200px" satisfaction={25} mainColor='#0a0' topColor='#ff0' />
            <Smiley size="200px" satisfaction={0} mainColor='#fa0' topColor='#f00' />
        </Container>
    )
}
```

Vous trouverez le widget définitif dans `goblin-gadgets/widgets/smiley`. Il est
possible de le tester avec [WidgetDoc](/goblins/gadgets/widgetdoc).
