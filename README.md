[![Build Status](https://travis-ci.org/MahShaaban/pcr.svg?branch=master)](https://travis-ci.org/MahShaaban/pcr)
[![Build status](https://ci.appveyor.com/api/projects/status/y9hfiwwc390cce28?svg=true)](https://ci.appveyor.com/project/MahShaaban/pcr)
[![Build status](https://ci.appveyor.com/api/projects/status/y9hfiwwc390cce28/branch/master?svg=true)](https://ci.appveyor.com/project/MahShaaban/pcr/branch/master)
[![Coverage Status](https://img.shields.io/codecov/c/github/MahShaaban/pcr/master.svg)](https://codecov.io/github/MahShaaban/pcr?branch=master)

# pcr  

# Overview  

Quantitative real-time PCR is an imprtant technique in medical and biomedical applicaitons. The `pcr` package provides a unified interface for quality assessing, analyzing and testing qPCR data for statistical significance. The aim of this document is to describe the different methods and modes used to relatively quantify gene expression of qPCR and their implemenation in the `pcr` package.  

# Getting started 

The `pcr` is available on github. To install it using `devtools`:  

```{r install_master, eval=FALSE}
# install package from github (under development)
devtools::install_github('MahShaaban/pcr')
```

The development version of the package can be similarly obtained through:  

```{r install_develop, eval=FALSE}
# install package from github (under development)
devtools::install_github('MahShaaban/pcr@develop')
```

```{r load_pcr}
# load required libraries
library(pcr)
```

The following chunck of code locates a dataset of $C_T$ values of two genes from 12 different samples and performs a quick analysis to obtain the expression of a target gene **c-myc** normalized by a control GAPDH in the **Kidney** samples relative to the brain samples. `pcr_analyze` provides differnt methods, the default one that is used here is 'delta_delta_ct' applies the populat ($\Delta\Delta C_T$) method.  

```{r analyze}
# default mode delta_delta_ct
## locate and read raw ct data
fl <- system.file('extdata', 'ct1.csv', package = 'pcr')
ct1 <- readr::read_csv(fl)

## add grouping variable
group_var <- rep(c('brain', 'kidney'), each = 6)

# calculate all values and errors in one step
## mode == 'separate_tube' default
res <- pcr_analyze(ct1,
                   group_var = group_var,
                   reference_gene = 'GAPDH',
                   reference_group = 'brain')
  
res
```

The output of `pcr_analyze` is explained in the documnetation of the function `?pcr_analyze` and the method it calls `?pcr_ddct`. Briefly, the input includes the $C_T$ value of c-myc `normalized` to the control GAPDH, The `calibrated` value of c-myc in the kidney relative to the brain samples and the final `relative_expression` of c-myc. In addition, an `error` term and a `lower` and `upper` intervals are provided.  

The previous analysis makes a few assumpitons. One of which is a perfect amplification efficiency of the PCR reation. To assess the validity of this assumption, `pcr_assess` provides a method called `efficiency`. The input `data.frame` is the $C_T$ values of c-myc and GAPDH at different input amounts/dilutions.  

```{r assess}
## locate and read data
fl <- system.file('extdata', 'ct3.csv', package = 'pcr')
ct3 <- readr::read_csv(fl)

## make a vector of RNA amounts
amount <- rep(c(1, .5, .2, .1, .05, .02, .01), each = 3)

## calculate amplification efficiency
res <- pcr_assess(ct3,
                  amount = amount,
                  reference_gene = 'GAPDH',
                  method = 'efficiency')
res
```

### Other analysis methods  

* Delta CT method  
* Relative standard curve method  

### Testing statistical significance  
* Two group testing *t*-test and *wilcoxon* test  
* Linear regression testing  

## Documnetation  

```r
browseVignettes("pcr")
```  

## Citation  

```r
citation("pcr")
```
