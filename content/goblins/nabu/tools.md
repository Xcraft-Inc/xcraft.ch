---
title: 'Using Nabu tools'
date: 2020-07-24
weight: 50
draft: true
---

## Using Nabu tools

Au lancement de l'application, choisisez NABUTEST:

![Launch 1](/img/nabu.launch1.png?width=700px)

### Opening a Nabu session

Le footer contient un nouveau bouton **Open a Nabu Session**. Dès qu'il est
cliqué, 4 nouveaux boutons dédiés à Nabu apparaissent:

![Launch 2](/img/nabu.launch2.png)

Le petit drapeau en haut à droite permet de changer de langue:

![Choice](/img/nabu.choice.png)

### The marker

A marked text indicate that the default message is displayed and you must
translate it.

### Open dictionary

Le bouton **Modify all messages** permet d'éditer directement le dictionnaire
des traductions. On voit ici 3 colonnes:

- Keys
- Traductions en français (fr_CH)
- Traductions en anglais (en_US)

![Messages](/img/nabu.messages.png?width=800px)

## Textes de Sam repris tels quels

Now you must translate messages for the de_CH locale. Displayed message are by
default in french because that the language used in the polypheme codebase. When
a translation is not found for the user selected locale, the fallback process
will display the default message.

If a message is not selectable using selection tools, it can be a missed
translation message in codebase. Please report us with an issue in this case.

Some message use `{ }` **ICU** syntax for describing counts, dates, plurals or
other special case. Ask us if you need help for translating these special
messages. In general it's easy to change the text in **ICU** syntax, don't touch
special chars, variables, and focus on editing the textual part.

If some message seems weird or contextually not specific to polypheme, keep it
blank, it can be admin/developer tools or error messages.

When translations is ready for you, contact Epsitec, we create a translation
package and distribute it within the next client app release.

### Opening a Nabu session

When you open a session, a new window is opened for editing translations.

The first window has now tools for marking text and opening single messages in
the opened session.

### Changing the current locale

### The marker

A marked text indicate that the default message is displayed and you must
translate it.

### Selection mode & Edit single message

The selection mode allow you to select messages in the user interface.

This is usefull when you are editing single messages in the nabu session, you
can select message in the first window, it will update the edited message in the
second window. Use this process for translating forms and screens with a better
context.
