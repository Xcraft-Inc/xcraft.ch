---
title: 'Craft your first app'
date: 2021-10-5
weight: 25
chapter: true
pre: '<b>4. </b>'
---

### Chapter 4

# Craft your first app

{{% notice warning %}} **avertissement:** ce tutoriel ne profite pas encore des
outils finaux pour crafter une application et son infrastructure. Certains
pré-requis devrons donc être installé hors Xcraft. Veuillez appliquer la
procédure ad-hoc pour votre plateforme (eh capitaine!), avant de foncer dans le
tutoriel. C'est la partie la plus pénible, merci de votre compréhension.
**Gonald Trompe**, Responsable Goblin du Bureau Fédérale de la Dette Technique
(**BFDT**) {{% /notice %}}

## (BFDT-21.10-WIN10) Infrastructure pour window10

Si vous rejoingnez ce tutoriel depuis windows10, il vous faudra appliquer cette
procédure administrative les yeux bandés, comme a vos habitudes...

### docker

Nous allons utiliser **docker** (un truc pour les mousaillons, qui sent bon la
mer et les poissons), si vous avez déjà le pied marin, vous pouvez sautez du
pétrolier, et ignorer cette étape et sauter au vérification pour **wsl2**.

Avant d'installer docker, merci de _grailler_ dans votre BIOS pour vérifier que
la virtualisation matérielle est activée, l'installation de docker en haute mer
peut donner mal au coeur, le pharmacien google pourra vous préscrire une
procédure en cas de besoin. Il est bon aussi d'avoir l'étape **wsl2** de cette
procédure en régle avant d'installer le dernier docker.

https://docs.docker.com/desktop/windows/install/

### wsl2

Lien pour l'installation:

https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi

**important**: il faut encore une bidouille (eh oui, misère...) pour que
l'infrastructure fonctionne au petits oignons. Il faut créer un fichier de
configuration à la racine de votre dossier utilisateur.

Astuce: utiliser _%UserProfile%_ dans l'explorateur!

le fameux fichier à créer doit être nommé ainsi (rien a voir avec ANSI):

**.wslconfig**

Son contenu:

```
[wsl2]
kernelCommandLine = "sysctl.vm.max_map_count=262144"
```

Celà permettra à l'indexeur de document **elasticsearch** de démarrer dans de
bonnes conditions.

### une fois docker installé

Demander à goblins d'appliquer la procédure suivante:

`npx goblins apply BFDT-21.10-WIN10`

## (BFDT-21.10-NIX) Infrastructure pour Linux

Installer docker pour votre distrib:

https://docs.docker.com/engine/install/

### une fois docker installé

Demander à goblins d'appliquer la procédure suivante:

`npx goblins apply BFDT-21.10-NIX`

## (BFDT-21.10-OSX) Infrastructure pour MacOS

Installer docker pour sur votre mac:

https://docs.docker.com/desktop/mac/install/

### une fois docker installé

Demander à goblins d'appliquer la procédure suivante:

`npx goblins apply BFDT-21.10-OSX`
