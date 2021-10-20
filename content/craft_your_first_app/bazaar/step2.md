---
title: 'Le Bazaar Goblin - Etape 2'
date: 2020-10-06
weight: 20
chapter: true
pre: '<b>4.2 </b>'
---

### Chapter 4.2

# Le Bazaar Goblin - Etape 2

Yeah! Après avoir connecté quelques tuyaux et fait exploser la moitié du
laboratoire, on arrive à la deuxième étape!

Il faudra ouvrir **bazaar-dev** dans ton IDE, et lancer un terminal.

Nous allons commencer par représenter les différents contenus qui seront stocké
dans notre bazaar d'informations. Le plus simple c'est de demander à notre
commande **goblins** de **crafter** un **workbench** (établis) pour travailler
autours de l'entité **bazaarContent**.

`npx goblins craft workbench bazaarContent`

Cette commande va crafter tout les fichiers nécessaires:

![Workbench](/img/bazaar_workbench.png?width=600px)

## Préparation du modèle de données

Nous allons modifier le fichier généré pour se rapprocher de notre idéal, et
procéder a quelques adaptations, qui nous permettrons de découvrir au passage,
un tas de bidouille bien pratiques!

Ouvre l'entité **bazaarContent.js** qui se trouve dans le dossier
**/entities/**.

### Astuce: tu peux utiliser _ctrl+p_ pour rechercher un fichier!

![Ctrl+p](/img/bazaar_ctrl_p.png?width=600px)

### Le sujet et le contenu

Nous allons remplacer les propriétés proposée dans le modèle et ajouter _about_
et _content_ afin de que l'utilisateur puisse décrire et saisir le contenu.

Repérer la section `properties:` dans la définition de l'entité, et la remplacer
par ceci:

```js
properties: {
    about: {type: 'string', defaultValue: '', text: T('Sujet')},
    content: {type: 'string', defaultValue: '', text: T('Contenu')},
  },
```

### Adapter les résumés textuels

Nous venons de changer le modèle et nous devons directement aller modifier la
fonction de construction des **summaries**.

Repérer la section `buildSummaries:` dans la définition de l'entité afin de
corriger l'utilisation de la propriété **name** et **status**:

```js
  buildSummaries: function (quest, entity, peers, MD) {
    const {about, content} = entity.pick('about', 'content');
    MD.addTitle(MD.bold(about));
    MD.addBlock(content);
    return {info: about, description: MD.toString()};
  },
```

On en profite pour ajoute le contenu en bloque, avec l'aide de la classe **MD**
(_markdown_)

Les propriétés **info** et **description** sont utilisé en standard dans
différents endroits de la UI, nous n'allons rien faire de plus pour les
**summaries**.

## Adapter le formulaire UI

A l'aide de _ctrl+p_ rechercher un fichier `UI.js`, actuellement un seul fichier
est proposé, c'est celui qui permet de définir les différentes présentations de
notre entité **bazaarContent**.

Tu peux remplacer les deux fonctions de rendu (_renderPanel_ , _renderPlugin_)
par ceci:

```js
/******************************************************************************/

function renderPanel(props, readonly) {
  return (
    <Container kind="column" grow="1">
      <Container kind="pane">
        <Field model=".about" />
        <Field model=".content" rows={20} />
      </Container>
    </Container>
  );
}

function renderPlugin(props, readonly) {
  return (
    <Container kind="column" grow="1">
      <Container kind="row">
        <Field model=".about" />
        <Field model=".content" />
      </Container>
    </Container>
  );
}

/******************************************************************************/
```

Le composant `<Field>` permet d'effectuer le lien avec le modèle de données a
l'aide de la propriété `model=".xxx"`.

Pour le champ qui represente le contenu, en mode _panel_ nous avons configuré du
multi-lignes `row=20`.

## Adapter les services de recherches

A l'aide de _ctrl+p_ rechercher un fichier contenant `search`, c'est celui
(_bazaarContent-search.js_) qui permet de définir la configuration des listes de
recherches de nos entités **bazaarContent**.

Nous allons profiter de corriger les textes affichés (fonction `T()`):

```js
const config = {
  type: 'bazaarContent',
  kind: 'search',
  title: T('Contenus'),
  hintText: T('...'),
  list: 'bazaarContent',
  hinters: {
    bazaarContent: {
      onValidate: editSelectedEntityQuest('bazaarContent-workitem'),
    },
  },
};
```

Recherche maintenant à l'aide de _ctrl+p_ un fichier `hinter`, c'est celui
(_bazaarContent-hinter.js_) qui permet de définir la configuration du panneau de
recherche et création rapide de nos entités **bazaarContent**.

Nous allons profiter de corriger les textes affichés (fonction `T()`), et aussi
le `mapNewValueTo:` pour la création rapide:

```js
exports.xcraftCommands = function () {
  return buildHinter({
    type: 'bazaarContent',
    fields: ['info'],
    newWorkitem: {
      name: 'bazaarContent-workitem',
      newEntityType: 'bazaarContent',
      description: T('Nouveau contenu'),
      view: 'default',
      icon: 'solid/pencil',
      mapNewValueTo: 'about',
      kind: 'tab',
      isClosable: true,
      navigate: true,
    },
    title: T('Contenus'),
    newButtonTitle: T('Nouveau contenu'),
  });
};
```

## Adapter le lanceur de tâches

Nous devons localiser un fichier `tasks.js`, c'est lui qui permet de définir la
taskbar du contexte de travail. Pour accéder au liste de recherches nous devons
ajouter un lanceur:

```js
  {
    text: T('Contenus'),
    glyph: 'solid/plus',
    workitem: {
      name: 'bazaarContent-search',
      maxInstances: 1,
    },
  },
```

## Tester le tout!

Il suffit maintenant de lancer notre **bazaar** avec la touche _F5_ et
d'attendre que la packager (_webpack_) termine de constuire le nouveau bundle.
Le **desktop** goblin devrais s'afficher avec un nouveau lanceur:

![Launch](/img/bazaar_launch.png?width=600px)

![Task](/img/bazaar_task.png?width=600px)

{{% notice warning %}} **avertissement:** parfois le rechargement automatiques à
chaud (**hot-reload**) des changements fait dans les fichiers UI se bloque, il
suffit de forcer un rechargement du bundle à l'aide du raccourcis _CTRL+R_
{{% /notice %}}
