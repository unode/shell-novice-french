---
title: Naviguer dans les fichiers et les répertoires
teaching: 30
exercises: 10
---

::::::::::::::::::::::::::::::::::::::: objectifs

- Expliquer les similitudes et les différences entre un fichier et un répertoire.
- Traduire un chemin absolu en un chemin relatif et vice versa.
- Construire des chemins absolus et relatifs qui identifient des fichiers et des répertoires spécifiques.
- Utiliser des options et des arguments pour modifier le comportement d'une commande shell.
- Démontrer l'utilisation de la complétion automatique et expliquer ses avantages.

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- Comment puis-je me déplacer sur mon ordinateur ?
- Comment puis-je voir quels fichiers et répertoires j'ai ?
- Comment puis-je spécifier l'emplacement d'un fichier ou d'un répertoire sur mon ordinateur ?

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: instructeur

Introduire et naviguer dans le système de fichiers dans le shell
(covered in [Navigating Files and Directories](02-filedir.md) section)
can be confusing. You may have both terminal and GUI file explorer
open side by side so learners can see the content and file
structure while they're using terminal to navigate the system.

::::::::::::::::::::::::::::::::::::::::::::::::::

La partie du système d'exploitation chargée de gérer les fichiers et les répertoires
est appelée le **système de fichiers**.
Il organise nos données en fichiers,
qui contiennent des informations,
et en répertoires (également appelés "dossiers"),
qui contiennent des fichiers ou d'autres répertoires.

Plusieurs commandes sont fréquemment utilisées pour créer, inspecter, renommer et supprimer des fichiers et des répertoires.
Pour commencer à les explorer, nous allons ouvrir une fenêtre de terminal.

Tout d'abord, découvrons où nous sommes en exécutant la commande `pwd`
(qui signifie 'répertoire de travail actuel'). Les répertoires sont comme des *emplacements* - à tout moment, 
lorsque nous utilisons le shell, nous sommes exactement à un emplacement appelé 
notre **répertoire de travail actuel**. Les commandes lisent et écrivent principalement dans les fichiers du 
répertoire de travail actuel, c'est-à-dire ici, donc il est important de savoir où vous vous trouvez avant d'exécuter une commande. 
`pwd` vous montre où vous êtes :

```bash
$ pwd
```

```output
/Users/nelle
```

Ici,
la réponse de l'ordinateur est `/Users/nelle`,
qui est le **répertoire personnel** de Nelle :

:::::::::::::::::::::::::::::::::::::::::  callout

## Variations de répertoire personnel

Le chemin du répertoire personnel peut être différent selon les systèmes d'exploitation.
Sous Linux, il peut ressembler à `/home/nelle`,
et sous Windows, il sera similaire à `C:\Documents and Settings\nelle` ou
`C:\Users\nelle`.
(Notez qu'il peut être légèrement différent selon les versions de Windows.)
Dans les exemples ultérieurs, nous avons utilisé la sortie Mac par défaut - Linux et Windows
la sortie peut être légèrement différente, mais devrait être généralement similaire.

Nous supposerons également que votre commande `pwd` renvoie le répertoire personnel de votre utilisateur.
Si `pwd` renvoie autre chose, vous devrez y accéder en utilisant `cd`
ou certaines commandes de cette leçon ne fonctionneront pas comme écrit.
Consultez [Exploring Other Directories](#exploring-other-directories) pour plus de détails
sur la commande `cd`.


::::::::::::::::::::::::::::::::::::::::::::::::::

Pour comprendre ce qu'est un 'répertoire personnel',
jetons un coup d'œil à la façon dont le système de fichiers est organisé dans son ensemble. À des fins
de cet exemple, nous allons
illustrer le système de fichiers sur l'ordinateur de notre scientifique Nelle. Après cette
illustration, vous allez apprendre les commandes pour explorer votre propre système de fichiers,
qui sera construit de manière similaire, mais pas exactement identique.

Sur l'ordinateur de Nelle, le système de fichiers ressemble à ceci :

![](fig/filesystem.svg){alt='Le système de fichiers est composé d'un répertoire racine qui contient des sous-répertoires appelés bin, data, users et tmp'}

Le système de fichiers ressemble à un arbre renversé. 
Le répertoire le plus haut placé est le **répertoire racine**
qui contient tout le reste.
Nous l'appelons en utilisant un caractère slash, `/`, tout seul ;
ce caractère est le slash initial dans `/Users/nelle`.

À l'intérieur de ce répertoire se trouvent plusieurs autres répertoires :
`bin` (qui est l'endroit où certaines programmes intégrés sont stockés),
`data` (pour les fichiers de données divers),
`Users` (où se trouvent les répertoires personnels des utilisateurs),
`tmp` (pour les fichiers temporaires qui n'ont pas besoin d'être stockés à long terme),
et ainsi de suite.

Nous savons que notre répertoire de travail actuel `/Users/nelle` est stocké à l'intérieur de `/Users`
parce que `/Users` est la première partie de son nom.
De même,
nous savons que `/Users` est stocké à l'intérieur du répertoire racine `/`
parce que son nom commence par `/`.

:::::::::::::::::::::::::::::::::::::::::  callout

## Slashes

Remarquez qu'il y a deux significations pour le caractère `/`.
Lorsqu'il apparaît au début d'un nom de fichier ou de répertoire,
il fait référence au répertoire racine. Lorsqu'il apparaît *à l'intérieur* d'un chemin,
ce n'est qu'un séparateur.


::::::::::::::::::::::::::::::::::::::::::::::::::

Sous `/Users`,
nous trouvons un répertoire pour chaque utilisateur ayant un compte sur la machine de Nelle,
ses collègues, *imhotep* et *larry*.

![](fig/home-directories.svg){alt='Comme les autres répertoires, les répertoires personnels sont des sous-répertoires situés sous "/Users" tels que "/Users/imhotep", "/Users/larry" ou "/Users/nelle"'}

Les fichiers de l'utilisateur *imhotep* sont stockés dans `/Users/imhotep`,
ceux de l'utilisateur *larry* dans `/Users/larry`,
et ceux de Nelle dans `/Users/nelle`. Nelle est l'utilisateur dans nos exemples ici ; par conséquent, nous obtenons `/Users/nelle`
comme répertoire personnel. Normalement, lorsque vous ouvrez une nouvelle invite de commandes, vous vous trouverez dans
votre répertoire personnel pour commencer.

Maintenant, apprenons la commande qui nous permettra de voir le contenu de notre
propre système de fichiers. Nous pouvons voir ce qu'il y a dans notre répertoire personnel en exécutant `ls` :

```bash
$ ls
```

```output
Applications Documents    Library      Music        Public
Desktop      Downloads    Movies       Pictures
```

(Une fois de plus, les résultats peuvent être légèrement différents en fonction de votre système d'exploitation
et de la façon dont vous avez personnalisé votre système de fichiers.)

`ls` affiche les noms des fichiers et des répertoires dans le répertoire actuel.
Nous pouvons rendre sa sortie plus compréhensible en utilisant l'**option** `-F`
qui indique à `ls` de classer la sortie
en ajoutant un indicateur aux noms des fichiers et des répertoires pour indiquer ce qu'ils sont :

- un `/` à la fin indique qu'il s'agit d'un répertoire
- `@` indique un lien
- `*` indique un exécutable

En fonction des paramètres par défaut de votre shell, il peut également utiliser des couleurs pour indiquer si chaque entrée est un fichier ou un répertoire.

```bash
$ ls -F
```

```output
Applications/ Documents/    Library/      Music/        Public/
Desktop/      Downloads/    Movies/       Pictures/
```

Ici,
nous pouvons voir que le répertoire personnel ne contient que des **sous-répertoires**.
Tous les noms dans la sortie qui n'ont pas de symbole de classification
sont des **fichiers** dans le répertoire de travail actuel.

:::::::::::::::::::::::::::::::::::::::::  callout

## Effacer votre terminal

Si votre écran est trop encombré, vous pouvez effacer votre terminal en utilisant la
commande `clear`. Vous pouvez toujours accéder aux commandes précédentes en utilisant <kbd>↑</kbd>
et <kbd>↓</kbd> pour vous déplacer ligne par ligne, ou en faisant défiler vers le haut ou le bas dans votre terminal.


::::::::::::::::::::::::::::::::::::::::::::::::::

### Obtenir de l'aide

`ls` dispose de nombreuses autres **options**. Il existe deux moyens courants de savoir comment
utiliser une commande et quelles options elle accepte :
**en fonction de votre environnement, vous constaterez peut-être que seule l'une de ces
méthodes fonctionne :**

1. Nous pouvons passer une option `--help` à n'importe quelle commande (disponible sur Linux et Git Bash),
   par exemple :

   ```bash
   $ ls --help
   ```

2. Nous pouvons lire son manuel avec `man` (disponible sur Linux et macOS) :

   ```bash
   $ man ls
   ```

Nous décrirons les deux méthodes ensuite.

:::::::::::::::::::::::::::::::::::::::::  callout

## Aide pour les commandes intégrées

Certaines commandes sont intégrées au shell Bash, plutôt que d'exister comme des programmes distincts
sur le système de fichiers. Un exemple est la commande `cd` (changer de répertoire).
Si vous obtenez un message tel que `"Aucune entrée manuelle pour cd"`,
essayez plutôt `"help cd"`. La commande `help` est utilisée pour obtenir des informations d'utilisation pour
[les commandes intégrées Bash](https://www.gnu.org/software/bash/manual/html_node/Bash-Builtins.html).


::::::::::::::::::::::::::::::::::::::::::::::::::

#### L'option `--help`

La plupart des commandes bash et des programmes qui peuvent être exécutés dans le cadre de bash, prennent en charge une option `--help` qui affiche plus d'informations sur l'utilisation de la commande ou du programme.

```bash
$ ls --help
```

```output
Utilisation: ls [OPTION]... [FICHIER]...
Afficher les fichiers (le répertoire courant par défaut).
Trier les entrées par ordre alphabétique si ni -cftuvSUX ni --sort n'est spécifié.

Les arguments obligatoires pour les options longues le sont aussi pour les options courtes.
  -a, --all                  N'ignorer pas les entrées commençant par .
  -A, --almost-all           Ne pas lister . et .. implicites
      --author               Avec -l, afficher les auteurs des fichiers
  -b, --escape               Afficher les caractères non graphiques sous forme échappée C
      --block-size=TAILLE    mettre des tailles à TAILLE avant de les imprimer, par ex.
                               '--block-size=M' donne les tailles en unités
                               de 1 048 576 octets ; voir les formats TAILLE PLUS bas
  -B, --ignore-backups       Ne pas lister les fichiers se terminant par ~ implicites
  -c                         Trie selon l'heure de dernière modification de l'état
                               d'information du fichier (-lt) ou selon l'heure de dernière
                               modification du fichier (-l) ou encore
                               suivant l'heure de dernière modification, par défaut la plus récente.
  -C                         Liste par colonne
      --color[=QUAND]        Coloriser la sortie ; QUAND peut valoir 'toujours' (valeur par défaut
                               si omis), 'auto' ou 'jamais' ; Plus d'informations ci-dessous
  -d, --directory            Afficher les répertoires eux-mêmes, pas leur contenu
  -D, --dired                Générer une sortie adaptée au mode dired d'Emacs
  -f                         Ne pas trier, activer -aUF, désactiver -ls --color
  -F, --classify             Appender un indicateur (un des *suivant=>@|) aux entrées
...      ...        ...
```

:::::::::::::::::::::::::::::::::::::::::  callout

## Options de ligne de commande non prises en charge

Si vous essayez d'utiliser une option qui n'est pas prise en charge, `ls` et d'autres commandes
afficheront généralement un message d'erreur similaire à :

```bash
$ ls -j
```

```error
ls: illegal option -- 'j'
Try 'ls --help' for more information.
```

::::::::::::::::::::::::::::::::::::::::::::::::::

#### La commande `man`

L'autre moyen d'en savoir plus sur `ls` consiste à taper :

```bash
$ man ls
```

Cette commande transformera votre terminal en une page contenant une description
de la commande `ls` et de ses options.

Pour naviguer dans les manuels `man`,
vous pouvez utiliser <kbd>↑</kbd> et <kbd>↓</kbd> pour vous déplacer ligne par ligne,
ou essayez <kbd>B</kbd> et <kbd>Espace</kbd> pour sauter vers le haut et vers le bas d'une page entière.
Pour rechercher un caractère ou un mot dans les pages `man`,
utilisez <kbd>/</kbd> suivi du caractère ou du mot que vous recherchez.
Parfois, une recherche donnera lieu à plusieurs résultats.
Si c'est le cas, vous pouvez vous déplacer entre les résultats en utilisant <kbd>N</kbd> (pour l'avancement) et
<kbd>Maj</kbd>\+<kbd>N</kbd> (pour le recul).

Pour **quitter** les pages `man`, appuyez sur <kbd>Q</kbd>.

:::::::::::::::::::::::::::::::::::::::::  callout

## Pages de manuel sur le web

Bien sûr, il existe un troisième moyen d'accéder à l'aide pour les commandes :
recherchez sur Internet à l'aide de votre navigateur web.
Lorsque vous effectuez une recherche sur Internet, inclure la phrase `unix man page` dans votre recherche
vous aidera à trouver des résultats pertinents.

GNU fournit des liens vers ses
[manuel](https://www.gnu.org/manual/manual.html), y compris le
[core des utilitaires GNU](https://www.gnu.org/software/coreutils/manual/coreutils.html),
qui couvre de nombreuses commandes introduites dans cette leçon.


::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## Explorer davantage les options `ls`

Vous pouvez également utiliser plusieurs options en même temps. Que fait la commande `ls`
lorsqu'elle est utilisée avec l'option `-l` ? Et qu'en est-il si vous utilisez à la fois les options `-l` et `-h` ?

Certaines de ses sorties sont axées sur des propriétés que nous ne couvrons pas dans cette leçon (telles que 
les autorisations de fichier et la propriété), mais le reste devrait tout de même être utile.

:::::::::::::::  solution

## Solution

L'option `-l` fait en sorte que `ls` utilise un format de liste **l**ongue, affichant non seulement
les noms des fichiers/répertoires, mais aussi des informations supplémentaires, telles que la taille du fichier
et l'heure de sa dernière modification. Si vous utilisez à la fois l'option `-h` et l'option `-l`, cela rend la taille du fichier '**h**uman readable', c'est-à-dire affiche quelque chose comme `5.3K` au lieu de `5369`.



:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## Lister dans l'ordre chronologique inverse

`ls` liste par défaut le contenu d'un répertoire par ordre alphabétique
par nom. La commande `ls -t` liste les éléments en fonction de la dernière modification
au lieu de la liste alphabétique. La commande `ls -r` liste
les éléments d'un répertoire dans l'ordre inverse.
Que se passe-t-il lorsque vous combinez les options `-t` et `-r` ?
Indice : vous devrez peut-être utiliser l'option `-l` pour voir les
dates de dernière modification.

:::::::::::::::  solution

## Solution

Le fichier modifié le plus récemment est listé en dernier avec
`-rt`. Cela peut être très utile pour trouver vos modifications les plus récentes ou pour vérifier
si un nouveau fichier de sortie a été écrit.



:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

### Explorer d'autres répertoires

Nous pouvons non seulement utiliser `ls` sur le répertoire de travail actuel,
mais nous pouvons également l'utiliser pour afficher le contenu d'un autre répertoire.
Examinons notre répertoire `Desktop` en exécutant `ls -F Desktop`,
c'est-à-dire, la commande `ls` avec l'option `-F` et l'**argument** [**répertoire**][Répertoire] `Desktop`.
L'argument `Desktop` indique à `ls` que
nous voulons une liste de quelque chose d'autre que notre répertoire de travail actuel :

```bash
$ ls -F Desktop
```

Notez que si un répertoire nommé `Desktop` n'existe pas dans votre répertoire de travail actuel,
cette commande renverra une erreur. Normalement, un répertoire `Desktop` existe dans votre
répertoire personnel, que nous supposons être votre répertoire de travail actuel de votre shell bash.

Votre sortie doit être une liste de tous les fichiers et sous-répertoires de votre
répertoire `Desktop`, y compris le répertoire `shell-lesson-data` que vous avez téléchargé lors de la
configuration de cette leçon (sur la plupart des systèmes, le
contenu du répertoire `Desktop` dans le shell s'affiche sous forme d'icônes dans une interface
graphique derrière toutes les fenêtres ouvertes. Vérifiez si c'est le cas pour vous.)

Organiser les choses de manière hiérarchique nous aide à garder une trace de notre travail. Bien qu'il soit
possible de mettre des centaines de fichiers dans notre répertoire personnel tout comme il est possible de
mettre des centaines de papiers imprimés sur notre bureau, il est beaucoup plus facile de trouver des choses lorsque
elles ont été organisées dans des sous-répertoires ayant des noms sensés.

Maintenant que nous savons que le répertoire `shell-lesson-data` se trouve dans notre répertoire `Desktop`, nous
pouvons faire deux choses.

Premièrement, en utilisant la même stratégie auparavant, nous pouvons examiner son contenu en passant
le nom d'un répertoire à `ls` :

```bash
$ ls -F Desktop/shell-lesson-data
```

Deuxièmement, nous pouvons réellement changer de répertoire pour un autre répertoire, donc
nous ne sommes plus situés dans
notre répertoire personnel.

La commande pour changer de répertoire est `cd` suivie d'un
nom de répertoire pour changer de répertoire de travail actuel.
`cd` signifie "changer de répertoire",
ce qui est un peu trompeur.
La commande ne change pas le répertoire ;
elle change le répertoire de travail actuel du shell.
En d'autres termes, cela change les paramètres du shell pour indiquer dans quel répertoire nous nous trouvons.
La commande `cd` est comparable à un double-clic sur un dossier dans une interface graphique
pour entrer dans ce dossier.

Disons que nous voulons nous déplacer dans le répertoire `exercise-data` que nous avons vu précédemment. Nous pouvons
utiliser la série de commandes suivante pour y arriver :

```bash
$ cd Desktop
$ cd shell-lesson-data
$ cd exercise-data
```

Ces commandes nous déplacent de notre répertoire personnel vers notre répertoire Desktop, puis dans
le répertoire shell-lesson-data, puis dans le répertoire exercise-data.
Vous remarquerez que `cd` n'imprime rien. C'est normal.
De nombreuses commandes du shell n'affichent rien à l'écran lorsqu'elles sont exécutées avec succès.
Mais si nous exécutons `pwd` après cela, nous pouvons voir que nous sommes maintenant
dans `/Users/nelle/Desktop/shell-lesson-data/exercise-data`.

Si nous exécutons `ls -F` sans arguments maintenant,
cela liste le contenu de `/Users/nelle/Desktop/shell-lesson-data/exercise-data`,
parce que c'est là où nous nous trouvons maintenant :

```bash
$ pwd
```

```output
/Users/nelle/Desktop/shell-lesson-data/exercise-data
```

```bash
$ ls -F
```

```output
alkanes/  animal-counts/  creatures/  numbers.txt  writing/
```

Nous savons maintenant comment descendre dans l'arborescence des répertoires (c'est-à-dire comment entrer dans un sous-répertoire),
mais comment pouvons-nous remonter (c'est-à-dire quitter un répertoire et entrer dans son répertoire parent) ?
Nous pourrions essayer ce qui suit :

```bash
$ cd shell-lesson-data
```

```error
-bash: cd: shell-lesson-data: No such file or directory
```

Mais nous obtenons une erreur ! Pourquoi ?

Avec les méthodes utilisées jusqu'à présent,
`cd` peut seulement voir les sous-répertoires à l'intérieur de votre répertoire actuel. Il existe
différentes façons de voir les répertoires au-dessus de votre emplacement actuel ; nous commencerons
par le plus simple.

Il existe un raccourci dans le shell pour remonter d'un niveau de répertoire. Voici comment cela fonctionne :

```bash
$ cd ..
```

`..` est un répertoire spécial qui signifie
"le répertoire contenant celui-ci",
ou plus simplement,
le **répertoire parent** du répertoire actuel.
En effet,
si nous exécutons `pwd` après avoir exécuté `cd ..`, nous sommes de retour dans `/Users/nelle/Desktop/shell-lesson-data` :

```bash
$ pwd
```

```output
/Users/nelle/Desktop/shell-lesson-data
```

Le répertoire spécial `..` n'apparaît généralement pas lorsque nous exécutons `ls`. Si nous voulons
l'afficher, nous pouvons ajouter l'option `-a` à `ls -F` :

```bash
$ ls -F -a
```

```output
./  ../  exercise-data/  north-pacific-gyre/
```

`-a` signifie 'afficher tout' (y compris les fichiers cachés) ;
il force `ls` à nous montrer les noms de fichiers et de répertoires qui commencent par `.`,
tels que `..` (qui, si nous nous trouvons dans `/Users/nelle`, fait référence au répertoire `/Users`).
Comme vous pouvez le voir,
il affiche également un autre répertoire spécial appelé `.`,
qui signifie 'le répertoire de travail actuel'.
Il peut sembler redondant d'avoir un nom pour cela,
mais nous verrons à quoi cela sert bientôt.

Notez que la plupart des outils de ligne de commande permettent de combiner plusieurs options
avec un seul tiret `-` et sans espaces entre les options ; `ls -F -a` est
équivalent à `ls -Fa`.

:::::::::::::::::::::::::::::::::::::::::  callout

## Autres fichiers cachés

En plus des répertoires cachés `..` et `.`, vous pouvez également voir un fichier
appelé `.bash_profile`. Ce fichier contient généralement des paramètres de configuration du shell.
Vous pouvez également voir d'autres fichiers et répertoires commençant
par `.`. Ce sont généralement des fichiers et des répertoires utilisés pour configurer
différents programmes sur votre ordinateur. Le préfixe `.` est utilisé pour éviter que ces
fichiers de configuration n'encombrent le terminal lorsqu'une commande `ls` standard
est utilisée.


::::::::::::::::::::::::::::::::::::::::::::::::::

Ces trois commandes sont les commandes de base pour naviguer dans le système de fichiers sur votre ordinateur :
`pwd`, `ls` et `cd`. Explorons maintenant quelques variations de ces commandes. Que se passe-t-il
si vous tapez simplement `cd`, sans donner
de répertoire ?

```bash
$ cd
```

Comment pouvez-vous vérifier ce qui s'est passé ? `pwd` nous donne la réponse !

```bash
$ pwd
```

```output
/Users/nelle
```

Il s'avère que `cd` sans argument vous ramène à votre répertoire personnel,
ce qui est pratique si vous vous êtes perdu dans votre propre système de fichiers.

Essayons maintenant de revenir dans le répertoire `exercise-data` dont nous avons parlé précédemment. La dernière fois, nous avons utilisé
trois commandes, mais nous pouvons réellement enchaîner la liste des répertoires
pour passer à `exercise-data` en une seule étape :

```bash
$ cd Desktop/shell-lesson-data/exercise-data
```

Vérifiez que nous sommes allés au bon endroit en exécutant `pwd` et `ls -F`.

Si nous voulons remonter d'un niveau dans le répertoire des données, nous pourrions utiliser `cd ..`. Mais
il existe une autre façon de passer à n'importe quel répertoire, indépendamment de votre
emplacement actuel.

Jusqu'à présent, lorsque nous avons spécifié des noms de répertoires, ou même un chemin de répertoire (comme ci-dessus),
nous avons utilisé des **chemins relatifs**. Lorsque vous utilisez un chemin relatif avec une commande
comme `ls` ou `cd`, celle-ci tente de trouver cet emplacement à partir de là où nous sommes,
plutôt qu'à partir de la racine du système de fichiers.

Cependant, il est possible de spécifier le **chemin absolu** vers un répertoire en
incluant tout le chemin depuis la racine du répertoire, qui est indiquée par un
slash initial. Le slash initial `/` indique à l'ordinateur de suivre le chemin à partir
de la racine du système de fichiers, il se réfère donc toujours exactement à un répertoire,
peu importe où nous sommes lorsque nous exécutons la commande.

Cela nous permet de passer à notre répertoire `shell-lesson-data` depuis n'importe où sur
le système de fichiers (y compris depuis `exercise-data`). Pour trouver le chemin absolu
que nous cherchons, nous pouvons utiliser `pwd`, puis extraire la partie dont nous avons besoin
pour passer à `shell-lesson-data`.

```bash
$ pwd
```

```output
/Users/nelle/Desktop/shell-lesson-data/exercise-data
```

```bash
$ cd /Users/nelle/Desktop/shell-lesson-data
```

Exécutez `pwd` et `ls -F` pour vous assurer que vous êtes dans le répertoire attendu.

:::::::::::::::::::::::::::::::::::::::::  callout

## Deux autres raccourcis

Le shell interprète le caractère tilde (`~`) au début d'un chemin comme
signifiant "le répertoire personnel de l'utilisateur en cours". Par exemple, si le répertoire personnel de Nelle est `/Users/nelle`, alors `~/data` est équivalent à `/Users/nelle/data`. Cela ne fonctionne que s'il est le premier caractère du
chemin ; `here/there/~/elsewhere` n'est PAS `here/there/Users/nelle/elsewhere`.

Un autre raccourci est le caractère `-` (tiret). `cd` traduira `-` en
*le répertoire précédent dans lequel j'étais*, ce qui est plus rapide que de devoir retenir,
puis de taper, le chemin complet.  C'est un moyen *très* efficace de se déplacer
*aller-retour entre deux répertoires* -- c'est-à-dire si vous exécutez `cd -` deux fois,
vous retournez au répertoire de départ.

La différence entre `cd ..` et `cd -` est que le premier vous fait *monter*, tandis que le second vous fait *revenir*.

***

Essayez !
D'abord allez à `~/Desktop/shell-lesson-data` (vous devriez déjà vous trouver ici).

```bash
$ cd ~/Desktop/shell-lesson-data
```

Ensuite `cd` dans le répertoire `exercise-data/creatures`

```bash
$ cd exercise-data/creatures
```

Maintenant, si vous exécutez

```bash
$ cd -
```

vous verrez que vous êtes de retour dans `~/Desktop/shell-lesson-data`.
Exécutez à nouveau `cd -` et vous êtes de retour dans `~/Desktop/shell-lesson-data/exercise-data/creatures`


::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## Chemins absolus vs relatifs

En partant de `/Users/nelle/data`,
laquelle des commandes suivantes Nelle pourrait-elle utiliser pour naviguer vers son répertoire personnel,
qui est `/Users/nelle` ?

1. `cd .`
2. `cd /`
3. `cd /home/nelle`
4. `cd ../..`
5. `cd ~`
6. `cd home`
7. `cd ~/data/..`
8. `cd`
9. `cd ..`

:::::::::::::::  solution

## Solution

1. Non : `.` signifie le répertoire en cours.
2. Non : `/` signifie le répertoire racine.
3. Non : le répertoire personnel de Nelle est `/Users/nelle`.
4. Non : cette commande remonte de deux niveaux, c'est-à-dire jusqu'à `/Users`.
5. Oui : `~` signifie le répertoire personnel de l'utilisateur, dans ce cas `/Users/nelle`.
6. Non : cette commande naviguerait dans un répertoire `home` du répertoire courant
   s'il existe.
7. Oui : compliqué inutilement, mais correct.
8. Oui : raccourci pour revenir au répertoire personnel de l'utilisateur.
9. Oui : remonte d'un niveau.

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## Résolution des chemins relatifs

En utilisant le diagramme du système de fichiers ci-dessous, si `pwd` affiche `/Users/thing`,
qu'affichera `ls -F ../backup`?

1. `../backup: No such file or directory`
2. `2012-12-01 2013-01-08 2013-01-27`
3. `2012-12-01/ 2013-01-08/ 2013-01-27/`
4. `original/ pnas_final/ pnas_sub/`

![](fig/filesystem-challenge.svg){alt='A directory tree below the Users directory where "/Users" contains thedirectories "backup" and "thing"; "/Users/backup" contains "original","pnas\_final" and "pnas\_sub"; "/Users/thing" contains "backup"; and"/Users/thing/backup" contains "2012-12-01", "2013-01-08" and"2013-01-27"'}

:::::::::::::::  solution

## Solution

1. Non : il *existe* un répertoire `backup` dans `/Users`.
2. Non : ce sont les contenus de `Users/thing/backup`,
   mais avec `..`, nous avons demandé un niveau plus haut.
3. Non : voir l'explication précédente.
4. Oui : `../backup/` fait référence à `/Users/backup/`.
  
  

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## Compréhension de `ls`

En utilisant le diagramme du système de fichiers ci-dessous,
si `pwd` affiche `/Users/backup`,
et que `-r` indique à `ls` d'afficher les choses dans l'ordre inverse,
quelle(s) commande(s) donnera(ont) le résultat suivant :

```output
pnas_sub/ pnas_final/ original/
```

![](fig/filesystem-challenge.svg){alt='A directory tree below the Users directory where "/Users" contains thedirectories "backup" and "thing"; "/Users/backup" contains "original","pnas\_final" and "pnas\_sub"; "/Users/thing" contains "backup"; and"/Users/thing/backup" contains "2012-12-01", "2013-01-08" and"2013-01-27"'}

1. `ls pwd`
2. `ls -r -F`
3. `ls -r -F /Users/backup`

:::::::::::::::  solution

## Solution

1. Non : `pwd` n'est pas le nom d'un répertoire.
2. Oui : `ls` sans argument répertoire liste les fichiers et les répertoires
   du répertoire courant.
3. Oui : utilise le chemin absolu explicitement.
  
  

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

## Syntaxe générale d'une commande shell

Nous avons déjà rencontré des commandes, des options et des arguments,
mais il est peut-être utile de formaliser un peu la terminologie.

Considérez la commande ci-dessous comme un exemple général d'une commande,
que nous allons disséquer en ses composantes :

```bash
$ ls -F /
```

![](fig/shell_command_syntax.svg){alt='Génération de la syntaxe d'une commande shell'}

`ls` est la **commande**, avec une **option** `-F` et un
**argument** `/`.
Nous avons déjà rencontré des options qui
commencent soit par un tiret simple (`-`), appelées **options courtes**,
soit deux tirets (`--`), appelées **options longues**.
[Les options] changent le comportement d'une commande et
[les arguments] indiquent à la commande sur quoi faire l'opération (par exemple, des fichiers et des répertoires).
Parfois, on se réfère aux options et aux arguments comme **paramètres**.
Une commande peut être appelée avec plus d'une option et plus d'un argument, mais une commande
n'a pas toujours besoin d'un argument ou d'une option.

Il peut arriver que l'on voie des options appelées **switches** ou **flags**,
surtout pour les options qui ne prennent pas d'argument. Dans cette leçon, nous utiliserons
le terme *option*.

Chaque partie est séparée par des espaces. Si vous omettez l'espace
entre `ls` et `-F`, le shell recherchera une commande appelée `ls-F`, qui
n'existe pas. De plus, la casse peut être importante.
Par exemple, `ls -s` affiche la taille des fichiers et des répertoires à côté des noms,
tandis que `ls -S` trie les fichiers et les répertoires par taille, comme indiqué ci-dessous :

```bash
$ cd ~/Desktop/shell-lesson-data
$ ls -s exercise-data
```

```output
total 28
 4 animal-counts   4 creatures  12 numbers.txt   4 alkanes   4 writing
```

Notez que les tailles renvoyées par `ls -s` sont exprimées en *blocs*.
Comme ces blocs sont définis différemment pour les différents systèmes d'exploitation,
vous ne pourrez peut-être pas obtenir les mêmes chiffres que dans l'exemple.

```bash
$ ls -S exercise-data
```

```output
animal-counts  creatures  alkanes  writing  numbers.txt
```

En mettant tout cela ensemble, notre commande `ls -F /` ci-dessus nous donne une liste
de fichiers et de répertoires dans le répertoire racine `/`.
Un exemple de la sortie que vous pourriez obtenir avec la commande ci-dessus est donné ci-dessous :

```bash
$ ls -F /
```

```output
Applications/         System/
Library/              Users/
Network/              Volumes/
```


:::::::::::::::::::::::::::::::::::::::: ====>

:paramètres:
 - Ce que je comprends mieux maintenant c'est que `pwd` nous donne le chemin absolu, ce chemin commence à la racine. 
 - Une autre chose que j'ai apprise dans la leçon est qu'il y a une différence entre un chemin absolu qui commence par `/` et un chemin relatif.
 - J'ai aussi compris l'intérêt de la complétion automatique pour économiser du temps en évitant de taper le chemin complet.

:paramètres: anglais, français
