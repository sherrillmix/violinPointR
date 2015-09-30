# Plot one-dimensional data using quasirandom noise and density estimates

## Introduction

`violinPointR` provides a way to plot one-dimensional data (perhaps divided into several categories) by spreading the data points to fill the kernel density. It uses a [van der Corput sequence](http://en.wikipedia.org/wiki/Van_der_Corput_sequence) to space the dots and avoid generating distracting patterns in the data. See the examples below.

Beeswarm plots (aka column scatter plots or violin scatter plots) are a way of plotting points that would ordinarily overlap so that they fall next to each other instead. In addition to reducing overplotting, it helps visualize the density of the data at each point (similar to a violin plot), while still showing each data point individually.

## Installation


```r
devtools::install_github("sherrillmix/violinPointR")
```

## Examples

### Using base graphics

We use the provided function `offsetX` to generate the x-offsets for plotting.

```r
library(violinPointR)
# Generate data
set.seed(12345)
dat <- list(rnorm(50), rnorm(500), c(rnorm(100), rnorm(100,5)), rcauchy(100))
names(dat) <- c("Normal", "Dense Normal", "Bimodal", "Extremes")

# Plot distributions
par(mfrow=c(4,1), mar=c(2.5,3.1, 1.2, 0.5),mgp=c(2.1,.75,0),cex.axis=1.2,cex.lab=1.2,cex.main=1.2)
sapply(names(dat),function(label) {
  y<-dat[[label]]
  ids <- rep(1:4, each=length(y))
  offsets <- c(
    offsetX(y),  # Default
    offsetX(y, adjust=2),    # More smoothing
    offsetX(y, adjust=0.1),  # Tighter fit
    offsetX(y, width=0.1))   # Less wide

  plot(offsets + ids, rep(y, 4), ylab='y value', xlab='', xaxt='n', pch=21,col='#00000099',bg='#00000033',las=1,main=label)
  axis(1, 1:4, c("Default", "Adjust=2", "Adjust=0.1", "Width=10%"))
})
```

![plot of chunk base-examples](README_files/base-examples-1.png) 

------
Authors: Scott Sherrill-Mix and Erik Clarke

