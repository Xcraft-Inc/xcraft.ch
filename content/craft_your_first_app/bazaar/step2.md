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

Nous allons commencer par représenter les différents bidules qu'on vend dans
notre bazaar. Le plus simple c'est de demander à notre commande **goblins** de
**crafter** une **entité bazaarItem**:

`npx goblins craft entity bazaarItem`

Une entité **bazaarItem** est maintenant configurable dans
`/bazaar-dev/lib/goblin-bazaar/entities/bazaarItem.js`
