---
title: "usefull codes"
author: "Elise Maigné"
date: "21/07/2020"
output: jekyllthat::jekylldown
---

## Benchmarking

Pour comparer la vitesse de 2 bouts de codes (ou plus que 2), on utilise
le package rbenchmark.

La structure générale est la suivante :

``` r
library(rbenchmark)

benchmark(replications=100,
          code1,
          code2,
          columns=c('test', 'elapsed', 'replications'))
```

Et avec un exemple :

``` r
library(rbenchmark)
library(data.table)

# Ecriture d'un fichier de 15000 lignes 
data(iris)
iris <- iris[rep(rownames(iris), each=100),]
fwrite(iris, file="irisbig.csv", sep=",")

# Lecture du fichier, répétée 1000 fois. 
benchmark(replications=rep(1000),
          test1 <- read.csv("irisbig.csv", sep=","),
          test2 <- fread("irisbig.csv"),
          columns=c('test', 'elapsed', 'replications'))
```

    ##                                          test elapsed replications
    ## 1 test1 <- read.csv("irisbig.csv", sep = ",")  13.613         1000
    ## 2               test2 <- fread("irisbig.csv")   1.840         1000

Le test2, utilisant la fonction `fread` du package `data.table` est bien
plus rapide.
