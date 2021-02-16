---
title: 'Using Nabu tools'
date: 2020-07-24
weight: 50
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

{{% notice warning %}} Ce mode ne fonctionne actuellement plus (04.09.20). Il
faudra le réparer puis le documenter! {{% /notice %}}

### Open dictionary

Le bouton **Modify all messages** permet d'éditer directement le dictionnaire
des traductions. On voit ici 3 colonnes:

- Keys
- Traductions en français (fr_CH)
- Traductions en anglais (en_US)

![Messages](/img/nabu.messages.png?width=800px)

{{% notice note %}} On voit que la plupart des textes anglais n'ont pas encore
été traduits. Cela n'est pas un problème, ces textes seront remplacés
provisoirement par la `key`, donc ici par des textes en français (_fallback_).
{{% /notice %}}

Dans cet exemple, le développeur a choisi de coder ses `keys` en français. S'il
n'a pas fait de fautes d'orthographes, il n'est pas nécessaire de traduire le
texte (_fallback_). Mais en cas de besoin, cela est toujours possible, comme
dans l'exemple de la `key = Bonjour` ci dessous:

![Dico](/img/nabu.dico.png?width=500px)
