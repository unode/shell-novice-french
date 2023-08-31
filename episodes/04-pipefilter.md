---
title: Pipes and Filters
teaching: 25
exercises: 10
---

::::::::::::::::::::::::::::::::::::::: objectifs

- Rediriger la sortie d'une commande vers un fichier.
- Construire des blocs de commande avec deux étapes ou plus.
- Expliquer ce qui se passe généralement si un programme ou un bloc de commande ne reçoit aucun fichier en entrée.
- Expliquer l'avantage de lier des commandes avec des pipes et des filtres.

::::::::::::::::::::::::::::::::::::::::::::::::::

...................................................................... questions

- Comment puis-je combiner des commandes existantes pour en faire de nouvelles choses ?

......................................................................

Maintenant que nous connaissons quelques commandes de base,
nous pouvons enfin nous pencher sur la fonctionnalité la plus puissante du shell :
la facilité avec laquelle il nous permet de combiner des programmes existants de nouvelles manières.
Nous commencerons avec le répertoire `shell-lesson-data/exercise-data/alkanes`
qui contient six fichiers décrivant certaines molécules organiques simples.
L'extension `.pdb` indique que ces fichiers sont au format Protein Data Bank,
un format de texte simple qui spécifie le type et la position de chaque atome dans la molécule.

```bash
$ ls
```

```output
cubane.pdb    methane.pdb    pentane.pdb
ethane.pdb    octane.pdb     propane.pdb
```

Exécutons une commande exemple :

```bash
$ wc cubane.pdb
```

```output
20  156 1158 cubane.pdb
```

`wc` est la commande 'word count' (compte de mots) :
elle compte le nombre de lignes, de mots et de caractères dans les fichiers (en retournant les valeurs
dans cet ordre, de gauche à droite).

Si nous exécutons la commande `wc *.pdb`, le `*` dans `*.pdb` correspond à zéro ou plusieurs caractères,
donc le shell transforme `*.pdb` en une liste de tous les fichiers `.pdb` du répertoire courant :

```bash
$ wc *.pdb
```

```output
  20  156  1158  cubane.pdb
  12  84   622   ethane.pdb
   9  57   422   methane.pdb
  30  246  1828  octane.pdb
  21  165  1226  pentane.pdb
  15  111  825   propane.pdb
 107  819  6081  total
```

Notez que `wc *.pdb` affiche également le nombre total de toutes les lignes dans la dernière ligne de la sortie.

Si nous exécutons `wc -l` à la place de simplement `wc`,
la sortie affiche uniquement le nombre de lignes par fichier :

```bash
$ wc -l *.pdb
```

```output
  20  cubane.pdb
  12  ethane.pdb
   9  methane.pdb
  30  octane.pdb
  21  pentane.pdb
  15  propane.pdb
 107  total
```

Les options `-m` et `-w` peuvent également être utilisées avec la commande `wc` pour afficher
seulement le nombre de caractères ou le nombre de mots, respectivement.

:::::::::::::::::::::::::::::::::::::::::  callout

## Pourquoi ne fait-il rien ?

Que se passe-t-il si une commande est censée traiter un fichier, mais que nous
ne lui donnons pas de nom de fichier ? Par exemple, que se passe-t-il si nous saisissons :

```bash
$ wc -l
```

mais ne saisissons pas `*.pdb` (ou autre chose) après la commande ? Comme il ne dispose d'aucun nom de fichier, `wc` suppose qu'il doit
traiter des données saisies à l'invite de commande, donc il se contente de rester là et d'attendre
que nous lui donnions des données de manière interactive. De l'extérieur, cependant, tout ce que nous
voyons, c'est qu'il est là, et la commande ne semble rien faire.

Si vous faites ce genre d'erreur, vous pouvez sortir de cet état en appuyant sur la touche de contrôle (<kbd>Ctrl</kbd>) et en appuyant une fois sur la lettre
<kbd>C</kbd> : <kbd>Ctrl</kbd>\+<kbd>C</kbd>. Ensuite, relâchez les deux touches.


::::::::::::::::::::::::::::::::::::::::::::::::::

## Capture de la sortie des commandes

Quel fichier contient le moins de lignes ?
C'est une question facile à répondre quand il n'y a que six fichiers,
mais qu'en serait-il s'il y en avait 6000 ?
Notre première étape vers une solution est d'exécuter la commande :

```bash
$ wc -l *.pdb > lengths.txt
```

Le symbole plus grand que, `>`, indique au shell de **rediriger** la sortie de la commande vers un
fichier au lieu de l'afficher à l'écran. Cette commande n'affiche aucune sortie à l'écran, car
tout ce que `wc` aurait affiché a été envoyé dans le fichier `lengths.txt` à la place.
Si le fichier n'existe pas avant d'exécuter la commande, le shell va créer le fichier.
Si le fichier existe déjà, il sera silencieusement écrasé, ce qui peut entraîner la perte de données.
Ainsi, **rediriger** les commandes nécessite de la prudence.

`ls lengths.txt` confirme l'existence du fichier :

```bash
$ ls lengths.txt
```

```output
lengths.txt
```

Nous pouvons maintenant envoyer le contenu de `lengths.txt` à l'écran en utilisant `cat lengths.txt`.
La commande `cat` tire son nom de 'concaténer', c'est-à-dire joindre ensemble,
et elle affiche le contenu des fichiers les uns après les autres.
Il n'y a qu'un seul fichier dans ce cas,
donc `cat` nous montre simplement ce qu'il contient :

```bash
$ cat lengths.txt
```

```output
  20  cubane.pdb
  12  ethane.pdb
   9  methane.pdb
  30  octane.pdb
  21  pentane.pdb
  15  propane.pdb
 107  total
```

:::::::::::::::::::::::::::::::::::::::::  callout

## Affichage page par page

Nous continuerons à utiliser `cat` dans cette leçon, pour la commodité et la cohérence,
mais il présente le désavantage de toujours afficher l'ensemble du fichier à l'écran.
Plus utile en pratique est la commande `less` (par exemple, `less lengths.txt`).
Cela affiche une page du fichier à l'écran, puis s'arrête.
Vous pouvez avancer d'une page en appuyant sur la barre d'espace,
ou reculer d'une page en appuyant sur `b`. Appuyez sur `q` pour quitter.


::::::::::::::::::::::::::::::::::::::::::::::::::

## Filtrage de la sortie

Ensuite, nous utiliserons la commande `sort` pour trier le contenu du fichier `lengths.txt`.
Mais d'abord, nous allons faire un exercice pour apprendre un peu plus sur la commande sort :

:::::::::::::::::::::::::::::::::::::::  challenge

## Que fait `sort -n` ?

Le fichier `shell-lesson-data/exercise-data/numbers.txt` contient les lignes suivantes :

```source
10
2
19
22
6
```

Si nous exécutons `sort` sur ce fichier, la sortie est :

```output
10
19
2
22
6
```

Si nous exécutons `sort -n` sur le même fichier, nous obtenons plutôt :

```output
2
6
10
19
22
```

Expliquez pourquoi `-n` a cet effet.

:::::::::::::::  solution

## Solution

L'option `-n` spécifie un tri numérique plutôt qu'un tri alphanumérique.



:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

Nous utiliserons également l'option `-n` pour spécifier que le tri est
numérique au lieu d'alphanumérique.
Ceci ne change pas le fichier;
à la place, il envoie le résultat trié à l'écran :

```bash
$ sort -n lengths.txt
```

```output
  9  methane.pdb
 12  ethane.pdb
 15  propane.pdb
 20  cubane.pdb
 21  pentane.pdb
 30  octane.pdb
107  total
```

Nous pouvons mettre la liste triée de lignes dans un autre fichier temporaire appelé `sorted-lengths.txt`
en ajoutant `> sorted-lengths.txt` après la commande,
tout comme nous avons utilisé `> lengths.txt` pour placer la sortie de `wc` dans `lengths.txt`.
Une fois que nous avons fait cela,
nous pouvons exécuter une autre commande appelée `head` pour obtenir les premières lignes de `sorted-lengths.txt` :

```bash
$ sort -n lengths.txt > sorted-lengths.txt
$ head -n 1 sorted-lengths.txt
```

```output
  9  methane.pdb
```

En utilisant `-n 1` avec `head`, nous lui disons que nous voulons seulement la première ligne du fichier ;
`-n 20` obtiendrait les 20 premières lignes,
et ainsi de suite.
Puisque `sorted-lengths.txt` contient les longueurs de nos fichiers ordonnées du moins au plus grand,
la sortie de `head` doit être le fichier avec le moins de lignes.

:::::::::::::::::::::::::::::::::::::::::  callout

## Redirection vers le même fichier

Il est très déconseillé d'essayer de rediriger 
la sortie d'une commande qui fonctionne sur un fichier
vers le même fichier. Par exemple :

```bash
$ sort -n lengths.txt > lengths.txt
```

Faire quelque chose comme cela peut vous donner
des résultats incorrects et/ou supprimer
le contenu de `lengths.txt`.


::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## Que signifie `>>` ?

Nous avons vu l'utilisation de `>`, mais il existe un opérateur similaire `>>`
qui fonctionne légèrement différemment.
Nous allons apprendre les différences entre ces deux opérateurs en imprimant quelques chaînes de caractères.
Nous pouvons utiliser la commande `echo` pour afficher des chaînes de caractères, par exemple :

```bash
$ echo La commande echo affiche du texte
```

```output
La commande echo affiche du texte
```

Testez maintenant les commandes ci-dessous pour connaître la différence entre les deux opérateurs :

```bash
$ echo bonjour > testfile01.txt
```

et :

```bash
$ echo bonjour >> testfile02.txt
```

Astuce : essayez d'exécuter chaque commande deux fois de suite, puis examinez les fichiers de sortie.

:::::::::::::::  solution

## Solution

Dans le premier exemple avec `>`, la chaîne 'bonjour' est écrite dans `testfile01.txt`,
mais le fichier est écrasé à chaque fois que nous exécutons la commande.

Nous voyons dans le deuxième exemple que l'opérateur `>>` écrit également 'bonjour' dans un fichier
(dans ce cas `testfile02.txt`),
mais ajoute la chaîne de caractères au fichier si celui-ci existe déjà
(c'est-à-dire lorsque nous l'exécutons pour la deuxième fois).



:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## Ajout de données

Nous avons déjà rencontré la commande `head`, qui affiche les lignes du début d'un fichier.
`tail` est similaire, mais affiche les lignes de fin d'un fichier à la place.

Considérez le fichier `shell-lesson-data/exercise-data/animal-counts/animals.csv`.
Après ces commandes, sélectionnez la réponse qui correspond au fichier `animals-subset.csv` :

```bash
$ head -n 3 animals.csv > animals-subset.csv
$ tail -n 2 animals.csv >> animals-subset.csv
```

1. Les trois premières lignes de `animals.csv`
2. Les deux dernières lignes de `animals.csv`
3. Les trois premières lignes et les deux dernières lignes de `animals.csv`
4. La deuxième et la troisième ligne de `animals.csv`

:::::::::::::::  solution

## Solution

L'option 3 est correcte.
Pour que l'option 1 soit correcte, nous exécuterions uniquement la commande `head`.
Pour que l'option 2 soit correcte, nous exécuterions uniquement la commande `tail`.
Pour que l'option 4 soit correcte, nous devrions transmettre la sortie de `head` à `tail -n 2`
en utilisant `head -n 3 animals.csv | tail -n 2 > animals-subset.csv`



:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

## Transmettre la sortie à une autre commande

Dans notre exemple de recherche du fichier avec le moins de lignes,
nous utilisons deux fichiers intermédiaires `lengths.txt` et `sorted-lengths.txt` pour stocker la sortie.
C'est une manière confuse de travailler car
même une fois que vous comprenez ce que font `wc`, `sort` et `head`,
ces fichiers intermédiaires rendent difficile de suivre ce qui se passe.
Nous pouvons rendre cela plus facile à comprendre en exécutant `sort` et `head` ensemble :

```bash
$ sort -n lengths.txt | head -n 1
```

```output
  9  methane.pdb
```

Le symbole vertical barre, `|`, entre les deux commandes est appelé un **pipe**.
Il indique au shell que nous voulons utiliser
la sortie de la commande de gauche
comme entrée de la commande de droite.

Cela a supprimé la nécessité du fichier `sorted-lengths.txt`.

## Combinaison de plusieurs commandes

Rien ne nous empêche de chaîner des tuyaux consécutivement.
Nous pouvons par exemple envoyer la sortie de `wc` directement dans `sort`,
et ensuite envoyer la sortie résultante dans `head`.
Cela supprime la nécessité de fichiers intermédiaires.

Nous commencerons par utiliser un pipe pour envoyer la sortie de `wc` à `sort` :

```bash
$ wc -l *.pdb | sort -n
```

```output
   9 methane.pdb
  12 ethane.pdb
  15 propane.pdb
  20 cubane.pdb
  21 pentane.pdb
  30 octane.pdb
 107 total
```

Nous pouvons ensuite envoyer cette sortie à travers un autre pipe, à `head`, de sorte que le pipeline complet devienne :

```bash
$ wc -l *.pdb | sort -n | head -n 1
```

```output
   9  methane.pdb
```

C'est exactement comme un mathématicien qui imbrique des fonctions comme *log(3x)*
et qui dit 'le logarithme de trois fois *x*'.
Dans notre cas,
l'algorithme est 'la tête du tri du compte de ligne de `*.pdb`'.

La redirection et les pipes utilisés dans les dernières commandes sont illustrés ci-dessous:

![](fig/redirects-and-pipes.svg){alt='Redirects and Pipes of different commands: "wc -l \*.pdb" will direct theoutput to the shell. "wc -l \*.pdb > lengths" will direct output to the file"lengths". "wc -l \*.pdb | sort -n | head -n 1" will build a pipeline where theoutput of the "wc" command is the input to the "sort" command, the output ofthe "sort" command is the input to the "head" command and the output of the"head" command is directed to the shell'}

:::::::::::::::::::::::::::::::::::::::  challenge

## Construction de tuyaux

Pour le fichier `animals.csv` de l'exercice précédent, considérez la commande suivante :

```bash
$ cut -d , -f 2 animals.csv
```

La commande `cut` est utilisée pour supprimer ou 'découper' certaines sections de chaque ligne du fichier,
et `cut` attend que les lignes soient séparées en colonnes par un caractère <kbd>Tab</kbd>.
Un caractère utilisé de cette manière est appelé un **délimiteur**.
Dans l'exemple ci-dessus, nous utilisons l'option `-d` pour spécifier la virgule comme caractère délimiteur.
Nous avons également utilisé l'option `-f` pour spécifier que nous voulons extraire le deuxième champ (colonne).
Cela donne la sortie suivante :

```output
deer
rabbit
raccoon
rabbit
deer
fox
rabbit
bear
```

La commande `uniq` filtre les lignes adjacentes identiques d'un fichier.
Comment pourriez-vous étendre ce pipeline (à l'aide de `uniq` et d'une autre commande) pour trouver
quels animaux le fichier contient (sans doublons dans leurs
noms) ?

:::::::::::::::  solution

## Solution

```bash
$ cut -d , -f 2 animals.csv | sort | uniq
```

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## Quel tuyau ?

Le fichier `animals.csv` contient 8 lignes de données au format suivant :

```output
2012-11-05,deer,5
2012-11-05,rabbit,22
2012-11-05,raccoon,7
2012-11-06,rabbit,19
...
```

La commande `uniq` dispose d'une option `-c` qui donne le nombre de fois où une ligne se produit dans son entrée.
En supposant que votre répertoire courant est `shell-lesson-data/exercise-data/animal-counts`,
quelle commande utiliseriez-vous pour produire un tableau qui montre
le nombre total de chaque type d'animal dans le fichier ?

1. `sort animals.csv | uniq -c`
2. `sort -t, -k2,2 animals.csv | uniq -c`
3. `cut -d, -f 2 animals.csv | uniq -c`
4. `cut -d, -f 2 animals.csv | sort | uniq -c`
5. `cut -d, -f 2 animals.csv | sort | uniq -c | wc -l`

:::::::::::::::  solution

## Solution

L'option 4. est la bonne réponse.
Si vous avez du mal à comprendre pourquoi, essayez d'exécuter les commandes, ou des sous-sections des
pipelines (assurez-vous d'être dans le répertoire `shell-lesson-data/exercise-data/animal-counts`).



:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

## Pipeline de Nelle : Vérification des fichiers

Nelle a exécuté ses échantillons à travers les machines d'analyse
et créé 17 fichiers dans le répertoire `north-pacific-gyre` décrit précédemment.
Pour une vérification rapide, à partir du répertoire `shell-lesson-data`, Nelle tape :

```bash
$ cd north-pacific-gyre
$ wc -l *.txt
```

La sortie est de 18 lignes qui ressemblent à cela :

```output
300 NENE01729A.txt
300 NENE01729B.txt
300 NENE01736A.txt
300 NENE01751A.txt
300 NENE01751B.txt
300 NENE01812A.txt
... ...
```

Ensuite, elle tape cela :

```bash
$ wc -l *.txt | sort -n | head -n 5
```

```output
 240 NENE02018B.txt
 300 NENE01729A.txt
 300 NENE01729B.txt
 300 NENE01736A.txt
 300 NENE01751A.txt
```

Oups : l'un des fichiers a 60 lignes de moins que les autres.
Lorsqu'elle vérifie à nouveau,
elle voit qu'elle a effectué cette analyse à 8h00 un lundi matin --- quelqu'un
a probablement utilisé la machine le week-end,
et elle a oublié de la remettre à zéro.
Avant de relancer cet échantillon,
elle vérifie si d'autres fichiers contiennent trop de données :

```bash
$ wc -l *.txt | sort -n | tail -n 5
```

```output
 300 NENE02040B.txt
 300 NENE02040Z.txt
 300 NENE02043A.txt
 300 NENE02043B.txt
5040 total
```

Ces chiffres semblent corrects --- mais qu'est-ce que ce 'Z' fait là, dans la troisième ligne en partant de la fin ?
Tous ses échantillons devraient être marqués 'A' ou 'B';
par convention,
son laboratoire utilise 'Z' pour indiquer les échantillons avec des informations manquantes.
Pour trouver les autres échantillons de ce type, elle fait ceci :

```bash
$ ls *Z.txt
```

```output
NENE01971Z.txt    NENE02040Z.txt
```

Bien sûr,
lorsqu'elle vérifie le journal sur son ordinateur portable,
il n'y a pas de profondeur enregistrée pour aucun de ces échantillons.
Puisqu'il est trop tard pour obtenir les informations de toute autre façon,
elle doit exclure ces deux fichiers de son analyse.
Elle pourrait les supprimer avec `rm`,
mais il y a en réalité des analyses qu'elle pourrait faire ultérieurement où la profondeur n'a pas d'importance,
elle devra donc simplement faire attention plus tard à sélectionner les fichiers en utilisant les expressions génériques
`NENE*A.txt NENE*B.txt`.

:::::::::::::::::::::::::::::::::::::::  challenge

## Suppression de fichiers inutiles

Supposons que vous voulez supprimer vos fichiers de données traités et ne conserver que
vos fichiers bruts et script de traitement pour économiser de l'espace de stockage.
Les fichiers bruts se terminent par `.dat` et les fichiers de données traités se terminent par `.txt`.
Lequel des éléments suivants supprimerait tous les fichiers de données traités,
et *uniquement* les fichiers de données traités ?

1. `rm ?.txt`
2. `rm *.txt`
3. `rm * .txt`
4. `rm *.*`

:::::::::::::::  solution

## Solution

1. Ceci supprimerait les fichiers `.txt` avec des noms d'un caractère
2. C'est la bonne réponse
3. Le shell étendrait `*` pour correspondre à tout dans le répertoire courant,
  donc la commande tenterait de supprimer tous les fichiers correspondants ainsi qu'un fichier supplémentaire appelé `.txt`
4. Le shell étend `*.*` pour correspondre à tous les noms de fichiers contenant au moins un
  `.`, y compris les fichiers traités (`.txt`) *et* les fichiers bruts (`.dat`)
  
  

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::



:::::::::::::::::::::::::::::::::::::::: keypoints

- `wc` compte les lignes, les mots et les caractères des entrées.
- `cat` affiche le contenu des entrées.
- `sort` trie les entrées.
- `head` affiche les dix premières lignes de l'entrée.
- `tail` affiche les dix dernières lignes de l'entrée.
- `commande > [fichier]` redirige la sortie d'une commande vers un fichier (en écrasant tout contenu existant).
- `commande >> [fichier]` ajoute la sortie d'une commande à un fichier.
- `[première] | [seconde]` est un pipeline : la sortie de la première commande est utilisée comme entrée de la seconde.
- La meilleure façon d'utiliser le shell est d'utiliser des pipes pour combiner des programmes simples à usage spécifique (filtres).

::::::::::::::::::::::::::::::::::::::::::::::::::