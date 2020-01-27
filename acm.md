Untitled
================

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
