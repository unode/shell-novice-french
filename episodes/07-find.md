---
title: Trouver des choses
teaching: 25
exercises: 20
---

::::::::::::::::::::::::::::::::::::::: objectifs

- Utiliser `grep` pour sélectionner des lignes dans des fichiers texte correspondant à des motifs simples.
- Utiliser `find` pour trouver des fichiers et des répertoires dont les noms correspondent à des motifs simples.
- Utiliser la sortie d'une commande comme argument(s) en ligne de commande pour une autre commande.
- Expliquer ce qui est entendu par «fichiers texte» et «fichiers binaires», et pourquoi de nombreux outils courants ne gèrent pas bien ces derniers.

::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- Comment puis-je trouver des fichiers ?
- Comment puis-je trouver des choses dans des fichiers ?

::::::::::::::::::::::::::::::::::::::::::::::

De la même manière que beaucoup d'entre nous utilisent maintenant «Google» comme un verbe signifiant «trouver», les programmeurs Unix utilisent souvent le mot «grep». «grep» est une contraction de «recherche / expression régulière / imprimer», une séquence courante d'opérations dans les éditeurs de texte Unix primitifs. C'est également le nom d'un programme en ligne de commande très utile.

`grep` trouve et imprime les lignes dans les fichiers qui correspondent à un motif. Pour nos exemples, nous utiliserons un fichier contenant trois haikus tirés d'un concours de 1998 dans le magazine *Salon* (Crédits aux auteurs Bill Torcaso, Howard Korder et Margaret Segall. Voir [Page 1](https://web.archive.org/web/20000310061355/http://www.salon.com/21st/chal/1998/02/10chal2.html) et [Page 2](https://web.archive.org/web/20000229135138/http://www.salon.com/21st/chal/1998/02/10chal3.html) en archive.). Pour cet ensemble d'exemples, nous allons travailler dans le sous-répertoire `writing` :

```bash
$ cd
$ cd Desktop/shell-lesson-data/exercise-data/writing
$ cat haiku.txt
```

```output
The Tao that is seen
Is not the true Tao, until
You bring fresh toner.

With searching comes loss
and the presence of absence:
"My Thesis" not found.

Yesterday it worked
Today it is not working
Software is like that.
```

Trouvons les lignes qui contiennent le mot `not` :

```bash
$ grep not haiku.txt
```

```output
Is not the true Tao, until
"My Thesis" not found
Today it is not working
```

Ici, `not` est le motif que nous recherchons. La commande grep recherche dans le fichier, à la recherche de correspondances avec le motif spécifié. Pour l'utiliser, tapez `grep`, puis le motif que nous recherchons et enfin le nom du fichier (ou des fichiers) dans lesquels nous recherchons.

La sortie est les trois lignes du fichier qui contiennent les lettres `not`.

Par défaut, `grep` recherche un motif de manière sensible à la casse. De plus, le motif de recherche que nous avons sélectionné n'a pas besoin de former un mot complet, comme nous le verrons dans l'exemple suivant.

Recherchons le motif : `The`.

```bash
$ grep The haiku.txt
```

```output
The Tao that is seen
"My Thesis" not found.
```

Cette fois, deux lignes qui incluent les lettres `The` sont affichées, dont une contenait notre motif de recherche dans un mot plus long, `Thesis`.

Pour limiter les correspondances aux lignes contenant le mot `The` seul, nous pouvons utiliser l'option `-w` de `grep`. cela limitera les correspondances aux limites des mots.

Plus tard dans cette leçon, nous verrons également comment nous pouvons modifier le comportement de recherche de `grep` en ce qui concerne sa sensibilité à la casse.

```bash
$ grep -w The haiku.txt
```

```output
The Tao that is seen
```

Notez qu'une «limite de mot» comprend le début et la fin d'une ligne, donc pas seulement des lettres entourées d'espaces. Parfois, nous ne voulons pas rechercher un seul mot, mais une phrase. Nous pouvons également le faire avec `grep` en mettant la phrase entre guillemets.

```bash
$ grep -w "is not" haiku.txt
```

```output
Today it is not working
```

Nous avons maintenant vu que vous n'avez pas besoin de mettre des guillemets autour des mots simples, mais il est utile d'utiliser des guillemets lors de la recherche de plusieurs mots. Cela aide également à faciliter la distinction entre le terme ou la phrase recherché et le fichier recherché. Nous utiliserons des guillemets dans les exemples suivants.

Une autre option utile est `-n`, qui numérote les lignes qui correspondent :

```bash
$ grep -n "it" haiku.txt
```

```output
5:With searching comes loss
9:Yesterday it worked
10:Today it is not working
```

Ici, nous pouvons voir que les lignes 5, 9 et 10 contiennent les lettres `it`.

Nous pouvons combiner les options (ou drapeaux) comme nous le faisons avec d'autres commandes Unix. Par exemple, trouvons les lignes qui contiennent le mot `the`. Nous pouvons combiner l'option `-w` pour trouver les lignes qui contiennent le mot `the` et `-n` pour numéroter les lignes qui correspondent :

```bash
$ grep -n -w "the" haiku.txt
```

```output
2:Is not the true Tao, until
6:and the presence of absence:
```

Maintenant, nous voulons utiliser l'option `-i` pour rendre notre recherche insensible à la casse :

```bash
$ grep -n -w -i "the" haiku.txt
```

```output
1:The Tao that is seen
2:Is not the true Tao, until
6:and the presence of absence:
```

Maintenant, nous voulons utiliser l'option `-v` pour inverser notre recherche, c'est-à-dire que nous voulons afficher les lignes qui ne contiennent pas le mot `the`.

```bash
$ grep -n -w -v "the" haiku.txt
```

```output
1:The Tao that is seen
3:You bring fresh toner.
4:
5:With searching comes loss
7:"My Thesis" not found.
8:
9:Yesterday it worked
10:Today it is not working
11:Software is like that.
```

Si nous utilisons l'option `-r` (récursive), `grep` peut rechercher un motif de manière récursive à travers un ensemble de fichiers dans les sous-répertoires.

Recherchons récursivement `Yesterday` dans le répertoire `shell-lesson-data/exercise-data/writing` :

```bash
$ grep -r Yesterday .
```

```output
./LittleWomen.txt:"Yesterday, when Aunt was asleep and I was trying to be as still as a
./LittleWomen.txt:Yesterday at dinner, when an Austrian officer stared at us and then
./LittleWomen.txt:Yesterday was a quiet day spent in teaching, sewing, and writing in my
./haiku.txt:Yesterday it worked
```

`grep` dispose de nombreuses autres options. Pour savoir lesquelles, nous pouvons taper :

```bash
$ grep --help
```

```output
Usage: grep [OPTION]... PATTERN [FILE]...
Search for PATTERN in each FILE or standard input.
PATTERN is, by default, a basic regular expression (BRE).
Example: grep -i 'hello world' menu.h main.c

Regexp selection and interpretation:
  -E, --extended-regexp     PATTERN is an extended regular expression (ERE)
  -F, --fixed-strings       PATTERN is a set of newline-separated fixed strings
  -G, --basic-regexp        PATTERN is a basic regular expression (BRE)
  -P, --perl-regexp         PATTERN is a Perl regular expression
  -e, --regexp=PATTERN      use PATTERN for matching
  -f, --file=FILE           obtain PATTERN from FILE
  -i, --ignore-case         ignore case distinctions
  -w, --word-regexp         force PATTERN to match only whole words
  -x, --line-regexp         force PATTERN to match only whole lines
  -z, --null-data           a data line ends in 0 byte, not newline

Miscellaneous:
...        ...        ...
```

:::::::::::::::::::::::::::::::::::::::::  challenge

## Utiliser `grep`

Quelle commande donnerait le résultat suivant ?

```output
and the presence of absence:
```

1. `grep "of" haiku.txt`
2. `grep -E "of" haiku.txt`
3. `grep -w "of" haiku.txt`
4. `grep -i "of" haiku.txt`

:::::::::::::::  solution

## Solution

La réponse correcte est 3, parce que l'option `-w` ne cherche que les correspondances de mots entiers. Les autres options trouveront également 'of' lorsqu'il fait partie d'un autre mot.



:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## Suivre une espèce

Leah a plusieurs centaines de fichiers de données enregistrés dans un seul répertoire, chacun étant formaté comme suit :

```source
2012-11-05,deer,5
2012-11-05,rabbit,22
2012-11-05,raccoon,7
2012-11-06,rabbit,19
2012-11-06,deer,2
2012-11-06,fox,4
2012-11-07,rabbit,16
2012-11-07,bear,1
```

Elle souhaite écrire un script shell qui prend une espèce comme premier argument en ligne de commande et un répertoire comme second argument. Le script devrait renvoyer un fichier appelé `<espèce>.txt` contenant une liste de dates et du nombre de cette espèce observée à chaque date. Par exemple, en utilisant les données indiquées ci-dessus, `rabbit.txt` contiendrait :

```source
2012-11-05,22
2012-11-06,19
2012-11-07,16
```

Ci-dessous, chaque ligne contient une commande individuelle, ou conduit. Organisez leur séquence dans une commande pour atteindre l'objectif de Leah :

```bash
cut -d : -f 2
>
|
grep -w $1 -r $2
|
$1.txt
cut -d , -f 1,3
```

Indice : utilisez `man grep` pour rechercher comment rechercher du texte de manière récursive dans un répertoire et `man cut` pour sélectionner plus d'un champ dans une ligne.

Un exemple de ce type de fichier est fourni dans `shell-lesson-data/exercise-data/animal-counts/animals.csv`

:::::::::::::::  solution

## Solution

```bash
grep -w $1 -r $2 | cut -d : -f 2 | cut -d , -f 1,3 > $1.txt
```

En fait, vous pouvez inverser l'ordre des deux commandes `cut` et cela fonctionne toujours. À la ligne de commande, essayez de changer l'ordre des commandes `cut`, et regardez la sortie de chaque étape pour comprendre pourquoi cela fonctionne.

Vous appelleriez le script ci-dessus comme ceci :

```bash
$ bash count-species.sh bear .
```

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## Les petites femmes

Vous et votre ami, après avoir fini de lire *Little Women* de Louisa May Alcott, êtes en désaccord. Parmi les quatre soeurs du livre, Jo, Meg, Beth et Amy, votre ami pense que Jo a été la plus mentionnée. Vous êtes pourtant certain(e) que c'était Amy. Heureusement, vous avez un fichier `LittleWomen.txt` contenant le texte intégral du roman (`shell-lesson-data/exercise-data/writing/LittleWomen.txt`). À l'aide d'une boucle `for`, comment pouvez-vous tabuler le nombre de fois que chacune des quatre soeurs est mentionnée ?

Indice : une solution peut utiliser les commandes `grep` et `wc` et un `|`, tandis qu'une autre peut utiliser les options de `grep`.


:::::::::::::::  solution

## Solution

```bash
for sis in Jo Meg Beth Amy
do
    echo $sis:
    grep -ow $sis LittleWomen.txt | wc -l
done
```

Une autre solution, légèrement inférieure :

```bash
for sis in Jo Meg Beth Amy
do
    echo $sis:
    grep -ocw $sis LittleWomen.txt
done
```

Cette solution est inférieure car `grep -c` ne rapporte que le nombre de lignes correspondantes. Le nombre total de correspondances rapporté par cette méthode sera inférieur s'il y a plus d'une correspondance par ligne.

Les observateurs perspicaces ont peut-être remarqué que les noms de personnages apparaissent parfois en majuscules dans les titres des chapitres (par exemple, "MEG GOES TO VANITY FAIR"). Si vous voulez également les compter, vous pouvez ajouter l'option `-i` pour la prise en compte de la casse (bien que, dans ce cas, cela n'affecte pas la réponse à laquelle des sœurs a été mentionnée le plus fréquemment).



:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

Bien que `grep` trouve des lignes dans des fichiers, la commande `find` trouve les fichiers eux-mêmes. Encore une fois, elle dispose de nombreuses options ; pour montrer comment les plus simples fonctionnent, nous utiliserons l'arborescence suivante du répertoire `shell-lesson-data/exercise-data`.

```output
.
├── animal-counts/
│   └── animals.csv
├── creatures/
│   ├── basilisk.dat
│   ├── minotaur.dat
│   └── unicorn.dat
├── numbers.txt
├── alkanes/
│   ├── cubane.pdb
│   ├── ethane.pdb
│   ├── methane.pdb
│   ├── octane.pdb
│   ├── pentane.pdb
│   └── propane.pdb
└── writing/
    ├── haiku.txt
    └── LittleWomen.txt
```

Le répertoire `exercise-data` contient un fichier, `numbers.txt`, et quatre répertoires `animal-counts`, `creatures`, `alkanes` et `writing` contenant divers fichiers.

Pour notre première commande, lançons `find .` (n'oubliez pas de lancer cette commande à partir du répertoire `shell-lesson-data/exercise-data`).

```bash
$ find .
```

```output
.
./writing
./writing/LittleWomen.txt
./writing/haiku.txt
./creatures
./creatures/basilisk.dat
./creatures/unicorn.dat
./creatures/minotaur.dat
./animal-counts
./animal-counts/animals.csv
./numbers.txt
./alkanes
./alkanes/ethane.pdb
./alkanes/propane.pdb
./alkanes/octane.pdb
./alkanes/pentane.pdb
./alkanes/methane.pdb
./alkanes/cubane.pdb
```

Comme toujours, `.` à lui seul signifie le répertoire de travail actuel, là où nous voulions que notre recherche commence. La sortie de `find` est le nom de chaque fichier **et** répertoire dans le répertoire de travail actuel. Cela peut sembler inutile au premier abord, mais `find` dispose de nombreuses options pour filtrer la sortie et dans cette leçon, nous en découvrirons certaines.

La première option de notre liste est `-type d`, ce qui signifie « les choses qui sont des répertoires ». Nous obtenons bien les cinq répertoires (y compris `.`) :

```bash
$ find . -type d
```

```output
.
./writing
./creatures
./animal-counts
./alkanes
```

Remarquez que les objets que `find` trouve ne sont pas répertoriés dans un ordre particulier. Si nous changeons `-type d` en `-type f`, nous obtenons une liste de tous les fichiers :

```bash
$ find . -type f
```

```output
./writing/LittleWomen.txt
./writing/haiku.txt
./creatures/basilisk.dat
./creatures/unicorn.dat
./creatures/minotaur.dat
./animal-counts/animals.csv
./numbers.txt
./alkanes/ethane.pdb
./alkanes/propane.pdb
./alkanes/octane.pdb
./alkanes/pentane.pdb
./alkanes/methane.pdb
./alkanes/cubane.pdb
```

Maintenant, essayons de faire correspondre par nom :

```bash
$ find . -name *.txt
```

```output
./numbers.txt
```

Nous nous attendions à trouver tous les fichiers texte, mais il imprime uniquement `./numbers.txt`. Le problème est que le shell développe les caractères génériques comme `*` *avant* d'exécuter les commandes. Étant donné que `*.txt` dans le répertoire actuel se développe en `./numbers.txt`, la commande que nous avons en réalité exécutée était :

```bash
$ find . -name numbers.txt
```

`find` a fait ce que nous avons demandé ; nous avons juste demandé le mauvais élément.

Pour obtenir ce que nous voulons, faisons ce que nous avons fait avec `grep` : mettons `*.txt` entre guillemets pour empêcher le shell de développer le caractère générique `*`. Ainsi, `find` obtient réellement le motif `*.txt`, pas le nom de fichier étendu `numbers.txt` :

```bash
$ find . -name "*.txt"
```

```output
./writing/LittleWomen.txt
./writing/haiku.txt
./numbers.txt
```

:::::::::::::::::::::::::::::::::::::::::  callout

## Liste vs Recherche

`ls` et `find` peuvent être utilisés pour effectuer des opérations similaires avec les bonnes options, mais dans des circonstances normales, `ls` répertorie tout ce qu'il peut, tandis que `find` cherche des éléments ayant des propriétés particulières et les affiche.


::::::::::::::::::::::::::::::::::::::::::::::::::

Comme nous l'avons dit précédemment, la puissance de la ligne de commande réside dans la combinaison d'outils. Nous avons vu comment le faire avec des tubes ; regardons une autre technique. Comme nous venons de le voir, `find . -name "*.txt"` nous donne une liste de tous les fichiers texte dans ou sous le répertoire de travail actuel. Comment pouvons-nous combiner cela avec `wc -l` pour compter les lignes dans tous ces fichiers ?

La manière la plus simple consiste à mettre la commande `find` à l'intérieur de `$()` :

```bash
$ wc -l $(find . -name "*.txt")
```

```output
  21022 ./writing/LittleWomen.txt
     11 ./writing/haiku.txt
      5 ./numbers.txt
  21038 total
```

Lorsque le shell exécute cette commande, la première chose qu'il fait est d'exécuter ce qui se trouve à l'intérieur de `$()`. Ensuite, il remplace l'expression `$()` par la sortie de cette commande. Étant donné que la sortie de `find` est constituée des trois noms de fichiers `./writing/LittleWomen.txt`, `./writing/haiku.txt` et `./numbers.txt`, le shell construit la commande :

```bash
$ wc -l ./writing/LittleWomen.txt ./writing/haiku.txt ./numbers.txt
```

ce que nous voulions. Cette expansion est exactement ce que fait le shell lorsqu'il développe des caractères génériques tels que `*` et `?`, mais nous permet d'utiliser n'importe quelle commande que nous souhaitons comme notre propre «caractère générique».

Il est très courant d'utiliser `find` et `grep` ensemble. Le premier trouve des fichiers qui correspondent à un motif ; le second recherche des lignes à l'intérieur de ces fichiers qui correspondent à un autre motif. Ici, par exemple, nous pouvons trouver des fichiers txt qui contiennent le mot "searching" en recherchant la chaîne 'searching' dans tous les fichiers `.txt` du répertoire de travail actuel :

```bash
$ grep "searching" $(find . -name "*.txt")
```

```output
./writing/LittleWomen.txt:sitting on the top step, affected to be searching for her book, but was
./writing/haiku.txt:With searching comes loss
```

:::::::::::::::::::::::::::::::::::::::  challenge

## Correspondance et soustraction

L'option `-v` de `grep` inverse la correspondance des motifs, de sorte que seules les lignes qui ne correspondent pas au motif sont affichées. Sachant cela, quelle commande parmi les suivantes trouvera tous les fichiers `.dat` dans `creatures` sauf `unicorn.dat` ?
Une fois que vous avez réfléchi à votre réponse, vous pouvez tester les commandes dans le répertoire `shell-lesson-data/exercise-data`.

1. `find creatures -name "*.dat" | grep -v unicorn`
2. `find creatures -name *.dat | grep -v unicorn`
3. `grep -v "unicorn" $(find creatures -name "*.dat")`
4. Aucune des réponses précédentes.

:::::::::::::::  solution

## Solution

L'option 1 est correcte. Mettre l'expression de correspondance entre guillemets empêche le shell de l'expanser, il est donc transmis à la commande `find`.

L'option 2 fonctionne également dans ce cas car le shell tente d'étendre `*.dat`, mais il n'y a pas de fichiers `*.dat` dans le répertoire actuel, donc l'expression générique est transmise à `find`. Nous avons rencontré cela pour la première fois dans [l'épisode 3](03-create.md).

L'option 3 est incorrecte car elle recherche le contenu des fichiers pour les lignes qui ne correspondent pas à 'unicorn', plutôt que de rechercher les noms de fichiers.



:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## Little Women

Vous et votre ami, après avoir fini de lire *Les Quatre Filles du docteur March* de Louisa May Alcott, êtes en désaccord. Parmi les quatre sœurs du livre, Jo, Meg, Beth et Amy, votre ami pense que Jo a été la plus mentionnée. Vous êtes pourtant certain(e) que c'était Amy. Heureusement, vous avez un fichier `LittleWomen.txt` contenant le texte intégral du roman (`shell-lesson-data/exercise-data/writing/LittleWomen.txt`). Utilisant une boucle `for`, comment pouvez-vous tabuler le nombre de fois où chacune des quatres soeurs est mentionnée ?

Indice : une solution pourrait utiliser les commandes `grep` et `wc` et un `|`, tandis qu'une autre pourrait utiliser les options de `grep`.

:::::::::::::::  solution

## Solution

```bash
for sis in Jo Meg Beth Amy
do
    echo $sis:
    grep -ow $sis LittleWomen.txt | wc -l
done
```

Une autre solution, légèrement inférieure :

```bash
for sis in Jo Meg Beth Amy
do
    echo $sis:
    grep -ocw $sis LittleWomen.txt
done
```

Cette solution est inférieure car `grep -c` ne rapporte que le nombre de lignes correspondantes. Le nombre total de correspondances rapporté par cette méthode sera inférieur s'il y a plus d'une correspondance par ligne.

Les observateurs perspicaces ont peut-être remarqué que les noms de personnages parfois apparaissent en majuscules dans les titres des chapitres (par exemple, 'MEG GOES TO VANITY FAIR'). Si vous voulez les compter aussi, vous pouvez ajouter l'option `-i` pour prendre en compte la casse (bien que, dans ce cas, cela n'affecte pas la réponse à laquelle des sœurs a été mentionnée le plus fréquemment).



:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

La shell Unix est plus ancienne que la plupart des personnes qui l'utilisent. Elle a survécu si longtemps parce que c'est l'un des environnements de programmation les plus productifs jamais créés - peut-être même le plus productif. Sa syntaxe peut sembler cryptique, mais les personnes qui la maîtrisent peuvent expérimenter différentes commandes de manière interactive, puis utiliser ce qu'elles ont appris pour automatiser leur travail. Les interfaces graphiques sont peut-être plus faciles à utiliser au départ, mais une fois maîtrisée, la productivité dans la ligne de commande est imbattable. Et comme l'a écrit Alfred North Whitehead en 1911, «La civilisation progresse en étendant le nombre d'opérations importantes que nous pouvons effectuer sans y réfléchir».

:::::::::::::::::::::::::::::::::::::::  challenge

## Compréhension de la lecture de la pipeline `find`

Rédigez un court commentaire explicatif sur le script shell suivant :

```bash
wc -l $(find . -name "*.dat") | sort -n
```

:::::::::::::::  solution

## Solution

1. Trouver tous les fichiers avec l'extension `.dat` de manière récursive à partir du répertoire actuel
2. Compter le nombre de lignes contenues dans chacun de ces fichiers
3. Trier la sortie de l'étape 2 numériquement