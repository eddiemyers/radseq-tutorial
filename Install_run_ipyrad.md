# RADseq Data Workflow for Population Genomics

This guide walks you through installing the tools, running `ipyrad` for RADseq assembly, and performing downstream population structure analyses in R.

---

## 1. Installing Conda (if not already installed)

Download and install **Miniconda** (recommended over full Anaconda):

**Linux/macOS:**
```bash
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
bash Miniconda3-latest-Linux-x86_64.sh
```

## 2. Create and Activate a Conda Environment for ipyrad
```bash
conda create -n ipyrad_env python=3.9
conda activate ipyrad_env
```

## 3. Install ipyrad
```bash
conda install -c bioconda -c conda-forge ipyrad
```

## 4. Running ipyrad
Initialize params file:
```bash
ipyrad -n myproject
```
Edit the params-myproject.txt file with your sample information and specific settings.
```bash
vi params-myproject.txt
```
Run assembly (setting number of cores sensibly):
```bash
ipyrad -p params-myproject.txt -s 1234567 -c 8
```
