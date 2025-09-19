---
title: 'Le Bazaar Goblin - Etape 1'
weight: 10
chapter: true
pre: '<b>6.1 </b>'
---

Hoy! Le cartel nous demande de coder une application pour partager les
connaissances entres les différents postes de travail de la confédération.

Un simple gestionnaire de contenu, avec des articles des étiquettes.

Nous allons utiliser la commande **goblins** afin de préparer une application
minimale et fonctionnelle.

## Au boulot!

Nous allons travailler depuis un shell de type _bash_. (_powershell_ fera
l'affaire sous windows)

Les premières commandes pour lancer la production de notre application sont les
suivantes:

`mkdir bazaar-dev`

`cd bazaar-dev`

Nous allons ensuite utiliser _npx_ pour executer l'outils **goblins** et
initialiser notre application bazaar:

`npx goblins init bazaar`

![Console](/img/bazaar_init.png?width=600px)

Arme toi de patience, car la commande **goblins init** s'occupe de tout!

## Retour au Bureau Fédéral de la Dette Technique

{{% notice warning %}} Vous devez avoir remplis les pré-requis pour votre
plateforme en suivant la procédure BFDT-21.10 avant de poursuivre.
{{% /notice %}}

Si tout est en ordre avec docker, vous pouvez _appliquer_ la procédure à l'aide
de cette commande:

`npx goblins apply BFDT-21.10-*`

elle va vérifier ton installation docker, préparer une composition et essayer de
monter tout celà dans docker. Un contrôle visuel dans docker permet de verifier
si tout c'est bien passé:

![Docker](/img/bazaar_docker.png?width=600px)

## Moteur!

Avec un peu de chance, on arrive à démarrer notre application en un coup de
tournevis:

`npm start`

La premier allumage est lent, mais il devrait déboucher sur un environnement
_goblin-desktop_ tout vide et prêt à acceuillir notre code métier!

![Start](/img/bazaar_start.png?width=600px)
