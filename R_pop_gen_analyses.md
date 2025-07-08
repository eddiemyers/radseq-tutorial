# R-Based Population Genetic Analyses from VCF

This page describes downstream population genetic analyses performed using the VCF file produced by ipyrad. All analyses were done in R using packages like `adegenet`, `LEA`, and `ggplot2`.

---

##  1. Load VCF and Prepare Data

```r
# Install all libraries
install.packages(c("adegenet", "vcfR", "tidyverse", "vegan", "ggplot2", "geosphere"))
# The package LEA needs to be installed using Bioconductor (it's not on CRAN)
if (!requireNamespace("BiocManager", quietly = TRUE))
    install.packages("BiocManager")

BiocManager::install("LEA")

# Load required libraries
library(adegenet)
library(vcfR)
library(LEA)
library(tidyverse)
library(vegan)
library(ggplot2)
library(geosphere)

# Load VCF
# SNP thinning may be necessary for SNMF and IBD if you have high linkage.
vcf <- read.vcfR("your_data.vcf")

# Convert to genlight
# Ensure your metadata.csv contains sample IDs in the same order as the VCF file.
gl <- vcfR2genlight(vcf)
pop(gl) <- read.csv("metadata.csv")$population  # Optional: assign population labels
```

## 2. Discriminant Analysis of Principal Components (DAPC)

```r
# Identify clusters using find.clusters
grp <- find.clusters(gl, max.n.clust = 10)  # Choose based on BIC plot

# Run DAPC
dapc_result <- dapc(gl, pop = grp$grp)
scatter(dapc_result)

```

## 3. SNMF and Ancestry Proportions on a Map

```r
# Convert to LEA format
write.geno(gl, "gl_data.geno")

# Run SNMF
project <- snmf("gl_data.geno", K = 1:6, repetitions = 5, entropy = TRUE)
plot(project, col = "blue", pch = 19)

# Choose best K (lowest cross-entropy)
best_run <- which.min(cross.entropy(project, K = 3))  # example with K=3
qmatrix <- Q(project, K = 3, run = best_run)

# Merge qmatrix with coordinates
coords <- read.csv("metadata.csv")  # must include lat/lon
q_df <- cbind(coords, qmatrix)

# Plot pie charts on a map
install.packages("ggforce")
library(ggforce)

ggplot() +
  borders("world", colour = "gray80") +
  geom_scatterpie(data = q_df, aes(x = longitude, y = latitude), 
                  cols = paste0("V", 1:3), pie_scale = 0.5) +
  coord_fixed() +
  theme_minimal()

# ahh... maybe we can do better
ggplot() +
  borders("world", colour = "gray80", fill = "gray95") +
  geom_scatterpie(data = q_df, aes(x = Longitude, y = Latitude), 
                  cols = paste0("V", 1:2), pie_scale = 1.0) +
  coord_sf(xlim = c(-78.5, -76), ylim = c(17.5, 18.6), expand = FALSE) +
  theme_minimal()

```

## 4. Isolation by Distance (IBD) – Mantel Test

```r
# Create distance matrices
gen_dist <- dist(tab(gl, NA.method = "mean"))
geo_dist <- distm(coords[, c("longitude", "latitude")]) / 1000  # in km

# Mantel test
mantel_result <- mantel(gen_dist, as.dist(geo_dist), method = "pearson")
print(mantel_result)

# Convert geographic distance matrix to half matrix
geo_dist_dist <- as.dist(geo_dist)

# Plot IBD. 
plot(as.vector(geo_dist_dist), as.vector(gen_dist),
     xlab = "Geographic Distance (km)",
     ylab = "Genetic Distance",
     main = "Isolation by Distance")
abline(lm(as.vector(gen_dist) ~ as.vector(geo_dist_dist)), col = "red")
```

## 5. Hierarchical Clustering of Samples

```r
# Perform hierarchical clustering
hc <- hclust(gen_dist, method = "ward.D2")

# Extract sample names (these are usually the row names of the input to dist())
labels <- attr(gen_dist, "Labels")

# Plot dendrogram with labels
plot(hc, labels = labels, hang = -1, main = "Hierarchical Clustering of Samples")
```
