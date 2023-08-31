---
title: Présentation du Shell
teaching: 5
exercises: 0
---

::::::::::::::::::::::::::::::::::::::: objectifs

- Expliquer comment le shell est lié au clavier, à l'écran, au système d'exploitation et aux programmes des utilisateurs.
- Expliquer quand et pourquoi les interfaces en ligne de commande doivent être utilisées plutôt que les interfaces graphiques.

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- Qu'est-ce qu'un shell et pourquoi devrais-je en utiliser un ?

::::::::::::::::::::::::::::::::::::::::::::::::::

### Contexte

Les humains et les ordinateurs interagissent couramment de différentes manières, que ce soit par le clavier et la souris, les interfaces tactiles ou en utilisant des systèmes de reconnaissance vocale.
Le moyen le plus couramment utilisé pour interagir avec les ordinateurs personnels est appelé une **interface graphique** (GUI).
Avec une GUI, nous donnons des instructions en cliquant avec la souris et en utilisant des interactions basées sur des menus.

Bien que le support visuel d'une GUI facilite l'apprentissage, ce moyen de transmettre des instructions à un ordinateur ne s'adapte pas bien à grande échelle.
Imaginez la tâche suivante :
pour une recherche littéraire, vous devez copier la troisième ligne de mille fichiers texte dans mille répertoires différents et la coller dans un seul fichier.
En utilisant une GUI, vous cliqueriez non seulement sur votre bureau pendant plusieurs heures, mais vous pourriez également commettre une erreur lors de l'exécution de cette tâche répétitive.
C'est là que nous tirons parti du shell Unix.
Le shell Unix est à la fois une **interface en ligne de commande** (CLI) et un langage de script,
permettant d'automatiser et d'accélérer les tâches répétitives.
Avec les commandes appropriées, le shell peut répéter les tâches avec ou sans certaines modifications
autant de fois que nous le souhaitons.
Avec le shell, la tâche de l'exemple littéraire peut être accomplie en quelques secondes.

### Le Shell

Le shell est un programme dans lequel les utilisateurs peuvent taper des commandes.
Grâce au shell, il est possible d'exécuter des programmes complexes comme des logiciels de modélisation du climat
ou des commandes simples permettant de créer un répertoire vide en une seule ligne de code.
Le shell Unix le plus populaire est Bash (le Bourne Again SHell -
ainsi appelé parce qu'il est dérivé d'un shell écrit par Stephen Bourne).
Bash est le shell par défaut dans la plupart des implémentations modernes d'Unix et dans la plupart des packages fournissant des outils de type Unix pour Windows.
Notez que "Git Bash" est un logiciel qui permet aux utilisateurs de Windows d'utiliser une interface de type Bash
lorsqu'ils interagissent avec Git.

Utiliser le shell demande un certain effort et du temps pour être appris.
Alors qu'une GUI vous présente des choix à sélectionner, les choix de CLI ne vous sont pas automatiquement présentés,
vous devez donc apprendre quelques commandes comme du nouveau vocabulaire dans une langue que vous étudiez.
Cependant, contrairement à une langue parlée, un petit nombre de "mots" (c'est-à-dire des commandes) vous permet de faire beaucoup de choses,
et nous aborderons aujourd'hui ces quelques commandes essentielles.

La grammaire d'un shell vous permet de combiner des outils existants en puissants pipelines
et de traiter automatiquement de grandes quantités de données. Les séquences de
commandes peuvent être écrites dans un *script*, améliorant ainsi la reproductibilité
des flux de travail.

De plus, la ligne de commande est souvent le moyen le plus simple d'interagir avec des machines distantes
et des supercalculateurs.
La familiarité avec le shell est pratiquement essentielle pour exécuter une variété d'outils et de ressources spécialisés,
y compris les systèmes informatiques à haute performance.
À mesure que les clusters et les systèmes informatiques basés sur le cloud deviennent plus populaires pour le traitement des données scientifiques,
savoir interagir avec le shell devient une compétence nécessaire.
Nous pouvons développer les compétences en ligne de commande abordées ici
pour résoudre un large éventail de questions scientifiques et de défis informatiques.

Commençons.

Lorsque le shell est ouvert pour la première fois, vous voyez un **invite**,
indiquant que le shell attend une saisie.

```bash
$
```

Le shell utilise généralement `$ ` comme invite, mais il peut utiliser un symbole différent.
Dans les exemples de cette leçon, nous montrerons l'invite comme `$ `.
Surtout, *ne tapez pas l'invite* lorsque vous entrez des commandes.
Tapez uniquement la commande qui suit l'invite.
Cette règle s'applique tant dans ces leçons que dans d'autres sources.
Notez également qu'après avoir tapé une commande, vous devez appuyer sur la touche <kbd>Enter</kbd> pour l'exécuter.

L'invite est suivie d'un **curseur de texte**, un caractère qui indique l'endroit où votre
frappe apparaîtra.
Le curseur est généralement un bloc clignotant ou solide, mais il peut aussi être un trait de soulignement ou un tube.
Vous avez peut-être vu cela dans un programme d'édition de texte, par exemple.

Notez que votre invite peut avoir l'air un peu différente. En particulier, la plupart des environnements de shell populaires
affichent par défaut votre nom d'utilisateur et le nom de l'hôte avant le `$`. Un tel
invite pourrait ressembler à ceci, par exemple :

```bash
nelle@localhost $
```

L'invite pourrait même inclure plus de détails que cela. Ne vous inquiétez pas si votre invite n'est pas
juste un simple `$ `. Cette leçon ne dépend pas de ces informations supplémentaires et elles
ne devraient pas vous gêner. Le seul élément important sur lequel se concentrer est le caractère `$ `
et nous verrons plus tard pourquoi.

Voyons donc notre première commande, `ls`, qui est l'abréviation de "listing" (liste).
Cette commande affiche le contenu du répertoire courant :

```bash
$ ls
```

```output
Desktop     Downloads   Movies      Pictures
Documents   Library     Music       Public
```

:::::::::::::::::::::::::::::::::::::::::  callout

## Commande introuvable

Si le shell ne peut pas trouver un programme dont le nom est celui de la commande que vous avez tapée,
il affiche un message d'erreur comme :

```bash
$ ks
```

```output
ks: command not found
```

Cela peut se produire si la commande a été mal tapée ou si le programme correspondant à cette commande
n'est pas installé.


::::::::::::::::::::::::::::::::::::::::::::::::::

## Le pipeline de Nelle : un problème typique

Nelle Nemo, une biologiste marine,
vient de rentrer d'une mission de six mois dans le
[Gyre du Pacifique Nord](https://en.wikipedia.org/wiki/North_Pacific_Gyre)
où elle a prélevé des échantillons de vie marine gélatineuse dans le
[Great Pacific Garbage Patch](https://en.wikipedia.org/wiki/Great_Pacific_Garbage_Patch).
Elle a 1520 échantillons qu'elle a analysés à l'aide d'une machine à mesurer l'abondance relative
de 300 protéines.
Elle doit faire passer ces 1520 fichiers dans un programme imaginaire appelé `goostats.sh`.
En plus de cette énorme tâche, elle doit rédiger les résultats avant la fin du mois pour que son article
puisse être publié dans un numéro spécial des *Aquatic Goo Letters*.

Si Nelle choisit d'exécuter `goostats.sh` manuellement à l'aide d'une GUI,
elle doit sélectionner et ouvrir un fichier 1520 fois.
Si `goostats.sh` met 30 secondes à s'exécuter pour chaque fichier, le processus complet prendra plus de 12 heures
de l'attention de Nelle.
Grâce au shell, Nelle peut confier cette tâche fastidieuse à son ordinateur tandis qu'elle se concentre
sur la rédaction de son article.

Les prochaines leçons exploreront les moyens pour que Nelle puisse y parvenir.
Plus précisément,
les leçons expliquent comment elle peut utiliser un shell pour exécuter le programme `goostats.sh`,
en utilisant des boucles pour automatiser les étapes répétitives de saisie des noms de fichiers,
afin que son ordinateur puisse travailler pendant qu'elle rédige son article.

En bonus,
une fois qu'elle aura mis en place un pipeline de traitement,
elle pourra l'utiliser à nouveau chaque fois qu'elle collectera de nouvelles données.

Pour réaliser sa tâche, Nelle a besoin de savoir comment :

- naviguer vers un fichier/répertoire
- créer un fichier/répertoire
- vérifier la longueur d'un fichier
- enchaîner des commandes
- récupérer un ensemble de fichiers
- itérer sur des fichiers
- exécuter un script shell contenant son pipeline



:::::::::::::::::::::::::::::::::::::::: points-clés

- Un shell est un programme dont le but principal est de lire des commandes et d'exécuter d'autres programmes.
- Cette leçon utilise Bash, le shell par défaut dans de nombreuses implémentations d'Unix.
- Les programmes peuvent être exécutés dans Bash en saisissant des commandes à l'invite de commande.
- Les principaux avantages du shell sont son rapport élevé entre l'action et le nombre de frappes, son support pour l'automatisation des tâches répétitives et sa capacité à accéder aux machines en réseau.
- Un défi important lors de l'utilisation du shell peut consister à savoir quelles commandes doivent être exécutées et comment les exécuter.

::::::::::::::::::::::::::::::::::::::::::::::::::