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

Format the file for easySFS to use:

```text
# Format: One line per sample, tab-delimited
sample1    pop1
sample2    pop1
sample3    pop2
...
```
Save this as popmap.txt.

## Step 3: Generate the SFS with easySFS
easySFS simplifies creation of folded/unfolded SFS from VCFs for input into demographic tools, allowing you to downsample as necessary to maximize the number of SNPs retained in your dataset.

Install easySFS
```bash
git clone https://github.com/isaacovercast/easySFS.git
cd easySFS
chmod +x easySFS.py
```
Run easySFS
```bash
# Preview projection values
./easySFS.py -i your_data.vcf -p popmap.txt -a --preview

# Select a projection and generate SFS
./easySFS.py -i your_data.vcf -p popmap.txt -a -f --proj 12,12 -o output_sfs
```
The projection reduces missing data bias and generates a frequency spectrum required by GADMA.

## Step 4: Run Demographic Modeling in GADMA
GADMA uses the two-population joint SFS (2D SFS) produced by easySFS to model divergence and migration histories.

Inputs
output_sfs/pop1-pop2.sfs: folded or unfolded 2D SFS

Population labels (e.g., pop1, pop2)

Optional time constraints

Run GADMA
```bash
# From the gadma_env environment
gadma --sfs output_sfs/pop1-pop2.sfs --pop_names pop1 pop2
```
What GADMA Does:
-GADMA fits demographic models (split, migration, bottlenecks) to your SFS.

-It uses genetic algorithms to search model space, then refines using optimization (e.g., moments or dadi).

-The result is an inferred demographic history with likelihood scores and parameter estimates.

Output Interpretation:
-Model plots showing population size changes and divergence times

-Parameter estimates (e.g., Ne, migration rates, split time)

-Log-likelihood scores for model comparisons

GADMA will save these in its output directory and optionally generate visual plots of demographic histories.

