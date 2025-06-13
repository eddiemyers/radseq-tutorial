# 🧬 R-Based Population Genetic Analyses from VCF

This page describes downstream population genetic analyses performed using the VCF file produced by ipyrad. All analyses were done in R using packages like `adegenet`, `LEA`, and `ggplot2`.

---

## 📥 1. Load VCF and Prepare Data

```r
# Required libraries
library(adegenet)
library(vcfR)
library(LEA)
library(tidyverse)
library(vegan)
library(ggplot2)
library(geosphere)

# Load VCF
vcf <- read.vcfR("your_data.vcf")

# Convert to genlight
gl <- vcfR2genlight(vcf)
pop(gl) <- read.csv("metadata.csv")$population  # Optional: assign population labels
