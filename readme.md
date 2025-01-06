# 1. Énoncé

Cet exercice vise à appliquer différents traitements sur un fichier contenant un grand nombre de caractères. Nous allons ici travailler différentes procédures "classique" appliqués sur les séquence d’ADN.

Au coeur des cellules vivantes se trouve l’ADN, cette structure de données compose les caractères génétiques de son porteur. Ces données sont “codés” à l’aide de 4 molécules que l’on appelle des nucléotides et que l’on représente avec les lettres A, T, C et G, correspondants au molécules adénine, thymine, cytosine et guanine.

Une séquence d’ADN complète pour un être humain comporte environ 6.6 milliards de nucléotides. Le séquençage de l’ADN est une tache complexe mais très utile, notamment dans le cas de l’étude des virus, par exemple [pour le SARS-CoV-2](https://www.pasteur.fr/fr/espace-presse/documents-presse/institut-pasteur-sequence-genome-complet-du-coronavirus-sars-cov-2) (COVID-19).

Pour cet exercice vous travaillerez avec un fichier contenant 27500 nucléotides.
Dans un premier temps, il sera question de traduire l'ADN en protéines puis de se pencher sur les séquences consensus.

Aucun langage n'est imposé. Dans votre choix de langage, prenez bien en compte le fait qu'il s'agira là de manipulation de fichier et de traitement de donnée.

Pour réaliser cet exercice, vous aurez donc besoin du fichier ADN [que vous trouverez ici](https://gist.githubusercontent.com/Que20/7f727f9ad9fa7e2761771fcffdcb5914/raw/6d2759d44eba3bc6a8e2184571da8fda9aa22aea/adn) (un petit `ctrl+s` pour le sauvegarder sur votre disque une fois dessus) - aucune extension n’est nécessaire mais vous pouvez l’enregistrer en `txt` si vous voulez l’ouvrir avec votre éditeur de texte.

# Partie A : Protéines

Les protéines sont des macromolécules aux formes spécifiques qui forment le vivant en fabriquant les composants structurants de nos cellules. Le rôle principal de l’ADN est de fabriquer ces protéines.
Nous allons donc voir comment chaque codon (composés de 3 nucléotides) arrivent à former une protéines.
Les protéines sont, comme les nucléotides, codés et représentés avec des lettres de l’alphabet.

## Étape 1: On découpe

Le fichier contient donc 27500 éléments. La première étape va être de découper et regrouper ces éléments en petits groupes de 3 appelés codons, ce sont donc des séquences de 3 nucléotides, pour un rendu qui suivrait cet exemple:

```
GTT
CGT
TGA
CGG
...

```

Une approche serait de découper ce gros fichier en un tableau, contenant, pour chaque entrée, 3 caractères.

## Étape 2: On convertit

Utiliser la table de conversion suivante pour traduire les codons en protéines:
L’idée est de remplacer les codons (à gauche) par la protéine (à droite).

```jsx
'ATA':'I',
'ATC':'I',
'ATT':'I',
'ATG':'M',
'ACA':'T',
'ACC':'T',
'ACG':'T',
'ACT':'T',
'AAC':'N',
'AAT':'N',
'AAA':'K',
'AAG':'K',
'AGC':'S',
'AGT':'S',
'AGA':'R',
'AGG':'R',
'CTA':'L',
'CTC':'L',
'CTG':'L',
'CTT':'L',
'CCA':'P',
'CCC':'P',
'CCG':'P',
'CCT':'P',
'CAC':'H',
'CAT':'H',
'CAA':'Q',
'CAG':'Q',
'CGA':'R',
'CGC':'R',
'CGG':'R',
'CGT':'R',
'GTA':'V',
'GTC':'V',
'GTG':'V',
'GTT':'V',
'GCA':'A',
'GCC':'A',
'GCG':'A',
'GCT':'A',
'GAC':'D',
'GAT':'D',
'GAA':'E',
'GAG':'E',
'GGA':'G',
'GGC':'G',
'GGG':'G',
'GGT':'G',
'TCA':'S',
'TCC':'S',
'TCG':'S',
'TCT':'S',
'TTC':'F',
'TTT':'F',
'TTA':'L',
'TTG':'L',
'TAC':'Y',
'TAT':'Y',
'TAA':'_',
'TAG':'_',
'TGC':'C',
'TGT':'C',
'TGA':'_',
'TGG':'W',
```

Exemple de résultat :

```jsx
> "EHI_R_VLRCGGLSGSLLTQFIGSAHIEPSPW"
```

# Partie B: Consensus

Une autre opération fréquente sur l’ADN est de calculer des séquences consensus. Cette séquence correspond à la suites des nucléotides les plus fréquents à chaque positions sur une liste de séquence donnée.

## Étape 1: On découpe

Revenons à notre fichier initial. Comme vu précédement, le fichier contient donc 27500 éléments. La première étape va être de re-découper et regrouper ces éléments en petits groupes, puis en séquences comme on l’a fait sur la partie A. Notre premier objectif est d'avoir 1100 groupes de 25 éléments/caractères/nucléotides. Bien évidement, nous gardons l’ordre des éléments dans le fichiers.

Exemple :

```
GTTCGTTGACGGTCCGGACCAATGA
AACAGACCCAACCAAGCTTTCGCTC
AACGACCATTCCCCACCTTCCGTCT
...

```

## Étape 2: On affine

Ces groupes de 25 nucléotides sont déjà plus facile à manipuler. Mais il va falloir encore affiner tout ça. Sur la même base, nous allons redécouper ces "groupes" en 5 séquences de 5 nucléotides pour obtenir le rendu suivant :

```
G T T C G
T T G A C
G G T C C
G G A C C
A A T G A

A A C A G
A C C C A
A C C A A
G C T T T
C G C T C
...

```

## Étape 3: On trouver les récurrences

Maintenant que nous avons une donnée plus facile à lire et à parcourir, calculer, pour chaque groupe de 5 séquences, la récurrence de chaque élément, pour générer un rendu comme suit:

```
Séquence 1:
A: 1 1 1 1 1
C: 0 0 0 3 3
G: 3 2 1 1 1
T: 1 2 3 0 0
```

Cet exemple correspond au premier bloc illustré au dessus. À lire de la manière suivante :

> Sur ces 5 séquences :
L'élément 'A' est présent 1 fois en première position, 1 fois en seconde, 1 fois en troisième, 1 fois en quatrième et 1 fois en cinquième.
L'élément 'B' n'apparait pas dans les 3 premières positions, 3 fois en quatrième et en cinquième position.
...
> 

## Étape 4: Enfin, la séquence consensus

Grace à la matrice de récurrences obtenue, il nous suffit de prendre, pour chaque position l'élément qui revient le plus pour en dégager la séquence consensus. Pour l'exemple ci-dessus, la séquence serait donc:

```
G [GT] T C C

```

Car :

- en première position, le G est l'élément le plus présent (3 occurrences)
- le G et le T sont présent tous les deux à 2 occurrences en seconde position, on les représente donc dans des crochets
- en troisième, c'est T qui est dominant (3 occurrences)
- le C est dominant en quatrième position (3 occurrences)
- le C est aussi dominant en cinquième position (3 occurrences)


