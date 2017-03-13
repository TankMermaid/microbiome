---
title: "Probe"
author: "Leo Lahti"
date: "2017-03-05"
bibliography: 
- bibliography.bib
- references.bib
output: 
  rmarkdown::html_vignette
---
<!--
  %\VignetteEngine{knitr::rmarkdown}
  %\VignetteIndexEntry{microbiome tutorial - probe}
  %\usepackage[utf8]{inputenc}
  %\VignetteEncoding{UTF-8}  
-->


### Probe summarization

Summarize (preprocessed) oligo-level data into phylotype level; examples with simulated data; see [read_hitchip](reading) to use your own data. We use and recommend the [Robust Probabilistic Averaging (RPA)](https://github.com/antagomir/RPA/wiki) for probe summarization.



```r
library(microbiome)
library(HITChipDB)
data.directory <- system.file("extdata", package = "microbiome")

# Read oligo-level data (here: simulated example data)
probedata <- HITChipDB::read_hitchip(data.directory, method = "frpa")$probedata

# Read phylogeny map
# NOTE: use phylogeny.filtered for species/L1/L2 summarization
# Load taxonomy from output directory
f <- system.file("inst/extdata/get_hitchip_taxonomy.R", package = "microbiome")
source(f)
taxonomy <- get_hitchip_taxonomy("HITChip", "filtered")

# Summarize oligos into higher level phylotypes
dat <- RPA::summarize_probedata(
                 probedata = probedata,
		 taxonomy = taxonomy, 
                 method = "rpa",
		 level = "species")
```


### Retrieve probe-level data

Get probes for each probeset:


```r
sets <- RPA::retrieve.probesets(taxonomy, level = "species", name = NULL)
```


Get probeset data matrix/matrices:


```r
set <- RPA::get.probeset("Actinomyces naeslundii", "species",
       		     taxonomy, probedata, log10 = TRUE)
```




