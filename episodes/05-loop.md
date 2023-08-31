---
title: Boucles
teaching: 40
exercises: 10
---

::::::::::::::::::::::::::::::::::::::: objectifs

- Écrire une boucle qui applique une ou plusieurs commandes distinctement à chaque fichier d'un ensemble de fichiers.
- Retracer les valeurs prises par une variable de boucle pendant l'exécution de la boucle.
- Expliquer la différence entre le nom d'une variable et sa valeur.
- Expliquer pourquoi les espaces et certains caractères de ponctuation ne doivent pas être utilisés dans les noms de fichiers.
- Démontrer comment voir quelles commandes ont récemment été exécutées.
- Réexécutez les commandes récemment exécutées sans les retaper.

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- Comment puis-je effectuer les mêmes actions sur de nombreux fichiers différents?

::::::::::::::::::::::::::::::::::::::::::::::::::

**Les boucles** sont une construction de programmation qui nous permet de répéter une commande ou un ensemble de commandes pour chaque élément d'une liste.
En tant que telles, elles sont essentielles pour améliorer la productivité grâce à l'automatisation.
Tout comme les caractères génériques et la complétion tabulaire, l'utilisation de boucles réduit également la quantité de saisie requise (et donc le nombre d'erreurs de saisie).

Supposons que nous avons plusieurs centaines de fichiers de données génomiques nommés `basilisk.dat`, `minotaur.dat` et `unicorn.dat`.
Pour cet exemple, nous utiliserons le répertoire `exercise-data/creatures` qui ne contient que trois fichiers d'exemple,
mais les principes peuvent être appliqués à beaucoup plus de fichiers à la fois.

La structure de ces fichiers est la même: le nom commun, la classification et la date de mise à jour sont présentés sur les trois premières lignes, avec des séquences d'ADN sur les lignes suivantes.
Jetons un coup d'œil aux fichiers:

```bash
$ head -n 5 basilisk.dat minotaur.dat unicorn.dat
```

Nous aimerions imprimer la classification de chaque espèce, qui est donnée sur la deuxième ligne de chaque fichier.
Pour chaque fichier, nous devons exécuter la commande `head -n 2` et la renvoyer à `tail -n 1`.
Nous utiliserons une boucle pour résoudre ce problème, mais regardons d'abord la forme générale d'une boucle,
en utilisant le pseudo-code ci-dessous:

```bash
# Le mot "for" indique le début d'une commande "for-loop"
for élément dans liste_d'éléments 
# Le mot "do" indique le début de l'exécution de la tâche
faire 
    # L'indentation à l'intérieur de la boucle n'est pas nécessaire, mais facilite la lisibilité
    opération_utilisant/commande $élément
# Le mot "done" indique la fin d'une boucle
terminé  
```

et nous pouvons appliquer cela à notre exemple de cette façon:

```bash
$ for fichier in basilisk.dat minotaur.dat unicorn.dat
> faire
>     echo $fichier
>     head -n 2 $fichier | tail -n 1
> terminé
```

```output
basilisk.dat
CLASSIFICATION: basiliscus vulgaris
minotaur.dat
CLASSIFICATION: bos hominus
unicorn.dat
CLASSIFICATION: equus monoceros
```

:::::::::::::::::::::::::::::::::::::::::  callout

## Suivre l'instruction

L'invite de commande shell passe de `$` à `>` et vice versa pendant que nous saisissons notre boucle. La deuxième invite de commande, `>`, est différente pour nous rappeler que nous n'avons pas encore fini de taper une commande complète. Un point-virgule, `;`, peut être utilisé pour séparer deux commandes écrites sur une seule ligne.


::::::::::::::::::::::::::::::::::::::::::::::::::

Lorsque le shell voit le mot-clé `for`, il sait qu'il doit répéter une commande (ou un groupe de commandes) une fois pour chaque élément d'une liste. Chaque fois que la boucle s'exécute (appelée itération), un élément de la liste est attribué en séquence à la **variable** et les commandes à l'intérieur de la boucle sont exécutées, avant de passer à l'élément suivant de la liste. À l'intérieur de la boucle, nous appelons la valeur de la variable en plaçant `$` devant. Le `$` indique à l'interpréteur de shell de traiter la variable comme un nom de variable et de substituer sa valeur à sa place, plutôt que de la traiter comme du texte ou une commande externe.

Dans cet exemple, la liste est constituée de trois noms de fichiers : `basilisk.dat`, `minotaur.dat` et `unicorn.dat`. À chaque itération de la boucle, nous imprimons d'abord la valeur actuelle que la variable `$fichier` détient. Cela n'est pas nécessaire pour le résultat, mais bénéfique ici pour nous faciliter le suivi. Ensuite, nous attribuons un nom de fichier à la variable `fichier` et exécutons la commande `head`. La première fois que la boucle s'exécute, `$fichier` est `basilisk.dat`. L'interpréteur exécute la commande `head` sur `basilisk.dat` et redirige les deux premières lignes vers la commande `tail`, qui affiche ensuite la deuxième ligne de `basilisk.dat`. Pour la deuxième itération, `$fichier` devient `minotaur.dat`. Cette fois, le shell exécute `head` sur `minotaur.dat` et redirige les deux premières lignes vers la commande `tail`, qui affiche ensuite la deuxième ligne de `minotaur.dat`. Pour la troisième itération, `$fichier` devient `unicorn.dat`, donc le shell exécute la commande `head` sur ce fichier et la commande `tail` sur la sortie de cette commande. Étant donné que la liste ne contenait que trois éléments, le shell sort de la boucle `for`.

:::::::::::::::::::::::::::::::::::::::::  callout

## Mêmes symboles, sens différents

Ici, nous voyons `>` utilisé comme invite de commandes shell, alors que `>` est également utilisé pour rediriger la sortie. De même, `$` est utilisé comme invite de commandes shell, mais, comme nous l'avons vu précédemment, il est également utilisé pour demander au shell de récupérer la valeur d'une variable.

Si le *shell* imprime `>` ou `$`, il attend de votre part que vous saisissiez quelque chose, et le symbole est une invite.

Si *vous* tapez `>` ou `$` vous-même, c'est un ordre de votre part au shell de rediriger la sortie ou de récupérer la valeur d'une variable.


::::::::::::::::::::::::::::::::::::::::::::::::::

Lors de l'utilisation de variables, il est également possible de placer les noms entre accolades pour délimiter clairement le nom de la variable : `$fichier` est équivalent à `${fichier}`, mais est différent de `${fichier}name`. Vous pouvez trouver cette notation dans les programmes d'autres personnes.

Nous avons appelé la variable dans cette boucle `fichier` afin de rendre son but plus clair pour les lecteurs humains. Le shell lui-même n'est pas concerné par le nom de la variable ; si nous avions écrit cette boucle comme suit:

```bash
$ for x in basilisk.dat minotaur.dat unicorn.dat
> faire
>     head -n 2 $x | tail -n 1
> terminé
```

ou :

```bash
$ for temperature in basilisk.dat minotaur.dat unicorn.dat
> faire
>     head -n 2 $temperature | tail -n 1
> terminé
```

cela fonctionnerait exactement de la même manière. *Ne faites pas cela*. Les programmes ne sont utiles que si les gens peuvent les comprendre, donc les noms sans signification (comme `x`) ou les noms trompeurs (comme `temperature`) augmentent les chances que le programme ne fasse pas ce que ses lecteurs pensent qu'il fait.

Dans les exemples ci-dessus, les variables (`chose`, `fichier`, `x` et `temperature`) auraient pu être appelées autrement, du moment que cela avait un sens à la fois pour la personne qui écrivait le code et pour celle qui le lisait.

Notez également que les boucles peuvent être utilisées pour autre chose que les noms de fichiers, comme une liste de nombres ou un sous-ensemble de données.

:::::::::::::::::::::::::::::::::::::::  challenge

## Créez votre propre boucle

Comment écririez-vous une boucle qui affiche les 10 chiffres de 0 à 9 ?

:::::::::::::::::::::::::::::::::::::::::  solution

## Solution

```bash
$ for loop_variable in 0 1 2 3 4 5 6 7 8 9
> faire
>     echo $loop_variable
> terminé
```

```output
0
1
2
3
4
5
6
7
8
9
```

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## Variables dans les boucles

Cet exercice fait référence au répertoire `shell-lesson-data/exercise-data/alkanes`.
`ls *.pdb` renvoie la sortie suivante :

```output
cubane.pdb  ethane.pdb  methane.pdb  octane.pdb  pentane.pdb  propane.pdb
```

Quel est le résultat du code suivant ?

```bash
$ for fichier in *.pdb
> faire
>     ls *.pdb
> terminé
```

Maintenant, quel est le résultat du code suivant ?

```bash
$ for fichier in *.pdb
> faire
>     ls $fichier
> terminé
```

Pourquoi ces deux boucles donnent-elles des résultats différents ?

:::::::::::::::::::::::::::::::::::::::::  solution

## Solution

Le premier bloc de code donne la même sortie à chaque itération de la boucle. Bash développe le joker `*.pdb` à l'intérieur du corps de la boucle (ainsi qu'avant le début de la boucle) pour correspondre à tous les fichiers se terminant par `.pdb` puis les répertorie à l'aide de `ls`. La boucle développée ressemblerait à ceci :

```bash
$ for fichier in cubane.pdb  ethane.pdb  methane.pdb  octane.pdb  pentane.pdb  propane.pdb
> faire
>     ls cubane.pdb  ethane.pdb  methane.pdb  octane.pdb  pentane.pdb  propane.pdb
> terminé
```

```output
cubane.pdb  ethane.pdb  methane.pdb  octane.pdb  pentane.pdb  propane.pdb
cubane.pdb  ethane.pdb  methane.pdb  octane.pdb  pentane.pdb  propane.pdb
cubane.pdb  ethane.pdb  methane.pdb  octane.pdb  pentane.pdb  propane.pdb
cubane.pdb  ethane.pdb  methane.pdb  octane.pdb  pentane.pdb  propane.pdb
cubane.pdb  ethane.pdb  methane.pdb  octane.pdb  pentane.pdb  propane.pdb
cubane.pdb  ethane.pdb  methane.pdb  octane.pdb  pentane.pdb  propane.pdb
```

Le deuxième bloc de code liste un fichier différent à chaque itération de la boucle. La valeur de la variable `fichier` est évaluée à l'aide de `$fichier`, puis listée à l'aide de `ls`.

```output
cubane.pdb
ethane.pdb
methane.pdb
octane.pdb
pentane.pdb
propane.pdb
```

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## Limitation des ensembles de fichiers

Quel serait le résultat de l'exécution de la boucle suivante dans le répertoire `shell-lesson-data/exercise-data/alkanes` ?

```bash
$ for nom_fichier in c*
> faire
>     ls $nom_fichier
> terminé
```

1. Aucun fichier n'est répertorié.
2. Tous les fichiers sont répertoriés.
3. Seuls `cubane.pdb`, `octane.pdb` et `pentane.pdb` sont répertoriés.
4. Seul `cubane.pdb` est répertorié.

:::::::::::::::  solution

## Solution

4 est la réponse correcte. `*` correspond à zéro ou plusieurs caractères, donc tout nom de fichier commençant par la lettre 'c', suivi de zéro ou plusieurs autres caractères, sera apparié.


::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## Limitation des ensembles de fichiers

En modifiant la commande ci-dessus pour utiliser cette commande à la place :

```bash
$ for fichier in *c*
> faire
>     ls $fichier
> terminé
```

1. Les mêmes fichiers sont répertoriés.
2. Tous les fichiers sont répertoriés cette fois.
3. Aucun fichier n'est répertorié cette fois.
4. Les fichiers `cubane.pdb` et `octane.pdb` seront répertoriés.
5. Seul le fichier `octane.pdb` sera répertorié.

:::::::::::::::  solution

## Solution

4 est la réponse correcte. `*` correspond à zéro ou plusieurs caractères, donc un nom de fichier avec zéro ou plusieurs caractères avant une lettre 'c' et zéro ou plusieurs caractères après la lettre 'c' sera apparié.

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## Enregistrement dans un fichier dans une boucle - Partie un

Dans le répertoire `shell-lesson-data/exercise-data/alkanes`, quel est l'effet de cette boucle?

```bash
for alcane in *.pdb
do
    echo $alcane
    cat $alcane > alkanes.pdb
done
```

1. Affiche `cubane.pdb`, `ethane.pdb`, `methane.pdb`, `octane.pdb`, `pentane.pdb` et le texte de `propane.pdb` sera enregistré dans un fichier appelé `alkanes.pdb`.
2. Affiche `cubane.pdb`, `ethane.pdb` et `methane.pdb`, et le texte des trois fichiers serait concaténé et enregistré dans un fichier appelé `alkanes.pdb`.
3. Affiche `cubane.pdb`, `ethane.pdb`, `methane.pdb`, `octane.pdb` et `pentane.pdb`, et le texte de `propane.pdb` sera enregistré dans un fichier appelé `alkanes.pdb`.
4. Aucun des choix précédents.

:::::::::::::::  solution

## Solution

1. Le texte de chaque fichier est écrit dans le fichier `alkanes.pdb` en fonction de l'itération de la boucle. Cependant, le fichier est écrasé à chaque itération de la boucle, donc le contenu final de `alkanes.pdb` est le texte du fichier `propane.pdb`.
  
  
  
::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## Enregistrement dans un fichier dans une boucle - Partie deux

Aussi dans le répertoire `shell-lesson-data/exercise-data/alkanes`, quel serait le résultat de la boucle suivante?

```bash
for fichier_donnees in *.pdb
do
    cat $datafile >> all.pdb
done
```

1. Tout le texte de `cubane.pdb`, `ethane.pdb`, `methane.pdb`, `octane.pdb` et `pentane.pdb` serait concaténé et enregistré dans un fichier appelé `all.pdb`.
2. Le texte de `ethane.pdb` sera enregistré dans un fichier appelé `all.pdb`.
3. Tout le texte de `cubane.pdb`, `ethane.pdb`, `methane.pdb`, `octane.pdb`, `pentane.pdb` et `propane.pdb` serait concaténé et enregistré dans un fichier appelé `all.pdb`.
4. Tout le texte de `cubane.pdb`, `ethane.pdb`, `methane.pdb`, `octane.pdb`, `pentane.pdb` et `propane.pdb` serait affiché à l'écran et enregistré dans un fichier appelé `all.pdb`.

:::::::::::::::  solution

## Solution

3 est la réponse correcte. `>>` ajoute à un fichier, au lieu de l'écraser avec la sortie redirigée d'une commande. Étant donné que la sortie de la commande `cat` a été redirigée, rien n'est affiché à l'écran.

