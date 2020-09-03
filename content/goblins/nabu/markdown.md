---
title: 'Markdown Builder'
date: 2020-07-24
weight: 40
draft: true
---

## Rich texts with MarkdownBuilder

Sans suprise, `MarkdownBuilder` permet de construire des `string` [markdown][1].
Construisons une liste de héros:

```jsx
const heroes = ['John Difool', 'Bragon', 'Achile Talon', 'Colombe Tiredaile'];
```

Pour générer une `string` markdown, rien de plus facile:

```jsx
import MarkdownBuilder from 'goblin-workshop/lib/markdown-builder.js';

const MD = new MarkdownBuilder();

function getHeroesList(heroes) {
  MD.flush();
  MD.addTitle(MD.bold('My favorite heroes'));
  MD.addUnorderedList(heroes);
  return MD.toString();
}
```

Ce texte `markdown` peut ensuite être affiché par le widget `Label`:

```jsx
render() {
    return <Label text={getHeroesList(this.props.heroes)} />;
};
```

### MarkdownBuilder and Nabu

Les textes passés à `MarkdownBuilder` peuvent être des functions `T( )`:

```jsx
const MD = new MarkdownBuilder();

const colors = [T("Red"), T("Green"), T("Blue")];

  MD.flush();
  MD.addTitle(MD.bold(T('Colors')));
  MD.addUnorderedList(colors);
  const list = MD.toString();
};
```

[1]: https://en.wikipedia.org/wiki/Markdown
