---
title: "GraphDistances"
author: "Ichcha Manipur"
date: "2020-05-26"
output: rmarkdown::html_vignette
vignette: >
  %\VignetteIndexEntry{Vignette Title}
  %\VignetteEngine{knitr::knitr}
  %\VignetteEncoding{UTF-8}
---



## Introduction

Compute **distribution** based distance measures between weighted directed
and undirected graphs/networks.

### Install


```r
devtools::install_github("cds-group/GraphDistances")
```

### Usage

#### Load package

```r
library(GraphDistances)
```

#### Distance computation: Pairwise
Distance computation between the NDD (node distance distribution) and Transition Probability (TM-one walk) matrices of two example graphs.

```r
library(igraph)
num_nodes <- 60
g1 <- make_tree(num_nodes, children =2, mode = "out")
E(g1)$weight <- seq_len(ecount(g1))
g2 <- make_tree(num_nodes, children = 3, mode = "out")
E(g2)$weight <- seq_len(ecount(g2))

# Compute the distances between the node distance distribution of 2 graphs
binList <- getBins(list(g1, g2))
ndd1 <- getNodeDistanceDistr(g1, binList)
ndd2 <- getNodeDistanceDistr(g2, binList)
nddDistancepair <- getGraphpairdistance(ndd1, ndd2)

# Compute the distances between the transition probability matrix (one walk) of 2 graphs
trans1 <- getTransitionmatrix(g1, walk=1)
trans2 <- getTransitionmatrix(g2, walk=1)
transDistancepair <- getGraphpairdistance(trans1, trans2) 
```

#### Distance computation: Multiple graphs
nddDistanceMulti and transDistanceMulti are the Gram matrices produced after distance computation between the NDD and TM-one walk matrices of [40 graphs](data/KidneyGraphs.RData) provided with the package. The annotation of these graphs can be loaded by running data(annoKidney) in R for classification, clustering and visualization tasks.

```r
data("KidneyGraphs")

binsList <- getBins(KidneyGraphs)
nddList <- lapply(KidneyGraphs, function(x) getNodeDistanceDistr(x, binsList))
nddDistanceMulti <- getGraphlistdistance(nddList) #Takes sometime for computation

transList <- lapply(KidneyGraphs, function(x) getTransitionmatrix(x, walk=1))
transDistanceMulti <- getGraphlistdistance(transList) #Takes sometime for computation
```


### Cite
1. Granata, I., Guarracino, M.R., Kalyagin, V.A., Maddalena, L., Manipur, I. and Pardalos, P.M., 2018, December. Supervised classification of metabolic networks. In 2018 IEEE International Conference on Bioinformatics and Biomedicine (BIBM) (pp. 2688-2693). IEEE.
https://ieeexplore.ieee.org/abstract/document/8621500
2. Granata, I., Guarracino, M.R., Kalyagin, V.A., Maddalena, L., Manipur, I. and Pardalos, P.M., 2020. Model simplification for supervised classification of metabolic networks. Annals of Mathematics and Artificial Intelligence, 88(1), pp.91-104.
https://link.springer.com/article/10.1007/s10472-019-09640-y