---
title: 'Le Bazaar Goblin - Etape 3'
date: 2020-10-06
weight: 20
chapter: true
pre: '<b>5.3 </b>'
---

### Chapter 5.3

# Le Bazaar Goblin - Etape 3

Il est temps d'essayer quelques mécaniques bien huilées et prête à démarrer.
Nous allons simuler le comportement multi-utilisateurs de notre application à
l'aide des lanceurs pré-générés pour notre environnement de développement.

## Portal et Thrall

Une application dite **portal**, permet à un utilisateur de se téléconnecter à
des services distants. Elle contient les services goblins utile pour profiter au
maximum du poste de l'utilisateur:

- gestion des paramètres locaux de la machine (goblin-client)
- gestion des fenêtres (xcraft-core-host et goblin-wm)
- téléchargements et autres gadgets pré-chargé pour afficher le contenu distant

Une application dite **thrall**, à un rôle multi-disciplinaire. Ce n'est pas
pour rien qu'on la compare à un chef de guerre Orc.

C'est le point d'entrée d'accès au système pour les portals voulant se
connecter.

Routage des commandes et événements xcraft, déploiement d'application via le
module **horde**, etc. Tout le code qui dois rester secret et confidentiel
tournera derrière **thrall**.

Celà ressemble fortement à une architecture client/serveur cepandant de subtiles
différences rendent ce concept inadapté et trop humain pour nous autres (Orcs et
Goblins).

## Essais locaux

Depuis l'IDE, il suffit de lancer les trois applications suivantes:

- `bazaar-portal (instance 1)` le portal du premier utilisateur
- `bazaar-portal (instance 2)` le portal du second utilisateur
- `bazaar-thrall` le système d'exploitation de notre application bazaar

Peux-importe l'ordre, les portals attendent que thrall soie prêt et
opérationnel. Une fois que tout est prêt, vous allez découvrir un nouvel écran,
le gestionnaire de sessions utilisateurs (goblin-configurator)

### Ouvrire une session

Si dans un des deux portal vous ouvrez une session (gros boutton **+** propulsé
par un lance rocket goblin), une session de bureau goblin s'ouvre pour le
premier utilisateur (nouvelle fenêtre). De manière réactive, le second portal va
afficher lui aussi la session de notre premier utilisateur dans son gestionnaire
de sessions. Comme vous êtes un coquin, vous pouvez **killer** la session avec
la croix **X** au dessous de celle-ci.

Celà va provoquer la fermeture de la fenêtre ainsi que de la session de notre
premier utilisateur.

Quelques précisions:

Nous n'avons pas configurer de système d'authentification, et le nom
d'utilisateur de votre machine sera utilisé pour identifier partiellement les
sessions de travails ouverte depuis ce poste.

Vous avez tout les droits, **killer** une sessions était donc complétement
gratuit et offert!

### Reprise de sessions

Si vous ouvrez une session (**+**) et que vous fermer la fenêtre, ou
l'application portal, vous ne tuez pas la session. L'état complet du bureau est
conservé coté thrall. Ce qui permet à un utilisateur de reprendre son travail
depuis un autre poste, après un bon week-end, ou après un rédémarrage de la
machine locale.

Vous pouvez vous amusez à ouvrir la même session depuis les deux portal, dans ce
cas, vous pouvez suivre tout les changement d'états qui passe par des services
goblins. Certain modification locales du bureau, comme l'ouverture du moniteur
d'état, ou l'outil de prise de notes, sont stocké dans l'état du composant et ne
bénéficie pas de la réplication d'état via les goblins. Nous ne conseillons pas
de trailler à plusieurs dans la même session, celà peux cependant être utile à
des fins de formations.
