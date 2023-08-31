---
title: Travailler avec des Fichiers et des Répertoires
teaching: 30
exercises: 20
---

::::::::::::::::::::::::::::::::::::::: objectifs

- Créer une hiérarchie de répertoires qui correspond à un diagramme donné.
- Créer des fichiers dans cette hiérarchie en utilisant un éditeur ou en copiant et renommant des fichiers existants.
- Supprimer, copier et déplacer des fichiers et/ou des répertoires spécifiés.

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- Comment puis-je créer, copier et supprimer des fichiers et des répertoires ?
- Comment puis-je modifier des fichiers ?

::::::::::::::::::::::::::::::::::::::::::::::::::


## Création de répertoires

Nous savons maintenant comment explorer les fichiers et les répertoires,
mais comment les créons-nous en premier lieu ?

Dans cet épisode, nous apprendrons à créer et à déplacer des fichiers et des répertoires,
en utilisant le répertoire `exercise-data/writing` comme exemple.

### Étape un : voir où nous sommes et ce que nous avons déjà

Nous devrions toujours être dans le répertoire `shell-lesson-data` sur le bureau,
que nous pouvons vérifier en utilisant :

```bash
$ pwd
```

```output
/Users/nelle/Desktop/shell-lesson-data
```

Ensuite, nous nous déplacerons dans le répertoire `exercise-data/writing` et verrons ce qu'il contient :

```bash
$ cd exercise-data/writing/
$ ls -F
```

```output
haiku.txt  LittleWomen.txt
```

### Créer un répertoire

Créons un nouveau répertoire appelé `thesis` en utilisant la commande `mkdir thesis`
(qui ne produit aucune sortie) :

```bash
$ mkdir thesis
```

Comme vous pouvez le deviner d'après son nom,
`mkdir` signifie "make directory" (créer un répertoire).
Comme `thesis` est un chemin relatif
(c'est-à-dire qu'il ne commence pas par un slash, comme `/quoi/que/ce/soit/thesis`),
le nouveau répertoire est créé dans le répertoire de travail actuel :

```bash
$ ls -F
```

```output
haiku.txt  LittleWomen.txt  thesis/
```

Comme nous venons de créer le répertoire `thesis`, il est encore vide :

```bash
$ ls -F thesis
```

Notez que `mkdir` n'est pas limité à la création de répertoires uniques à la fois.
L'option `-p` permet à `mkdir` de créer un répertoire avec des sous-répertoires imbriqués
en une seule opération :

```bash
$ mkdir -p ../project/data ../project/results
```

L'option `-R` de la commande `ls` liste tous les sous-répertoires imbriqués dans un répertoire.
Utilisons `ls -FR` pour lister de manière récursive la nouvelle hiérarchie de répertoires que nous venons de créer dans le répertoire `project` :

```bash
$ ls -FR ../project
```

```output
../project/:
data/  results/

../project/data:

../project/results:
```

:::::::::::::::::::::::::::::::::::::::::  callout

## Deux façons de faire la même chose

Utiliser le shell pour créer un répertoire n'est pas différent d'utiliser un explorateur de fichiers.
Si vous ouvrez le répertoire courant à l'aide de l'explorateur de fichiers graphique de votre système d'exploitation,
le répertoire `thesis` y apparaîtra également.
Bien que le shell et l'explorateur de fichiers soient deux façons différentes d'interagir avec les fichiers,
les fichiers et les répertoires eux-mêmes sont les mêmes.

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::::  callout

## Bonnes noms de fichiers et de répertoires

Les noms compliqués de fichiers et de répertoires peuvent rendre votre vie difficile
lorsque vous travaillez en ligne de commande. Voici quelques conseils utiles
pour les noms de vos fichiers et répertoires.

1. N'utilisez pas d'espaces.

Les espaces peuvent rendre un nom plus signifiant,
mais comme les espaces sont utilisés pour séparer les arguments en ligne de commande,
il est préférable de les éviter dans les noms de fichiers et de répertoires.
Vous pouvez utiliser le tiret `-` ou le tiret bas `_` à la place (par exemple `north-pacific-gyre/` plutôt que `north pacific gyre/`).
Pour tester cela, essayez de taper `mkdir north pacific gyre` et voyez quels répertoires sont créés lorsque vous vérifiez avec `ls -F`.

2. Ne commencez pas le nom par `-` (tiret).

Les commandes traitent les noms commençant par `-` comme des options.

3. Restez avec les lettres, les chiffres, `.` (point) `-` (tiret) et `_` (tiret bas).

De nombreux autres caractères ont des significations spéciales sur la ligne de commande.
Nous en apprendrons davantage sur certains d'entre eux tout au long de cette leçon.
Il existe des caractères spéciaux qui peuvent empêcher votre commande de fonctionner comme prévu
et même entraîner une perte de données.

Si vous devez faire référence à des noms de fichiers ou de répertoires contenant des espaces
ou d'autres caractères spéciaux, vous devez entourer le nom de guillemets (`""`).

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::::  instructor

Il arrive parfois que les apprenants se retrouvent piégés dans des éditeurs de texte en ligne de commande
comme Vim, Emacs ou Nano. Fermer l'émulateur de terminal et en ouvrir un nouveau peut être frustrant car les apprenants doivent naviguer à nouveau vers le bon dossier. Notre recommandation pour atténuer ce problème est que les instructeurs utilisent le même éditeur de texte que les apprenants pendant les ateliers (dans la plupart des cas Nano).

::::::::::::::::::::::::::::::::::::::::::::::::::

### Créer un fichier texte

Modifions notre répertoire de travail pour l'ensemble `thesis` en utilisant `cd`,
puis exécutez un éditeur de texte appelé Nano pour créer un fichier appelé `draft.txt` :

```bash
$ cd thesis
$ nano draft.txt
```

:::::::::::::::::::::::::::::::::::::::::  callout

## Quel éditeur choisir ?

Lorsque nous disons que "`nano` est un éditeur de texte", nous voulons vraiment dire "texte". Il ne peut
travailler qu'avec des données de caractères simples, pas avec des tableaux, des images ou tout autre
média convivial pour l'homme. Nous l'utilisons en exemple parce que c'est l'un des éditeurs de texte les moins complexes. Cependant, en raison de cette caractéristique, il peut
ne pas être assez puissant ou assez flexible pour le travail que vous devez faire
après cet atelier. Sur les systèmes Unix (tels que Linux et macOS),
de nombreux programmeurs utilisent [Emacs](https://www.gnu.org/software/emacs/) ou
[Vim](https://www.vim.org/) (qui nécessitent tous les deux plus de temps pour les apprendre),
ou un éditeur graphique tel que [Gedit](https://projects.gnome.org/gedit/)
ou [VScode](https://code.visualstudio.com/). Sur Windows, vous pouvez également utiliser
[Notepad++](https://notepad-plus-plus.org/). Windows dispose également d'un éditeur intégré
appelé `notepad` qui peut être exécuté à partir de la ligne de commande de la même
manière que `nano` dans le but de cette leçon.

Quel que soit l'éditeur que vous utilisez, vous devrez savoir où il recherche
les fichiers et où il les enregistre. Si vous le démarrez à partir du shell, il utilisera (probablement)
le répertoire de travail actuel comme emplacement par défaut. Si vous utilisez
le menu démarrer de votre ordinateur, il voudra peut-être enregistrer les fichiers dans votre bureau ou
dans le répertoire Documents. Vous pouvez changer cela en accédant à
un autre répertoire la première fois que vous enregistrez avec "Enregistrer sous..."

::::::::::::::::::::::::::::::::::::::::::::::::::

Saisissons quelques lignes de texte.

![](fig/nano-screenshot.png){alt="capture d'écran de l'éditeur de texte nano en action avec le texte Ce n'est plus publier ou périr, c'est partager et s'épanouir"}

Une fois satisfaits de notre texte, nous pouvons appuyer sur <kbd>Ctrl</kbd>\+<kbd>O</kbd>
(appuyez sur la touche <kbd>Ctrl</kbd> ou <kbd>Control</kbd>, puis, tout en la maintenant enfoncée, appuyez sur la touche <kbd>O</kbd>) pour écrire nos données sur le disque. On nous demandera
de donner un nom au fichier qui contiendra notre texte. Appuyez sur <kbd>Entrée</kbd> pour accepter
la valeur par défaut suggérée, `draft.txt`.

Une fois notre fichier enregistré, nous pouvons utiliser <kbd>Ctrl</kbd>\+<kbd>X</kbd> pour quitter l'éditeur et
retourner à l'interpréteur de commandes.

:::::::::::::::::::::::::::::::::::::::::  callout

## Contrôle, Ctrl ou ^ Key

La touche Control est également appelée touche 'Ctrl'. Il existe plusieurs façons
de décrire l'utilisation de la touche Control. Par exemple, vous pouvez
voir une instruction pour appuyer sur la touche <kbd>Control</kbd> et, en la maintenant enfoncée,
appuyez sur la touche <kbd>X</kbd>, décrite comme l'une des possibilités suivantes :

- `Control-X`
- `Control+X`
- `Ctrl-X`
- `Ctrl+X`
- `^X`
- `C-x`

Dans nano, en bas de l'écran, vous verrez `^G Get Help ^O WriteOut`.
Cela signifie que vous pouvez utiliser `Control-G` pour obtenir de l'aide et `Control-O` pour enregistrer votre
fichier.

::::::::::::::::::::::::::::::::::::::::::::::::::

nano ne laisse aucune sortie à l'écran lorsqu'il se termine,
mais `ls` montre maintenant que nous avons créé un fichier appelé `draft.txt` :

```bash
$ ls
```

```output
draft.txt
```

:::::::::::::::::::::::::::::::::::::::  challenge

## Créer des fichiers d'une autre façon

Nous avons vu comment créer des fichiers texte à l'aide de l'éditeur `nano`.
Maintenant, essayez la commande suivante :

```bash
$ touch mon_fichier.txt
```

1. Que fait la commande `touch` ?
   Lorsque vous regardez votre répertoire courant à l'aide d'un explorateur de fichiers graphique,
   est-ce que le fichier apparaît ?

2. Utilisez `ls -l` pour inspecter les fichiers. Quelle est la taille de `mon_fichier.txt` ?

3. Quand pourriez-vous vouloir créer un fichier de cette façon ?

:::::::::::::::  solution

## Solution

1. La commande `touch` génère un nouveau fichier appelé `mon_fichier.txt` dans
   votre répertoire courant. Vous pouvez observer ce fichier nouvellement créé en tapant `ls`
   à l'invite de commande. `mon_fichier.txt` peut également être consulté dans votre
   explorateur de fichiers graphique.

2. Lorsque vous inspectez le fichier avec `ls -l`, notez que la taille de
   `mon_fichier.txt` est de 0 octet. En d'autres termes, il ne contient pas de données.
   S'il vous plaît ouvrir `mon_fichier.txt` en utilisant votre éditeur de texte de votre choix (y compris nano) pour confirmier.

3. Certains programmes ne génèrent pas eux-mêmes des fichiers de sortie, mais
   exigent plutôt que des fichiers vides aient déjà été générés. Lorsque le programme est exécuté, il recherche un fichier existant pour le
   remplir avec sa sortie. La commande touch vous permet de générer efficacement un fichier texte vierge à utiliser par de tels programmes.

:::::::::::::::::::::::::

Pour éviter toute confusion plus tard,
nous vous suggérons de supprimer le fichier que vous venez de créer avant de continuer avec le reste
de l'épisode, sinon les sorties futures peuvent varier de celles données dans la leçon.
Pour ce faire, utilisez la commande suivante :

```bash
$ rm mon_fichier.txt
```

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::::  callout

## Que représente un nom ?

Vous avez peut-être remarqué que tous les fichiers de Nelle s'appellent "quelque chose point quelque chose", et dans cette partie de la leçon, nous avons toujours utilisé l'extension `.txt`. C'est juste une convention ; nous pouvons appeler un fichier `mathese` ou presque tout ce que nous voulons. Cependant, la plupart des gens utilisent des noms en deux parties la plupart du temps pour les aider (et leurs programmes) à distinguer les différents types de fichiers. La deuxième partie d'un tel nom s'appelle l'**extension de fichier** et indique le type de données que le fichier contient : `.txt` signale un fichier texte brut, `.pdf` indique un document PDF, `.cfg` est un fichier de configuration contenant des paramètres pour un programme ou autre, `.png` est une image PNG, etc.

C'est juste une convention, bien qu'importante. Les fichiers contiennent simplement des octets ; il nous revient, à nous et à nos programmes, d'interpréter ces octets conformément aux règles des fichiers texte brut, des documents PDF, des fichiers de configuration, des images, etc.

Appeler une image PNG d'une baleine `whale.mp3` ne la transforme pas magiquement en un enregistrement de son de baleine, bien que cela *puisse* pousser le système d'exploitation à associer le fichier à un programme de lecteur de musique. Dans ce cas, si quelqu'un double-clique sur `whale.mp3` dans un programme explorateur de fichiers, le lecteur de musique essaiera automatiquement (et à tort) d'ouvrir le fichier `whale.mp3`.

::::::::::::::::::::::::::::::::::::::::::::::::::

## Déplacement de fichiers et de répertoires

En retournant dans le répertoire `shell-lesson-data/exercise-data/writing`,

```bash
$ cd ~/Desktop/shell-lesson-data/exercise-data/writing
```

Dans notre répertoire `thesis`, nous avons un fichier `draft.txt`
qui n'est pas un nom particulièrement informatif,
alors renommons le fichier en utilisant `mv`,
qui est l'abréviation de "move" (déplacer) :

```bash
$ mv thesis/draft.txt thesis/quotes.txt
```

Le premier argument indique à `mv` ce que nous "déplaçons",
tandis que le deuxième est où il doit aller.
Dans ce cas,
nous déplaçons `thesis/draft.txt` vers `thesis/quotes.txt`,
ce qui a pour effet de renommer le fichier.
En effet,
`ls` nous montre que `thesis` contient maintenant un seul fichier appelé `quotes.txt` :

```bash
$ ls thesis
```

```output
quotes.txt
```

Il faut être prudent lorsque l'on spécifie le nom du fichier de destination, car `mv` va
silencieusement écraser tous les fichiers existants ayant le même nom, ce qui pourrait
entrainer une perte de données. Par défaut, `mv` ne demande pas confirmation avant d'écraser les fichiers.
Cependant, une option supplémentaire, `mv -i` (ou `mv --interactive`), permet à `mv` de demander
une telle confirmation.

Notez que `mv` fonctionne également avec les répertoires.

Déplaçons `quotes.txt` dans le répertoire de travail courant.
Nous utilisons à nouveau `mv`,
mais cette fois, nous utilisons simplement le nom d'un répertoire en tant que deuxième argument
pour indiquer à `mv` que nous voulons conserver le nom du fichier
mais mettre le fichier quelque part de nouveau.
(C'est pourquoi la commande s'appelle "move".)
Dans ce cas,
le nom du répertoire que nous utilisons est le nom de répertoire spécial `.` dont nous avons parlé plus tôt.

```bash
$ mv thesis/quotes.txt .
```

L'effet est de déplacer le fichier du répertoire dans lequel il se trouvait vers le répertoire de travail courant.
`ls` nous montre maintenant que `thesis` est vide :

```bash
$ ls thesis
```

```output
$
```

Alternativement, nous pouvons confirmer que le fichier `quotes.txt` n'est plus présent dans le répertoire `thesis`
en essayant explicitement de le lister :

```bash
$ ls thesis/quotes.txt
```

```error
ls: ne parvient pas à accéder à 'thesis/quotes.txt': Aucun fichier ou dossier de ce type
```

`ls` avec un nom de fichier ou de répertoire comme argument ne liste que le fichier ou le répertoire demandé.
Si le fichier donné en argument n'existe pas, le shell renvoie une erreur comme nous l'avons vu ci-dessus.
Nous pouvons utiliser cela pour voir que `quotes.txt` est maintenant présent dans notre répertoire de travail courant :

```bash
$ ls quotes.txt
```

```output
quotes.txt
```

:::::::::::::::::::::::::::::::::::::::  challenge

## Déplacer des fichiers vers un nouveau dossier

Après avoir exécuté les commandes suivantes,
Jamie réalise qu'elle a placé les fichiers `sucrose.dat` et `maltose.dat` dans le mauvais dossier.
Les fichiers auraient dû être placés dans le dossier `raw`.

```bash
$ ls -F
 analyzed/ raw/
$ ls -F analyzed
fructose.dat glucose.dat maltose.dat sucrose.dat
$ cd analyzed
```

Remplissez les blancs pour déplacer ces fichiers dans le dossier `raw/`
(c'est-à-dire celui qu'elle a oublié de rentrer dans)

```bash
$ mv sucrose.dat maltose.dat ____/____
```

:::::::::::::::  solution

## Solution

```bash
$ mv sucrose.dat maltose.dat ../raw
```

Rappelez-vous que `..` fait référence au répertoire parent (c'est-à-dire un niveau au-dessus du répertoire courant)
et que `.` fait référence au répertoire courant.

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

## Copie de fichiers et de répertoires

La commande `cp` fonctionne de la même manière que `mv`,
sauf qu'elle copie un fichier au lieu de le déplacer.
Nous pouvons vérifier que cela a été fait correctement en utilisant `ls`
avec deux chemins d'accès comme arguments --- comme la plupart des commandes Unix,
`ls` peut recevoir plusieurs chemins d'accès à la fois :

```bash
$ cp quotes.txt thesis/quotations.txt
$ ls quotes.txt thesis/quotations.txt
```

```output
quotes.txt   thesis/quotations.txt
```

Nous pouvons également copier un répertoire et tout son contenu en utilisant l'option
[récursive](https://fr.wikipedia.org/wiki/R%C3%A9cursivit%C3%A9) `-r`, par exemple pour sauvegarder un répertoire :

```bash
$ cp -r thesis thesis_backup
```

Nous pouvons vérifier le résultat en listant le contenu du répertoire `thesis` et `thesis_backup` :

```bash
$ ls thesis thesis_backup
```

```output
thesis:
quotations.txt

thesis_backup:
quotations.txt
```

Il est important d'inclure l'option `-r`. Si vous voulez copier un répertoire et omettez cette option
vous verrez un message indiquant que le répertoire a été omis car `-r` n'a pas été spécifié.

``` bash
$ cp thesis thesis_backup
cp: -r not specified; omitting directory 'thesis'
```


:::::::::::::::::::::::::::::::::::::::  challenge

## Renommer des fichiers

Supposons que vous avez créé un fichier texte dans votre répertoire courant pour contenir une liste des
tests statistiques que vous devrez effectuer pour analyser vos données, et que vous l'ayez nommé `statstics.txt`

Après avoir créé et enregistré ce fichier, vous vous rendez compte que vous avez mal orthographié le nom du fichier ! Vous voulez
corriger l'erreur. Quelle(s) commande(s) couverte(s) dans cette leçon doit-elle exécuter
pour que les commandes ci-dessous produisent la sortie indiquée ?

```bash
$ ls -F
analyzed/ raw/ sucrose.dat
```

```bash
$ mv statstics.txt statistics.txt
$ ls -F
```

```output
analyzed/ raw/ statistics.txt sucrose.dat
```

```bash
$ ls analyzed
```

```output
statistics.txt sucrose.dat
```

:::::::::::::::  solution

## Solution

La solution est `mv statstics.txt statistics.txt`

1. Cette solution fonctionne. Le fichier `statstics.txt` est renommé en `statistics.txt`,
   l'effet est de créer un fichier avec le nom de `statstics.txt` et le contenu d'origine est copié dans `statistics.txt`.
   Nous notons que `sucrose.dat` n'a pas été affecté car il n'a pas été mentionné dans la commande.
2. Cette solution produirait la même sortie car elle est identique à la première solution, mais utilise des chemins relatifs au lieu d'utiliser le chemin absolu.
3. Cette commande donne une erreur car elle essaie de déplacer le fichier `statistics.txt` dans l'analyse sans spécifier son nouveau nom,
   donc elle n'est pas renommée mais déplacée.
4. Cette solution aurait pu fonctionner si nous avions spécifié le nom du fichier dans notre première commande.
   Mais dans cette forme actuelle, il essaie de déplacer les fichiers `statstics.txt` et `statistics.txt`
   dans le répertoire `analyzed`, et donne donc une erreur.

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## Déplacer et Copier

Que donne la commande `ls` de clôture dans la séquence indiquée ci-dessous ?

```bash
$ pwd
```

```output
/Users/jamie/data
```

```bash
$ ls
```

```output
proteins.dat
```

```bash
$ mkdir recombined
$ mv proteins.dat recombined/
$ cp recombined/proteins.dat ../proteins-saved.dat
$ ls
```

1. `proteins-saved.dat recombined`
2. `recombined`
3. `proteins.dat recombined`
4. `proteins-saved.dat`

:::::::::::::::  solution

## Solution

Nous commençons par le répertoire `/Users/jamie/data` et créons un nouveau répertoire appelé `recombined`.
La deuxième ligne déplace (`mv`) le fichier `proteins.dat` vers le nouveau répertoire (`recombined`).
La troisième ligne fait une copie du fichier que nous venons de déplacer.
La partie délicate ici est l'endroit où le fichier a été copié.
Rappelez-vous que `..` fait référence au répertoire parent, donc le fichier copié est maintenant dans `/Users/jamie`.
Notez que `..` est interprété par rapport au répertoire de travail courant, **pas** par rapport à l'emplacement du fichier à copier.
Donc, la seule chose qui sera affichée en utilisant ls (dans `/Users/jamie/data`) est le répertoire `recombined`.

1. Non, voir explication ci-dessus. `proteins-saved.dat` est situé à `/Users/jamie`
2. Oui
3. Non, voir explication ci-dessus. `proteins.dat` est situé à `/Users/jamie/data/recombined`
4. Non, voir explication ci-dessus. `proteins-saved.dat` est situé à `/Users/jamie`

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

## Suppression de fichiers et de répertoires

En retournant dans le répertoire `shell-lesson-data/exercise-data/writing`,

```bash
$ cd ~/Desktop/shell-lesson-data/exercise-data/writing
```

Pour nettoyer ce répertoire, supprimons le fichier `quotes.txt` que nous avons créé.
La commande Unix que nous utiliserons pour ce faire est `rm` (remove, supprimer) :

```bash
$ rm quotes.txt
```

Nous pouvons confirmer la suppression du fichier en utilisant `ls` :

```bash
$ ls quotes.txt
```

```error
ls: cannot access 'quotes.txt': No such file or directory
```

:::::::::::::::::::::::::::::::::::::::::  callout

## La suppression est définitive

Le shell Unix n'a pas de corbeille dans laquelle nous pouvons récupérer les fichiers supprimés (bien que la plupart des interfaces graphiques à Unix le fassent). Au lieu de cela, lorsque nous supprimons des fichiers, ils sont détachés du système de fichiers afin que leur espace de stockage sur disque puisse être réutilisé. Des outils permettant de trouver et de récupérer des fichiers supprimés existent, mais il n'y a aucune garantie qu'ils fonctionneront dans n'importe quelle situation particulière, car l'ordinateur peut réutiliser l'espace disque du fichier immédiatement.

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## Utilisation de `rm` en toute sécurité

Que se passe-t-il lorsque nous exécutons `rm -i thesis_backup/quotations.txt` ?
Pourquoi voudrait-on cette protection lors de l'utilisation de `rm` ?

:::::::::::::::  solution

## Solution

```output
rm: remove regular file 'thesis_backup/quotations.txt'? y
```

L'option `-i` demandera une confirmation avant (chaque) suppression (utilisez <kbd>Y</kbd> pour confirmer la suppression
ou <kbd>N</kbd> pour conserver le fichier).
Le shell Unix n'a pas de corbeille, donc tous les fichiers supprimés disparaîtront définitivement.
En utilisant l'option `-i`, nous avons la possibilité de vérifier que nous supprimons uniquement les fichiers
que nous voulons supprimer.

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

Si nous essayons de supprimer le répertoire `thesis` en utilisant `rm thesis`,
nous recevons un message d'erreur :

```bash
$ rm thesis
```

```error
rm: cannot remove 'thesis': Is a directory
```

Cela se produit parce que `rm` fonctionne par défaut uniquement sur les fichiers, pas sur les répertoires.

`rm` peut supprimer un répertoire *et tous ses contenus* si nous utilisons l'option
récursive `-r`, et le fera *sans demander de confirmation* :

```bash
$ rm -r thesis
```

Étant donné qu'il n'y a aucun moyen de récupérer des fichiers supprimés à l'aide de l'interpréteur de commandes,
`rm -r` *doit être utilisé avec une grande prudence*
(vous pouvez envisager d'ajouter l'option interactive `rm -r -i`).

## Opérations avec plusieurs fichiers et répertoires

Souvent, on a besoin de copier ou de déplacer plusieurs fichiers en même temps.
Cela peut être fait en fournissant une liste de noms de fichiers individuels,
ou en spécifiant un motif de nommage à l'aide de caractères de remplacement. Les caractères de remplacement sont
des caractères spéciaux qui peuvent être utilisés pour représenter des caractères inconnus
ou des ensembles de caractères lors de la navigation dans le système de fichiers Unix.

:::::::::::::::::::::::::::::::::::::::::  challenge

## Copier avec plusieurs noms de fichiers

Dans cet exercice, vous pouvez tester les commandes dans le répertoire `shell-lesson-data/exercise-data`.

Dans l'exemple ci-dessous, que fait `cp` lorsqu'il reçoit plusieurs noms de fichiers et un nom de répertoire ?

```bash
$ mkdir backup
$ cp creatures/minotaur.dat creatures/unicorn.dat backup/
```

Dans l'exemple ci-dessous, que fait `cp` lorsqu'il reçoit trois noms de fichiers ou plus ?

```bash
$ cd creatures
$ ls -F
```

```output
basilisk.dat  minotaur.dat  unicorn.dat
```

```bash
$ cp minotaur.dat unicorn.dat basilisk.dat
```

:::::::::::::::  solution

## Solution

Si vous donnez plus d'un nom de fichier suivi d'un nom de répertoire
(c'est-à-dire que le répertoire de destination doit être le dernier argument),
`cp` copie les fichiers dans le répertoire indiqué.

Si vous donnez trois noms de fichier, `cp` renvoie une erreur comme celle ci-dessous,
car il attend un nom de répertoire comme dernier argument.

```error
cp: target 'basilisk.dat' is not a directory
```

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

### Utiliser des caractères de remplacement pour accéder à plusieurs fichiers en même temps

:::::::::::::::::::::::::::::::::::::::::  callout

## Caractères de remplacement

`*` est un **caractère de remplacement**, qui représente zéro ou plusieurs autres caractères.
Prenons en considération le répertoire `shell-lesson-data/exercise-data/alkanes` :
`*.pdb` représente `ethane.pdb`, `propane.pdb`, et tous
les fichiers qui se terminent par '.pdb'. D'un autre côté, `p*.pdb` représente
`pentane.pdb` et `propane.pdb`, car le 'p' au début ne peut
représenter que des noms de fichiers commençant par la lettre 'p'.

`?` est également un caractère de remplacement, mais il représente exactement un caractère.
Ainsi, `?ethane.pdb` pourrait représenter `methane.pdb`, tandis que
`*ethane.pdb` représente à la fois `ethane.pdb` et `methane.pdb`.

Les caractères de remplacement peuvent être utilisés en combinaison les uns avec les autres. Par exemple,
`???ane.pdb` indique trois caractères suivis de `ane.pdb`,
donnant `cubane.pdb  ethane.pdb  octane.pdb`.

Lorsque le shell voit un caractère générique, il l'étend pour créer une
liste de noms de fichiers correspondants *avant* d'exécuter la commande précédente.
À titre d'exception, si une expression de caractère générique ne correspond à
aucun fichier, Bash transmettra l'expression sous forme d'argument à la commande
telle quelle. Par exemple, en tapant `ls *.pdf` dans le répertoire `alkanes`
(qui contient uniquement des fichiers portant des noms se terminant par `.pdb`)
produit un message d'erreur indiquant qu'il n'y a pas de fichier appelé `*.pdf`.
Cependant, généralement les commandes comme `wc` et `ls` voient les listes de
noms de fichiers correspondant à ces expressions, mais pas les caractères génériques
eux-mêmes. C'est le shell, et non les autres programmes, qui développent
les caractères génériques.

Pour représenter plusieurs fichiers à la fois, vous pouvez utiliser des caractères génériques, par exemple `*.txt` représente tous les fichiers se terminant par `.txt`. Le caractère `?` correspond à un seul caractère.

:::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::::::::::::

### Utilisation de caractères génériques pour accéder à plusieurs fichiers en même temps

:::::::::::::::::::::::::::::::::::::::::  callout

## Commentaires sur les caractères génériques

`*` correspond à zéro ou plusieurs caractères.
Donc `*.txt` correspond à `a.txt`, `file.txt` et `file2.txt`.

`?` correspond à un seul caractère.
Donc `?.txt` pourrait correspondre à `a.txt`, mais ne correspondrait pas à `any.txt`.

Il est possible de combiner des caractères génériques. Par exemple,
`?a*.txt` correspondrait à `a1.txt`, `adata.txt` et `a2.txt`.

La correspondance est sensible à la casse, donc `*.TXT` ne correspondra pas à `file.txt`.

Utilisez `echo` pour confirmer quelle commande aurait quel caractère
correspondance.

Ce sont les bases. Un seul caractère de remplacement peut être utilisé
ou plusieurs à la fois.

:::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## Afficher les noms de fichiers correspondant à un motif

Lorsqu'il est exécuté dans le répertoire `alkanes`, quelle commande `ls`
produira cette sortie ?

`ethane.pdb   methane.pdb`

1. `ls *t*ane.pdb`
2. `ls *t?ne.*`
3. `ls *t??ne.pdb`
4. `ls ethane.*`

:::::::::::::::  solution

## Solution

La solution est `3.`.

`1.` affiche tous les fichiers dont les noms contiennent zéro ou plusieurs caractères (`*`)
suivis de la lettre `t`, puis zéro ou plusieurs caractères (`*`) suivis de `ane.pdb`. Cela donne `ethane.pdb` `methane.pdb` `octane.pdb` `pentane.pdb`.

`2.` montre tous les fichiers dont les noms commencent par zéro ou plusieurs caractères (`*`) suivis de
la lettre `t`, puis un seul caractère (`?`), puis `ne.` suivi de zéro ou plusieurs caractères (`*`). Cela nous donnera `octane.pdb` et `pentane.pdb` mais ne correspondra à rien
qui se termine par `thane.pdb`.

`3.` corrige les problèmes de l'option 2 en correspondant à deux caractères (`??`) entre `t` et `ne`.
C'est la solution.

`4.` affiche uniquement les fichiers commençant par `ethane.`.

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

### Utilisation de caractères génériques pour accéder à plusieurs fichiers en même temps

:::::::::::::::::::::::::::::::::::::::::  callout

## Commentaires sur les caractères génériques

`*` correspond à 0 ou plusieurs caractères dans un nom de fichier.
Ainsi, `*tane.pdb` correspondra à `ethane.pdb`, `octane.pdb` et tous les fichiers finissant par `tane.pdb`.

`?` correspond à exactement 1 caractère.
Donc `?ethane.pdb` correspondrait à `methane.pdb` mais pas à `aethane.pdb`.

L'ordre des caractères génériques peut être important. Par exemple
`p*.pdb` correspondra à `pentane.pdb` et `propane.pdb`, `*p.pdb` correspondra à `prop.pdb` et `exp.pdb`.

Les caractères génériques peuvent être utilisés dans les noms de fichiers ou à la fin d'un nom de fichier.
Ainsi, `*p*.pdb` correspondrait à `prop.pdb`, `strep.pdb` et `exp.pdb`.

Les caractères génériques peuvent également être utilisés dans les sous-répertoires d'un répertoire.
Par exemple, `*/m?.pdb` correspondrait à `dira/mmpdb`, `dirb/mlpdb` et non à `dira/mypdb`.

Sachez que la mise en correspondance des cas est sensible. `*A.pdb` ne sera pas mis en correspondance avec `filea.pdb`.

FilEs, files, Files et FILEs sont tous des fichiers distincts.

D