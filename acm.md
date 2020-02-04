Analyse en composantes multiples
================

# Introduction à l’ACM

## Sur quelles données porte une ACM ?

L’ACM consiste à synthétiser de manière géométrique à l’aide de nuages
de points des tableaux présentant, en ligne, des individus décrits en
colonne par des variables qualitatives, c’est-à-dire des informations
sur ces individus qui ne prennent qu’un nombre limité de valeurs
appelées des modalités.

:bulb: Il faut bien distinguer :

  - Une variable qualitative : exemple : periode.  
  - Une modalité (ou valeur): ex : 1gm.

<!-- end list -->

    ## # A tibble: 10 x 6
    ##    id_individu nationalite_localisation periode sexe  age       dernier_domicile
    ##          <dbl> <fct>                    <fct>   <fct> <fct>     <fct>           
    ##  1           1 nat: nonlimit            1gm     h     (44,70.1] fr              
    ##  2           2 nat: nonlimit            1gm     h     (44,70.1] fr              
    ##  3           3 nat: nonlimit            av1gm   h     (44,70.1] fr              
    ##  4           4 nat: nonlimit            1gm     f     (44,70.1] fr              
    ##  5           5 nat: nonlimit            1gm     h     (44,70.1] etr             
    ##  6           6 nat: nonlimit            av1gm   h     (44,70.1] etr             
    ##  7           7 nat: nonlimit            av1gm   h     (44,70.1] etr             
    ##  8           8 nat: nonlimit            1gm     h     (17.9,44] etr             
    ##  9           9 nat: limitrophe          av1gm   f     (17.9,44] alslor          
    ## 10          10 nat: limitrophe          1gm     h     (44,70.1] alslor

## Interpétation

L’ACM débute par l’étude des individus puis l’études des variables.

### Etude des individus

L’ACM vise à étudier la variablilité des individus. Est ce que des
indivus se ressemblent (individus possédant de nombreux modalités en
commun) ? Est ce que des individus sont différents (individus avec peu
de modalités en commun) ? L’ensemble des ressemblances / différences
entre les individus est appelée la variabilité des individus. Si tous
les individus sont similaires l’analyse est inutile.

  - Si deux individus prennent les mêmes modalités : distance = 0.
  - Si deux individus prennent une majorité de modalités en commun :
    distance = petite.
  - Si deux individus prennent les même modalités sauf un qui possède
    une rare : distance grande.
  - Si deux individus ont en commun une modalité rare : distante petite.
  - Un individu est d’autant plus loin de l’origine qu’il possède des
    modalités rares.
  - Un individu proche de l’origine possède des modalités fréquentes.

<!-- end list -->

``` r
res <- MCA(etr_acm_demo[-c(1)], graph = FALSE)  
```

``` r
plot(res, invisible=c("var","quali.sup"), cex=0.7)
```

![](acm_files/figure-gfm/unnamed-chunk-5-1.png)<!-- -->

### Etudes des variables

Les modalités proches du centre sont les plus fréquentes, les modalités
les plus éloignées sont les plus rares.

``` r
plot(res, invisible=c("ind","quali.sup"), autoLab="y", cex=0.7,title="Modalités actives")
```

![](acm_files/figure-gfm/unnamed-chunk-6-1.png)<!-- -->

## Préparations des données

La préparation des données vise à construire un tableau dans lequel : -
variables qualitatives : le nombre des modalités est réduit à quelques
modalités. - variables quantitatives discrétisées (ex :âge en classes
d’âges). - individus : les lignes comportant des valeurs absentes ou
aberrantes sont filtrées.

## L’ACM dans R

``` r
# Packages
library(FactoMineR)                        # analyse exploratoire des donnees multivariées
library(explor)                            # visualisation interactive de l'ACM
library(dplyr)                             # manipulation des  données
library(readr)                             # lecture des fichiers csv

dataset_acm0 <- read_csv("etr_acm.csv")    # lecture du fichier etr_acm.csv

# Préparation des données 
dataset_acm1 <- dataset_acm0 %>%
  distinct(nom_nat, .keep_all = TRUE) %>%  # supression des doublons
  mutate(age=cut(age, 2)) %>%              # la variable age est discrétisée en deux intervalles égaux
  select(-prof_manoeuvre)                  # la variable manoeuvre est exclue de l'ACM. 

# visualisation du tableau brut 
View(dataset_acm1)

# ACM
acm1 <- MCA(dataset_acm1[-c(1:4)])         # acm (avec exclusion des variables 1 à 4).

# Visualisation interactive
explor(acm1)                               # visualisation interactive de l'ACM
```

## Interprétation

### Individus

![GitHub Logo](images/explor_ind.svg)

## Modalités

### Modalité et origine des axes

![GitHub Logo](images/explor_var.svg)

les modalités proches du centre sont les plus fréquentes, les modalités
les plus éloignées sont les plus rares. Les modalités proches traduisent
des associations.

### Modalité et construction des axes

Le repère, appelé plan factoriel est construit par un axe 1 (horizontal)
et un axe 2 (vertical). Les axes traduisent des oppositions de
modalités.

Exemple :

La modalité « mar » de la variable “situation\_matrimoniale” se situe du
côté positif de l’axe 1 (coordonnée positive), les individus mariés sont
donc biens représentés du côté positif de l’axe 1. Ils ont par ailleurs
plus de chance d’avoir comme modalité « avec enfants » de la variable
“enfants”. Ainsi on peut déduire que le le côté positif de l’axe 1
représente bien les individus mariés avec efants.

:bulb: On relève les deux premières variables de chaque axe pour
traduire les principales oppositions.

![GitHub Logo](images/var-axe1.PNG) ![GitHub Logo](images/var-axe2.PNG)
