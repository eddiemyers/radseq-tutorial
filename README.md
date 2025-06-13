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
---

## 2. Create and Activate a Conda Environment for ipyrad

conda create -n ipyrad_env python=3.9
conda activate ipyrad_env
