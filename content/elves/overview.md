---
title: "Qui sont'ils"
date: 2022-12-09
weight: 5
tags: ['devel', 'goblin', 'elf']
pre: '<b>3.1 </b>'
---

Les Elfes sont une couche d'abstraction pour les Goblins. Ecrire un Elfe n'est pas écrire un nouveau type de service Xcraft. Cela revient à écrire un Goblin avec les avantages de profiter de l'auto-complétion, d'une écriture orientée objet et surtout de se concentrer avant tout sur le métier plutôt que sur les mécaniques des Goblins.

## Les Elfes sont classes

L'écriture devient plus naturelle et plus concise. Il est alors possible d'exploiter les mécanismes d'aide à la complétion pour suivre et comprendre les API. L'aide à la complétion fonctionne uniquement entre Elfes et il est possible que le support des APIs ne soit pas de 100% dans certains cas particuliers.

Il est important de comprendre qu'une classe elfique n'est pas une classe javascript comme les autres. Ce type de classe abstrait la création de services et les communications à tel point qu'il suffit de réaliser un `await` sur une quête comme par exemple `await this.myQuest()` comme si on faisait un appel directement sur une méthode d'instance d'une classe. Bien entendu dans le cas d'une classe elfique, un appel de ce genre provoque la génération d'une commande sur le bus Xcraft et en aucun cas un appel direct sur la méthode en question.

## Les Elfes sont immuables

Les Goblins exploitent redux pour la gestion du state. Les Elfes font de même à une petite différence prêt. Les Elfes ont bien toujours des reducers associés à leurs quêtes respectivent, néanmoins le state reçu, ressemble bien à un state immutable mais celui-ci est paramétré comme étant mutable. Il suffit d'utiliser la méthode `.set()` de `Shredder` tout à fait comme d'habitude, par contre il est inutile de récupérer le résultat en retour, en effet, ce state est mutable. Et donc ne retournez pas le nouveau state avec le reducer, c'est complètement inutile.

Il est possible que vous devez effectuer des comparaisons avec le state initial (entrant dans le reducer). Pas de problème car tous les reducers elfiques reçoivent aussi le state immutable en 3ème arguments.

## Les états des Elfes ne connaissent pas de schémas

La problématique est en cours d'étude.
