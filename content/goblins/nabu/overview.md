---
title: 'Overview'
date: 2020-07-24
weight: 10
draft: true
---

## Polyglotify your app

Nabu permet de développer une application pour plusieurs langues (ou cultures,
ou encore "locales"). L'application n'est pas compilée pour une langue donnée.
Elle peut changer de langue "à la volée".

Le principe est très simple. Il suffit de remplacer les `string` constantes par
des appels à la fonction `T`:

![Concept](/img/nabu.concept1.png)

L'application contient un dictionnaire de traduction. Dans l'exemple ci-dessous,
le dictionnaire contient les traductions pour la locale `english`, mais il peut
contenir aurant de traductions que souhaité (`french`, `german`, `italian`,
etc.).

![Concept](/img/nabu.concept2.png)

{{% notice info %}} Le développeur peut choisir l'écrire ses textes dans la
langue de son choix. Cela peut être le français, l'anglais, ou même du
"franglais". Peu importe, puisque ces textes pourront toujours être traduits
et/ou corrigés par le dictionnaire Nabu. {{% /notice %}}
