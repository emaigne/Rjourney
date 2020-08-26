---
title: "benchmarking"
author: "Elise Maigné"
date: "21/07/2020"
output: jekyllthat::jekylldown
---

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
benchmark(replications=1000,
          test1 <- read.csv("irisbig.csv", sep=","),
          test2 <- fread("irisbig.csv"),
          columns=c('test', 'elapsed', 'replications'))
```

    ##                                          test elapsed replications
    ## 1 test1 <- read.csv("irisbig.csv", sep = ",")  13.924         1000
    ## 2               test2 <- fread("irisbig.csv")   1.949         1000

Le test2, utilisant la fonction `fread` du package `data.table` est bien
plus rapide.

De la même manière on peut utiliser le package microbenchmark qui a
l’avantage d’avoir une belle visualisation avec ggplot2 :

``` r
library(microbenchmark)
library(ggplot2)

# Lecture du fichier, répétée 1000 fois. 
mb <- microbenchmark(
          test1 <- read.csv("irisbig.csv", sep=","),
          test2 <- fread("irisbig.csv"),
          times=1000)

mb
```

    ## Unit: milliseconds
    ##                                         expr       min        lq      mean
    ##  test1 <- read.csv("irisbig.csv", sep = ",") 13.189501 13.399656 14.006566
    ##                test2 <- fread("irisbig.csv")  1.596916  1.635067  1.811893
    ##     median        uq      max neval cld
    ##  13.536801 13.998012 53.24624  1000   b
    ##   1.689401  1.777894 43.81017  1000  a

``` r
autoplot(mb)
```

    ## Coordinate system already present. Adding new coordinate system, which will replace the existing one.

![](benchmarking_files/figure-gfm/unnamed-chunk-3-1.png)<!-- -->
