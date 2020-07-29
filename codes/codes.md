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

benchmark(replications=rep(100),
          code1,
          code2,
          columns=c('test', 'elapsed', 'replications'))
```

``` r
library(rbenchmark)
library(data.table)
data(iris)
iris <- iris[rep(rownames(iris), each=100),]
fwrite(iris, file="irisbig.csv", sep=",")
benchmark(replications=rep(1000),
          test1 <- read.csv("irisbig.csv", sep=","),
          test2 <- fread("irisbig.csv", sep=",") ,
          test3 <- fread("irisbig.csv") ,
          columns=c('test', 'elapsed', 'replications'))
```

    ##                                          test elapsed replications
    ## 1 test1 <- read.csv("irisbig.csv", sep = ",")  16.527         1000
    ## 2    test2 <- fread("irisbig.csv", sep = ",")   2.951         1000
    ## 3               test3 <- fread("irisbig.csv")   3.024         1000
