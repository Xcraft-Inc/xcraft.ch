---
title: 'Démarrage'
weight: 40
tags: ['carnotzet', 'bootstrap']
pre: '<b>5.4 </b>'
---

Que vous ayez modifié des [paramètres](/carnotzet/03.settings.md) ou non, le
premier démarrage de **Carnotzet OS** est particulier. Vous avez transféré
l'image sur votre carte µSD, et cette image ne couvre pas tout l'espace
disponible de votre carte. Vous n'avez pas eu besoin de créer les partitions à
l'avance. C'est pour cette raison que le tout premier démarrage est différent
des suivants. Lors de cette phase, **Carnotzet OS** va automatiquement démarrer
en mode "rescue". Des manipulations vont être effectuées automatiquement pour
modifier la table de partition et agrandir la partition du système (ext4) afin
qu'elle utilise tout l'espace disponible. Une fois fait, le système va
automatiquement redémarrer afin de quitter le mode "rescue" et rendre votre
installation opérationnelle.

Quand tout est nominal (ce qui devrait être le cas si vous avez suivi les
instructions à la lettre) tout devrait se passer sans la moindre intervention de
votre part.

## Interface

Pour commencer, veuillez brancher un écran et un clavier à votre matériel. Il
est possible également pour les spécialistes, d'utiliser une interface UART.
Pour des raisons de sécurité, il n'y a aucun serveur SSH (ou équivalent)
installé par défaut, les comptes utilisateurs n'étant pas encore protégés.

## Les comptes utilisateur

Il y a deux comptes utilisateur disponibles. Le premier est bien entendu le
compte `root` et le second est le compte `rasp` qui a des droits limités. Il est
primordial de modifier les mots de passe de ces deux comptes avant d'exploiter
votre installation. Connectez-vous avec le compte `root` en utilisant le mot de
passe `root` (c'est le même principe avec le compte `rasp`). Effectuez alors les
commandes suivantes afin de modifier les mots de passe des deux comptes
susmentionnés :

```
~ # passwd root
Enter new UNIX password: *********
Retype new UNIX password: *********

~ # passwd rasp
Enter new UNIX password: *********
Retype new UNIX password: *********
```

Ne négligez pas cette étape. Choisissez des mots de passe différents entre les
deux comptes. Aidez-vous d'une phrase, il n'est pas interdit d'utiliser des
espaces et de la ponctuation dans les mots de passe.

## Ensuite

L'essentiel est là, vous pouvez passer à la section
[Services](/carnotzet/05.services.md) pour en savoir plus sur **Carnotzet OS**.
