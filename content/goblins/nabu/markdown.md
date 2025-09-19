---
title: 'Markdown Builder'
weight: 40
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

{{% notice warning %}} `MarkdownBuilder` n'est pas une classe statique. Il faut
l'instancier avant de l'utiliser avec `new MarkdownBuilder()` . L'instance
contient le texte en cours de construction. Si vous réutilisez une instance
existante (`MD` dans cet exemple), il faut donc prendre l'habitude de toujours
commencer par vider son contenu avec `MD.flush()`. En fait, c'est une bonne
habitude de le faire chaque fois. {{% /notice %}}

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

const colors = [T('Red'), T('Green'), T('Blue')];

MD.flush();
MD.addTitle(MD.bold(T('Colors')));
MD.addUnorderedList(colors);
const result = MD.toString();
```

Il est bien entendu possible d'utiliser l'[ICU](/goblins/nabu/t):

```jsx
const MD = new MarkdownBuilder();

const t1 = T('Example');
const t2 = T('{n, plural, =0 {Empty} other {Full}}', '', {n: 99});
const list = [t1, t2];

MD.flush();
MD.addUnorderedList(list);
const result = MD.toString();
```

### Summary

| Function           | Action                                                          |
| ------------------ | --------------------------------------------------------------- |
| `flush`            | Flush the content.                                              |
| `joinWords`        | Appends several texts, separated by spaces.                     |
| `joinSentences`    | Appends several texts, separated by commas and spaces (`", "`). |
| `joinHyphen`       | Appends several texts, separated by hyphens (U+2014).           |
| `joinLines`        | Appends several lines.                                          |
| `join`             | Appends several texts, with a separator of your choice.         |
| `bold`             | Return the text in bold.                                        |
| `italic`           | Return the text in Italic.                                      |
| `addTitle`         | Add a title.                                                    |
| `addBlock`         | Add a bloc of text.                                             |
| `addBlocks`        | Add many blocs of text.                                         |
| `addUnorderedItem` | Add an item to a unordered list.                                |
| `addUnorderedList` | Add many items to a unordered list.                             |
| `addOrderedItem`   | Add an item to a ordered list.                                  |
| `addOrderedList`   | Add many items to a ordered list.                               |
| `toString`         | Return the final text (with markdown structure, of course).     |

[1]: https://en.wikipedia.org/wiki/Markdown
