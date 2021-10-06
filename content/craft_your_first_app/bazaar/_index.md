---
title: 'Le Bazaar Goblin'
date: 2020-08-05
weight: 80
chapter: true
pre: '<b>4.1 </b>'
---

### Chapter 4.1

# Le Bazaar Goblin

Hoy! Le cartel nous demande de coder une application pour gérer sa boutique de
gadgets. Nous allons utiliser la commande **goblins** afin de préparer une
application minimale et fonctionnelle.

## Au boulot!

Nous allons travailler depuis un shell de type _bash_. (_powershell_ fera
l'affaire sous windows)

Les première commande à lancer la production de notre application sont les
suivantes:

`mkdir bazaar-dev`

`cd bazaar-dev`

Nous allons ensuite utiliser _npx_ pour executer l'outils goblins et initialiser
notre application bazaar:

`npx goblins init bazaar`

![Console](/img/bazaar_init.png?width=600px)

Arme toi de patience, car la commande **goblins init** s'occupe de tout!

## Retour au Bureau Fédéral de la Dette Technique

{{% notice warning %}} Vous devez avoir remplis les pré-requis pour votre
plateforme en suivant la procédure BFDT-21.10 avant de poursuivre. **Gonald
Trompe**, Responsable Goblin du Bureau Fédéral de la Dette Technique (**BFDT**)
{{% /notice %}}

Si tout est en ordre avec docker, vous pouvez _appliquer_ la procédure à l'aide
de cette commande:

`npx goblins apply BFDT-21.10-*`

elle va vérifier ton installation docker, préparer une composition et essayer de
monter tout celà dans docker. Un contrôle visuel dans docker permet de verifier
si tout c'est bien passé:

![Docker](/img/bazaar_docker.png?width=600px)
