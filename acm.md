Anamyse ne composantes multiples
================

# Introduction à l’ACM

## Sur quelles données porte une ACM ?

L’ACM consiste à synthétiser de manière géométrique à l’aide de nuages
de points des tableaux présentant, en ligne, des individus décrits en
colonne par des variables qualitatives, c’est-à-dire des informations
sur ces individus qui ne prennent qu’un nombre limité de valeurs
appelées des modalités.

Il faut bien distinguer : \* Une variable qualitative : exemple : statut
matrimominal. \* Une modalité (ou valeur): ex : célibataire.

## Transformées en TDC…

L’ACM va reposer sur une déclinaison du tableau brut que l’on appelle
tableau dijonctif complet. Dans le tableau disjonctif complet les lignes
sont les individus et les colonnes les modalités des variables. Lorsque
qu’un individu possède la modalité de la variable on va trouver la
valeur 1 et s’il ne la possède pas, 0. L’utilisateur ne construit et ne
visualise jamais le tableau disjonctif complet. C’est une opération
transparente.

## Objectifs

L’ACM débute par l’étude des individus puis l’études des variables.

### Etude des individus

L’ACM vise à étudier la variablilité des individus. Est ce que des
indivus se ressemblent (indivdus possédant de nombreux modalités en
commun) ? Est ce que des indivus sont différents (indivus avec peu de
modalités en commun) ? L’ensemble des ressemblances / différences entre
les individus est appelée la variabilité des individus.Si tous les
individus sont similaires : l’analyse est inutile.

### Etudes des variables

#### Liaison des variables

La liaison entre deux variables qualitatives s’étudie au travers des
associations entre leurs modalités. Par exemple, les variables statut
matrimonial et enfants sont liées : les personnes qui sont mariées ont
plutôt des enfants.

``` r
- L'ACM va extraire les principales dimensions de variabilité. Dimension: jeunes / vieux. 1ErGM / av GM...
- On s'interresse à la liaison des modalités  qu'on appele également asosciations ou correlation. (en relation avec les modalités).
Modalités fréquentes, quelles sont les modalités rares ?

### 

- l'ACM va construire des variables synthétiques qui résument au mieux les variables.
```

## Préparations des données

La préparation des données vise à construire un tableau dans lequel : -
variables qualitatives : le nombre des modalités est réduit à quelques
modalités. - variables quantitatives discrétisées (ex :âge en classes
d’âges). - individus : les lignes comportant des valeurs absentes ou
abérrantes filtrées.

#### Variables calculées

age =1911-R2 pb des années vide et ? -\> valeur.

\=SI(ESTNUM(R2);1911-R2;"")

ATtention aux valuers abbérantes

#### Regrouper les modalités d’une variable

nationalité =\> limitrophe -non limitrophe (ninarisation)

1 Tableau dynmaique pour lister les modalités 2. Copie =\> nelle feuille
de calcul =\> nationalités. 3. Création colonne proxi et on renseigne à
la main en minsucule par copié collé. 4 Création colonne :
nationalite\_localisation 5. Recherche verticale :
=RECHERCHEV(B2;$Feuille4.A\(2:\)Feuille4.B$39;2;0)

### Convertir un type de données en un autre

### Periode de déclaration

c4\_annee\_declaration periode =SI(N2\<=1915;“av1gm”;“1gm”)

#### Nombre d’enfants

nombre d’enfants en enfants vrai / faux =\> binarisation.
=SI(NON(ESTVIDE(AL2)) ;“avec\_enfants”;“sans\_enfants”) Mieux :
=SI(OU(ESTVIDE(AL2); AL2=0) ;“sans\_enfants”;“avec\_enfants”)

\=SI(N2\<=1915;“av1gm”;“1gm”)

### Combiner plusieurs variables

nom et nationalité

#### renommage variables

situation\_matrimoniale

``` r
library("FactoMineR")
library("factoextra")
library(explor)

library(dplyr)

library(readr)
dataset_acm0 <- read_csv("etr_acm.csv")

dataset_acm1 <- dataset_acm0 %>%
  distinct(nom_nat, .keep_all = TRUE) %>%
  mutate(age=cut(age, 2)) %>% 
  select(-prof_manoeuvre) 

View(dataset_acm1)

acm1 <- MCA(dataset_acm1[-c(1:4)])


explor(acm1)
```
