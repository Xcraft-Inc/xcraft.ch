---
title: 'Chest'
date: 2024-05-08
weight: 50
chapter: true
pre: '<b>5. </b>'
---

### Chapitre 5

# Chest

Il est fort et il a du coffre...

Le Chest est un coffre pouvant contenir n'importe quel document. Son API se
limite à ce que tout coffre devrait permettre à un consommateur. On peut y
insérer des documents avec la quête `supply`, récupérer des documents avec la
quête `retrieve` et supprimer des documents avec la quête `trash`.

Dans un monde centralisé, on pourrait s'en contenter car `supply` est en quelque
sorte un "upload" et `retrieve` un "download". Tout ceci paraît vraiment très
simpliste. Pourquoi en parler ici et surtout, pourquoi réinventer quelque chose
d'aussi courant ?

## Local-first

Le Chest est bien plus que ce qui a été mentionné dans le paragraphe précédent.
Le Chest est un Elfe et il s'appuie complètement sur les mécaniques de
synchronisation des Elfes. Il faut avoir compris, avant d'aller plus loin, que
les Elfes peuvent être sérialisables et surtout, synchronisables. Ici, on quitte
le monde centralisé et on bascule dans le monde fabuleux d'Alice aux Pays des
Merveilles. Ou plutôt, on bascule dans le monde du décentralisé.

Oubliez un instant internet. Vous avez le Chest et il est chez vous. Quand vous
utilisez la quête `supply`, vous remplissez votre coffre. Quand vous utilisez la
quête `retrieve`, vous récupérez un stream du document qui est dans votre
coffre. Il existe d'autres quêtes que je n'ai pas présentées tout à l'heure. Ce
sont les quêtes `location` et `locationTry`. Arrêtons-nous un instant sur la
quête `location`. Son implémentation est extrêmement simple. Au lieu de vous
rendre un stream comme `retrieve`, elle vous rend le chemin absolu qui permet
d'atteindre le document. Dit autrement, cette quête n'aurait aucun sens dans un
monde centralisé, car qui souhaite avoir le chemin d'un document côté serveur ?
Mais chez nous, c'est ce qu'on souhaite. Nous sommes au Pays des Merveilles et
tout est à portée de main.

## Mais j'veux pas être tout seul, moi ?!

Je vous comprends, la solitude peut rendre fou. Moi aussi, je souhaite partager
mes documents avec d'autres personnes. Et ces personnes ne sont pas devant mon
ordinateur, mais bel et bien joignables que par le réseau internet. Pas de
problème, comme je l'ai dit, nous sommes distribués (ou plutôt) distribuables.
Pour cela, il nous faut un Chest de référence qui est quelque part sur internet.

Nous avons alors des personnes chez elles, avec leur Chest, ainsi qu'un Chest de
référence. Pour pouvoir utiliser le Chest, vous avez forcément un serveur Xcraft
sous-jacent. Et ce serveur Xcraft peut très facilement se connecter de manière
transparente à un autre serveur Xcraft qui, ici, serait le Chest de référence.
Cette mécanique sous-jacente n'a pas besoin de pouvoir accéder en continu au
serveur distant. Si celui-ci est là, tant mieux, s'il n'est pas là, tant pis.
Vous pouvez continuer à travailler avec votre Chest comme si de rien n'était.

## La distribution des pains, euh.. des documents

Les Elfes peuvent synchroniser leurs états avec un serveur de référence. Dans le
cas du Chest, il y a des Elfes qui se nomment les `chestObject`. Un
`chestObject` est une entité qui contient des informations sur le document
stocké dans le coffre. L'ensemble de ces entités représente l'index de tout ce
que peut contenir un coffre. Alors oui, j'ai dit **peut contenir** et pas juste
**contient**. C'est là que la magie va opérer.

### Remplir le coffre depuis chez soi

Quand on remplit le coffre, `supply`, on stocke simplement un fichier dans le
storage local du coffre avec une entité `chestObject` qui le décrit. Si un
serveur Xcraft de référence est accessible, la mécanique générique de
synchronisation va envoyer l'entité (et pas le document) au serveur distant. Le
serveur distant va effectuer quelques vérifications suite à l'ajout de cette
entité. Il va regarder s'il connaît le document référencé dans son stockage
local.

> Le document référencé est nommé avec le sha256 de son propre contenu.

Si le serveur connaît déjà ce document, il ne se passe rien de plus. Mais si le
serveur ne le connaît pas, il va alors émettre un événement en broadcast où il
demande à tous les clients connectés sur lui, si l'un d'eux a ce document chez
lui. Le premier client qui répond à cette demande, va alors effectuer un
`supply` (donc de local chest → remote chest) afin de satisfaire le serveur. Si
aucun client actuellement connecté ne connaît ce document, ce n'est pas trop
grave. Le Chest de référence va redemander périodiquement à tout le monde
jusqu'à ce qu'un client finisse par le lui envoyer.

### Récupérer un document depuis chez soi

Je vous ai présenté les quêtes tout à l'heure et j'ai parlé de la quête
`retrieve`. Mais est-ce que vous devriez l'utiliser ? En principe non. Même si
elle est publique, finalement elle n'est pas adaptée à un modèle distribué. Il
faut comprendre qu'elle va vous retourner un stream uniquement si le document
existe dans votre coffre. Dans le cas contraire, c'est une exception que vous
allez recevoir.

Oh mince alors, que faire ? Facile, la magie va opérer avec la quête
`locationTry`. Étant donné que votre coffre fonctionne localement, il est tout à
fait censé de travailler avec des chemins absolus pour récupérer des documents.
Vous pouvez alors facilement copier ce document ou créer un stream en lecture
sur celui-ci avec les API de système de fichiers tout à fait habituels. Mais que
se passe-t-il quand un document existe en tant que `chestObject` mais pas
physiquement sur votre système ?

C'est ici qu'intervient `locationTry`. Si le document existe déjà chez vous,
vous allez immédiatement recevoir le chemin absolu sur celui-ci. Mais si le
document n'existe pas et que le serveur de référence est là, il va y avoir (par
derrière) une récupération avec `retrieve` pour vous fournir en fin de compte le
chemin absolu.

### En résumé

Les entités `chestObject` sont synchronisées dès que possible et les documents
en tant que tels sont récupérés sur demande.

D'accord, mais ça va gonfler avec le temps... Alors non, le stockage local est
un cache roulant paramétrable. Il va stocker au maximum 2 GB (par exemple) de
documents et quand la limite est atteinte, les plus anciens (demandés il y a le
plus longtemps) sont supprimés.

### Et comment fait le serveur

Les scénarios précédents expliquent le mécanisme depuis les clients. Pour le
serveur de référence, c'est différent. Ici, il n'y a pas de cache roulant. Le
serveur de référence essaie tout le temps d'avoir tous les documents existants
en tant qu'entités `chestObject`. Tant qu'une entité n'est pas détruite, et que
le document n'existe pas dans son stockage, il va le demander, et le redemander,
... C'est le principe de l'upload expliqué précédemment.

Maintenant, il est possible que du côté de ce serveur, il y ait une quête qui va
réclamer un document et que ce document n'existe peut-être pas encore dans
l'index du serveur ou pas encore dans le stockage. Si c'est le cas, au moment du
`locationTry`, le serveur va demander avec plus d'insistance qu'on lui donne ce
document avec un timeout afin de ne pas bloquer l'appel pour toujours.
