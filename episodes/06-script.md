---
title: Scripts Shell 
teaching: 30
exercises: 15
---

::::::::::::::::::::::::::::::::::::::: objectifs

- Écrire un script shell qui exécute une commande ou une série de commandes pour un ensemble fixe de fichiers.
- Exécuter un script shell à partir de la ligne de commande.
- Écrire un script shell qui fonctionne sur un ensemble de fichiers définis par l'utilisateur sur la ligne de commande.
- Créer des pipelines qui incluent des scripts shell que vous et d'autres avez écrits.

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- Comment puis-je enregistrer et réutiliser des commandes ?

::::::::::::::::::::::::::::::::::::::::::::::::::

Nous sommes enfin prêts à voir ce qui rend le shell si puissant en tant qu'environnement de programmation.
Nous allons prendre les commandes que nous répétons fréquemment et les sauvegarder dans des fichiers
afin de pouvoir réexécuter toutes ces opérations plus tard en tapant une seule commande.
Pour des raisons historiques,
un ensemble de commandes enregistrées dans un fichier est généralement appelé un **script shell**,
mais ne vous trompez pas - il s'agit en réalité de petits programmes.

Non seulement l'écriture de scripts shell accélérera votre travail, mais vous n'aurez plus à retaper
les mêmes commandes encore et encore. Cela les rendra également plus précis (moins de chances de faire des
erreurs de frappe) et plus reproductibles. Si vous revenez à votre travail plus tard (ou si quelqu'un d'autre trouve
votre travail et souhaite le poursuivre), vous pourrez reproduire les mêmes résultats simplement
en exécutant votre script, plutôt que de devoir vous souvenir ou retaper une longue liste de commandes.

Commençons par retourner à `alkanes/` et créer un nouveau fichier, `middle.sh` qui sera
notre script shell :

```bash
$ cd alkanes
$ nano middle.sh
```

La commande `nano middle.sh` ouvre le fichier `middle.sh` dans l'éditeur de texte 'nano'
(qui s'exécute dans le shell).
Si le fichier n'existe pas, il sera créé.
Nous pouvons utiliser l'éditeur de texte pour modifier directement le fichier en insérant la ligne suivante :

```source
head -n 15 octane.pdb | tail -n 5
```

Il s'agit d'une variation du pipeline que nous avons construit précédemment, qui sélectionne les lignes 11 à 15 du
fichier `octane.pdb`. Rappelez-vous, nous ne l'exécutons pas encore comme une commande ;
nous incorporons simplement les commandes dans un fichier.

Ensuite, nous enregistrons le fichier (`Ctrl-O` dans nano) et nous quittons l'éditeur de texte (`Ctrl-X` dans nano).
Vérifiez que le répertoire `alkanes` contient maintenant un fichier appelé `middle.sh`.

Une fois que nous avons enregistré le fichier,
nous pouvons demander au shell d'exécuter les commandes qu'il contient.
Notre shell s'appelle `bash`, donc nous exécutons la commande suivante :

```bash
$ bash middle.sh
```

```output
ATOM      9  H           1      -4.502   0.681   0.785  1.00  0.00
ATOM     10  H           1      -5.254  -0.243  -0.537  1.00  0.00
ATOM     11  H           1      -4.357   1.252  -0.895  1.00  0.00
ATOM     12  H           1      -3.009  -0.741  -1.467  1.00  0.00
ATOM     13  H           1      -3.172  -1.337   0.206  1.00  0.00
```

Effectivement,
la sortie de notre script est exactement ce que nous obtiendrions si nous exécutions ce pipeline directement.

:::::::::::::::::::::::::::::::::::::::::  callout

## Texte contre Autre

Nous appelons généralement les programmes comme Microsoft Word ou LibreOffice Writer des "éditeurs de texte",
mais nous devons être un peu plus prudents lorsqu'il s'agit de programmation. Par défaut, Microsoft Word utilise des fichiers `.docx` pour stocker non seulement le texte, mais aussi des informations de mise en forme sur les polices, les en-têtes, etc. Ces informations supplémentaires ne sont pas stockées sous forme de caractères et n'ont aucune signification pour des outils tels que `head`, qui attendent que les fichiers d'entrée ne contiennent rien d'autre que les lettres, les chiffres et la ponctuation d'un clavier d'ordinateur standard. Lorsque vous éditez des programmes, vous devez donc soit utiliser un éditeur de texte brut, soit veiller à enregistrer les fichiers au format texte brut.


::::::::::::::::::::::::::::::::::::::::::::::::::

Que se passe-t-il si nous voulons sélectionner des lignes à partir d'un fichier arbitraire ?
Nous pourrions modifier `middle.sh` à chaque fois pour changer le nom du fichier,
mais cela prendrait probablement plus de temps que de taper la commande à nouveau
dans le shell et de l'exécuter avec un nouveau nom de fichier.
À la place, nous allons modifier `middle.sh` pour le rendre plus polyvalent :

```bash
$ nano middle.sh
```

Maintenant, dans "nano", remplacez le texte `octane.pdb` par la variable spéciale appelée `$1` :

```source
head -n 15 "$1" | tail -n 5
```

Dans un script shell,
`$1` signifie 'le premier nom de fichier (ou autre argument) de la ligne de commande'.
Maintenant, nous pouvons exécuter notre script de cette manière :

```bash
$ bash middle.sh octane.pdb
```

```output
ATOM      9  H           1      -4.502   0.681   0.785  1.00  0.00
ATOM     10  H           1      -5.254  -0.243  -0.537  1.00  0.00
ATOM     11  H           1      -4.357   1.252  -0.895  1.00  0.00
ATOM     12  H           1      -3.009  -0.741  -1.467  1.00  0.00
ATOM     13  H           1      -3.172  -1.337   0.206  1.00  0.00
```

ou sur un autre fichier de cette façon :

```bash
$ bash middle.sh pentane.pdb
```

```output
ATOM      9  H           1       1.324   0.350  -1.332  1.00  0.00
ATOM     10  H           1       1.271   1.378   0.122  1.00  0.00
ATOM     11  H           1      -0.074  -0.384   1.288  1.00  0.00
ATOM     12  H           1      -0.048  -1.362  -0.205  1.00  0.00
ATOM     13  H           1      -1.183   0.500  -1.412  1.00  0.00
```

:::::::::::::::::::::::::::::::::::::::::  callout

## Guillemets autour des arguments

Pour la même raison pour laquelle nous avons mis la variable de boucle entre guillemets,
au cas où le nom de fichier contiendrait des espaces,
nous entourons `$1` de guillemets.

::::::::::::::::::::::::::::::::::::::::::::::::::

Actuellement, nous devons modifier `middle.sh` à chaque fois que nous voulons ajuster la plage de
lignes retournées.
Corrigeons cela en configurant notre script pour qu'il utilise plutôt trois arguments de ligne de commande.
Après le premier argument de ligne de commande (`$1`), chaque argument supplémentaire que nous
fournissons sera accessible via les variables spéciales `$1`, `$2`, `$3`,
qui se réfèrent respectivement au premier, deuxième et troisième argument de ligne de commande.

Sachant cela, nous pouvons utiliser des arguments supplémentaires pour définir la plage de lignes à
passer à `head` et `tail`, respectivement :

```bash
$ nano middle.sh
```

```source
head -n "$2" "$1" | tail -n "$3"
```

Nous pouvons maintenant exécuter :

```bash
$ bash middle.sh pentane.pdb 15 5
```

```output
ATOM      9  H           1       1.324   0.350  -1.332  1.00  0.00
ATOM     10  H           1       1.271   1.378   0.122  1.00  0.00
ATOM     11  H           1      -0.074  -0.384   1.288  1.00  0.00
ATOM     12  H           1      -0.048  -1.362  -0.205  1.00  0.00
ATOM     13  H           1      -1.183   0.500  -1.412  1.00  0.00
```

en changeant les arguments de notre commande, nous pouvons modifier le comportement de notre script :

```bash
$ bash middle.sh pentane.pdb 20 5
```

```output
ATOM     14  H           1      -1.259   1.420   0.112  1.00  0.00
ATOM     15  H           1      -2.608  -0.407   1.130  1.00  0.00
ATOM     16  H           1      -2.540  -1.303  -0.404  1.00  0.00
ATOM     17  H           1      -3.393   0.254  -0.321  1.00  0.00
TER      18              1
```

Cela fonctionne,
mais cela peut prendre un certain temps à la personne suivante qui lit `middle.sh` pour comprendre ce qu'il fait.
Nous pouvons améliorer notre script en ajoutant quelques **commentaires** en haut :

```bash
$ nano middle.sh
```

```source
# Sélectionne des lignes du milieu d'un fichier.
# Utilisation: bash middle.sh nom_fichier fin_ligne nombre_lignes
head -n "$2" "$1" | tail -n "$3"
```

Un commentaire commence par le caractère `#` et s'étend jusqu'à la fin de la ligne.
L'ordinateur ignore les commentaires,
mais ils sont précieux pour aider les personnes (y compris vous-même) à comprendre et à utiliser les scripts.
La seule mise en garde est que chaque fois que vous modifiez le script,
vous devez vérifier que le commentaire est toujours correct. Une explication qui envoie
le lecteur dans la mauvaise direction est pire que pas d'explication du tout.

Et si nous voulons traiter plusieurs fichiers dans un seul pipeline ?
Par exemple, si nous voulons trier nos fichiers `.pdb` par longueur, nous taperions :

```bash
$ wc -l *.pdb | sort -n
```

parce que `wc -l` liste le nombre de lignes dans les fichiers
(remarquez que `wc` signifie "word count", en ajoutant l'option `-l` cela signifie "compter les lignes" à la place)
et `sort -n` trie les choses numériquement.
Nous pourrions mettre cela dans un fichier,
mais alors il ne trierait toujours qu'une liste de fichiers `.pdb` dans le répertoire actuel.
Si nous voulons être en mesure d'obtenir une liste triée d'autres types de fichiers,
nous devons trouver un moyen d'obtenir tous ces noms dans le script.
Nous ne pouvons pas utiliser `$1`, `$2`, etc.
parce que nous ne connaissons pas le nombre de fichiers.
À la place, nous utilisons la variable spéciale `$@`,
qui signifie,
"Tous les arguments de ligne de commande pour le script shell".
Nous devons également mettre `$@` entre guillemets
pour gérer le cas des arguments contenant des espaces
(`"$@"` est une syntaxe spéciale et est équivalente à `"$1"` `"$2"` ...).

Voici un exemple :

```bash
$ nano sorted.sh
```

```source
# Trie les fichiers selon leur longueur.
# Utilisation: bash sorted.sh un_ou_plusieurs_noms_de_fichier
wc -l "$@" | sort -n
```

```bash
$ bash sorted.sh *.pdb ../creatures/*.dat
```

```output
9 methane.pdb
12 ethane.pdb
15 propane.pdb
20 cubane.pdb
21 pentane.pdb
30 octane.pdb
163 ../creatures/basilisk.dat
163 ../creatures/minotaur.dat
163 ../creatures/unicorn.dat
596 total
```

:::::::::::::::::::::::::::::::::::::::  challenge

## Liste d'espèces uniques

Leah a plusieurs centaines de fichiers de données, chacun étant formaté comme suit :

```source
2013-11-05,deer,5
2013-11-05,rabbit,22
2013-11-05,raccoon,7
2013-11-06,rabbit,19
2013-11-06,deer,2
2013-11-06,fox,1
2013-11-07,rabbit,18
2013-11-07,bear,1
```

Un exemple de ce type de fichier est donné dans
`shell-lesson-data/exercise-data/animal-counts/animals.csv`.

Nous pouvons utiliser la commande `cut -d , -f 2 animals.csv | sort | uniq` pour produire
la liste des espèces uniques dans `animals.csv`.
Afin de ne pas devoir taper cette série de commandes à chaque fois,
un scientifique peut choisir d'écrire un script shell à la place.

Écrivez un script shell appelé `species.sh` qui prend un nombre quelconque de
noms de fichiers en ligne de commande et utilise une variante de la commande ci-dessus
pour afficher une liste des espèces uniques apparaissant dans chacun de ces fichiers séparément.

:::::::::::::::  solution

## Solution

```bash
# Script pour trouver les espèces uniques dans les fichiers CSV où l'espèce est le deuxième champ de données
# Ce script accepte de un à plusieurs noms de fichiers comme arguments de ligne de commande

# Boucle sur tous les fichiers
for file in $@
do
    echo "Espèces uniques dans $file :"
    # Extraction des noms d'espèces
    cut -d , -f 2 $file | sort | uniq
done
```

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

Supposons que nous venons d'exécuter une série de commandes qui ont accompli quelque chose d'utile --- par exemple,
créer un graphique que nous aimerions utiliser dans un article.
Nous aimerions pouvoir recréer le graphique plus tard si nécessaire,
donc nous voulons enregistrer les commandes dans un fichier.
Au lieu de les saisir à nouveau
(et risquer de les taper incorrectement),
nous pouvons faire ceci :

```bash
$ history | tail -n 5 > refaire-figure-3.sh
```

Le fichier `refaire-figure-3.sh` contient maintenant :

```source
297 bash goostats.sh NENE01729B.txt stats-NENE01729B.txt
298 bash goodiff.sh stats-NENE01729B.txt /data/validated/01729.txt > 01729-differences.txt
299 cut -d ',' -f 2-3 01729-differences.txt > 01729-time-series.txt
300 ygraph --format scatter --color bw --borders none 01729-time-series.txt figure-3.png
301 history | tail -n 5 > redo-figure-3.sh
```

Après un peu de travail dans un éditeur pour supprimer les numéros de série des commandes,
et pour supprimer la dernière ligne où nous avons appelé la commande `history`,
nous disposons d'un enregistrement parfaitement précis de la façon dont nous avons créé ce graphique.

:::::::::::::::::::::::::::::::::::::::  challenge

## Pourquoi enregistrer les commandes dans l'historique avant de les exécuter ?

Si vous exécutez la commande :

```bash
$ history | tail -n 5 > recent.sh
```

la dernière commande dans le fichier est la commande `history` elle-même, c'est-à-dire
le shell a ajouté `history` au journal des commandes avant de l'exécuter réellement. En fait, le shell ajoute toujours les commandes au journal
avant de les exécuter. Pourquoi pensez-vous qu'il fait cela ?

:::::::::::::::  solution

## Solution

Si une commande provoque un problème de blocage ou de plantage, il peut être utile
de savoir quelle était cette commande, afin de pouvoir enquêter sur le problème. Si la commande était enregistrée uniquement après son exécution, nous n'aurions pas
d'enregistrement de la dernière commande exécutée en cas de plantage.



:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

En pratique, la plupart des gens développent des scripts shell en exécutant des commandes
à l'invite de commande quelques fois
pour être sûr de faire ce qu'ils veulent,
puis en les enregistrant dans un fichier pour une utilisation ultérieure.
Ce mode de travail permet aux gens de recycler
ce qu'ils découvrent sur leurs données et leur flux de travail avec un seul appel à `history`
et un peu de modification pour nettoyer la sortie
et l'enregistrer sous forme de script shell.

## Pipeline de Nelle : Créer un Script

Le superviseur de Nelle a insisté pour que toutes ses analyses soient reproductibles.
La manière la plus simple de capturer toutes les étapes est dans un script.

D'abord, nous revenons dans le répertoire du projet de Nelle :

```bash
$ cd ../../north-pacific-gyre/
```

Elle crée un fichier en utilisant `nano` ...

```bash
$ nano do-stats.sh
```

...qui contient ce qui suit :

```bash
# Calcul des statistiques pour les fichiers de données.
for datafile in "$@"
do
    echo $datafile
    bash goostats.sh $datafile stats-$datafile
done
```

Elle enregistre cela dans un fichier appelé `do-stats.sh`
de sorte qu'elle peut maintenant refaire la première étape de son analyse en tapant :

```bash
$ bash do-stats.sh NENE*A.txt NENE*B.txt
```

Elle peut également faire cela :

```bash
$ bash do-stats.sh NENE*A.txt NENE*B.txt | wc -l
```

de sorte que la sortie soit juste le nombre de fichiers traités
au lieu des noms des fichiers qui ont été traités.

Un point à noter sur le script de Nelle est que
elle laisse la personne qui l'exécute décider quels fichiers traiter.
Elle aurait pu l'écrire comme ceci :

```bash
# Calcul des statistiques pour les fichiers de données des sites A et B.
for datafile in NENE*A.txt NENE*B.txt
do
    echo $datafile
    bash goostats.sh $datafile stats-$datafile
done
```

L'avantage est que cela sélectionne toujours les bons fichiers :
elle n'a pas besoin de se rappeler d'exclure les fichiers 'Z'.
L'inconvénient est que cela *seulement* sélectionne ces fichiers - elle ne peut pas l'exécuter sur tous les fichiers
(y compris les fichiers 'Z'),
ou sur les fichiers 'G' ou 'H' que ses collègues en Antarctique produisent,
sans modifier le script.
Si elle voulait être plus audacieuse,
elle pourrait modifier son script pour vérifier les arguments de la ligne de commande,
et utiliser `NENE*A.txt NENE*B.txt` s'il n'y en a pas de fournis.
Bien sûr, cela introduit un autre compromis entre la flexibilité et la complexité.

:::::::::::::::::::::::::::::::::::::::  challenge

## Variables dans les Scripts Shell

Dans le répertoire `alkanes`, imaginez que vous avez un script shell appelé `script.sh` contenant les
commandes suivantes :

```bash
head -n $2 $1
tail -n $3 $1
```

Pendant que vous êtes dans le répertoire `alkanes`, vous tapez la commande suivante :

```bash
$ bash script.sh '*.pdb' 1 1
```

Parmi les sorties suivantes, à laquelle vous attendez-vous à voir ?

1. Toutes les lignes entre la première et la dernière ligne de chaque fichier se terminant par `.pdb` dans le répertoire `alkanes`
2. La première et la dernière ligne de chaque fichier se terminant par `.pdb` dans le répertoire `alkanes`
3. La première et la dernière ligne de chaque fichier dans le répertoire `alkanes`
4. Une erreur en raison des guillemets autour de `*.pdb`

:::::::::::::::  solution

## Solution

La bonne réponse est 2.

Les variables spéciales `$1`, `$2` et `$3` représentent les arguments de ligne de commande donnés au
script, de sorte que les commandes exécutées sont :

```bash
$ head -n 1 cubane.pdb ethane.pdb octane.pdb pentane.pdb propane.pdb
$ tail -n 1 cubane.pdb ethane.pdb octane.pdb pentane.pdb propane.pdb
```

Le shell n'élargit pas `'*.pdb'` car il est enfermé entre guillemets.
Ainsi, le premier argument du script est `'*.pdb'` qui est ensuite élargi dans le
script par `head` et `tail`.



:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## Trouver le fichier le plus long avec une extension donnée

Écrivez un script shell appelé `longest.sh` qui prend le nom d'un
répertoire et une extension de fichier en argument, et affiche
le nom du fichier ayant le plus de lignes dans ce répertoire
avec cette extension. Par exemple :

```bash
$ bash longest.sh shell-lesson-data/exercise-data/alkanes pdb
```

afficherait le nom du fichier `.pdb` dans `shell-lesson-data/exercise-data/alkanes` qui a
le plus de lignes.

N'hésitez pas à tester votre script sur un autre répertoire, par exemple :

```bash
$ bash longest.sh shell-lesson-data/exercise-data/writing txt
```

:::::::::::::::  solution

## Solution

```bash
# Script shell qui prend deux arguments :
#    1. un nom de répertoire
#    2. une extension de fichier
# et affiche le nom du fichier dans le répertoire
# qui a le plus de lignes et qui correspond à l'extension du fichier.

wc -l $1/*.$2 | sort -n | tail -n 2 | head -n 1
```

La première partie du pipeline, `wc -l $1/*.$2 | sort -n`, compte
le nombre de lignes dans chaque fichier et les trie numériquement (les plus grands en dernier). Lorsqu'il
y a plus d'un fichier, `wc` produit également une ligne de synthèse finale,
donnant le nombre total de lignes sur l'ensemble des fichiers. Nous utilisons `tail -n 2 | head -n 1` pour ignorer cette dernière ligne.

Avec `wc -l $1/*.$2 | sort -n | tail -n 1`, nous verrons la synthèse finale
ligne : nous pouvons construire notre pipeline progressivement pour être sûr de comprendre
la sortie.

:::::::::::::::::::::::::::::::::::::::::  challenge

## Compréhension de Scripts

Pour cette question, considérez à nouveau le répertoire `shell-lesson-data/exercise-data/alkanes`.
Celui-ci contient un certain nombre de fichiers `.pdb` en plus de tous les autres fichiers que vous
avez pu créer.
Expliquez ce que chacun des trois scripts suivants ferait lorsqu'ils sont exécutés comme
`bash script1.sh *.pdb`, `bash script2.sh *.pdb` et `bash script3.sh *.pdb` respectivement.

```bash
# Script 1
echo *.*
```

```bash
# Script 2
for filename in $1 $2 $3
do
    cat $filename
done
```

```bash
# Script 3
echo $@.pdb
```

:::::::::::::::  solution

## Solutions

Dans chaque cas, le shell développe le caractère générique dans `*.pdb` avant de passer la liste résultante de noms de fichiers en tant qu'arguments au script.

Le script 1 imprimerait une liste de tous les fichiers contenant un point dans leur nom.
Les arguments transmis au script ne sont en fait utilisés nulle part dans le script.

Le script 2 imprimerait le contenu des 3 premiers fichiers avec l'extension de fichier `.pdb`.
`$1`, `$2` et `$3` se réfèrent respectivement au premier, deuxième et troisième argument.

Le script 3 afficherait tous les arguments du script (c'est-à-dire tous les fichiers `.pdb`),
suivis de `.pdb`.
`$@` se réfère à *tous* les arguments donnés à un script shell.

```output
cubane.pdb ethane.pdb methane.pdb octane.pdb pentane.pdb propane.pdb.pdb
```

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## Déboguer des Scripts

Supposons que vous ayez enregistré le script suivant dans un fichier appelé `do-errors.sh`
dans le répertoire `north-pacific-gyre` de Nelle :

```bash
# Calculate stats for data files.
for datafile in "$@"
do
    echo $datfile
    bash goostats.sh $datafile stats-$datafile
done
```

Lorsque vous l'exécutez à partir du répertoire `north-pacific-gyre` :

```bash
$ bash do-errors.sh NENE*A.txt NENE*B.txt
```

la sortie est vide.
Pour comprendre pourquoi, réexécutez le script en utilisant l'option `-x` :

```bash
$ bash -x do-errors.sh NENE*A.txt NENE*B.txt
```

Que vous montre la sortie ?
Quelle ligne est responsable de l'erreur ?

:::::::::::::::  solution

## Solution

L'option `-x` permet à `bash` de s'exécuter en mode de débogage.
Cela affiche chaque commande au fur et à mesure de son exécution, ce qui vous aidera à localiser les erreurs.
Dans cet exemple, nous pouvons voir que `echo` n'imprime rien. Nous avons fait une faute de frappe
dans le nom de la variable de la boucle, et la variable `datfile` n'existe pas, ce qui renvoie donc
une chaîne vide.



:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::



:::::::::::::::::::::::::::::::::::::::: keypoints

- Enregistrez les commandes dans des fichiers (généralement appelés scripts shell) pour les réutiliser.
- `bash [nom_fichier]` exécute les commandes enregistrées dans un fichier.
- `$@` fait référence à tous les arguments de la ligne de commande d'un script shell.
- `$1`, `$2`, etc., font référence au premier argument de la ligne de commande, au deuxième argument de la ligne de commande, etc.
- Placez les variables entre guillemets si les valeurs peuvent contenir des espaces.
- Permettre aux utilisateurs de décider quels fichiers traiter est plus flexible et plus cohérent avec les commandes Unix intégrées.

----------------------------

