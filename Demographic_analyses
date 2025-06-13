# 🧬 Demographic Inference with SFS and GADMA

This guide walks through performing demographic inference using a Site Frequency Spectrum (SFS) generated from a VCF file and running demographic models in **GADMA**. It uses the output of population structure analysis (e.g., DAPC) to define population groups.

---

## Step 1: Install GADMA in a Conda Environment

GADMA is a Python-based tool for inferring demographic history using allele frequency spectra. It's best installed in a fresh conda environment.

```bash
# Create and activate a new conda environment
# Remember a clean environment avoids dependency conflicts and ensures GADMA functions as intended.
conda create -n gadma_env python=3.10 -y
conda activate gadma_env

# Install GADMA
pip install gadma
```

## Step 2: Prepare the VCF and Population Map
We assume your population groups were inferred via DAPC and saved to a CSV file (dapc_groups.csv):

```r
# Example: Save groups from R
write.csv(data.frame(Individual = indNames(gl), Population = grp$grp), 
          "dapc_groups.csv", row.names = FALSE)
```

