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
This will open a file that looks like this:
```text
------- ipyrad params file (v.0.9.102)------------------------------------------
myproject                      ## [0] [assembly_name]: Assembly name. Used to name output directories for assembly steps
/home/emyers/Leioheterodon     ## [1] [project_dir]: Project dir (made in curdir if not present)
                               ## [2] [raw_fastq_path]: Location of raw non-demultiplexed fastq files
                               ## [3] [barcodes_path]: Location of barcodes file
                               ## [4] [sorted_fastq_path]: Location of demultiplexed/sorted fastq files
denovo                         ## [5] [assembly_method]: Assembly method (denovo, reference)
                               ## [6] [reference_sequence]: Location of reference sequence file
rad                            ## [7] [datatype]: Datatype (see docs): rad, gbs, ddrad, etc.
TGCAG,                         ## [8] [restriction_overhang]: Restriction overhang (cut1,) or (cut1, cut2)
5                              ## [9] [max_low_qual_bases]: Max low quality base calls (Q<20) in a read
33                             ## [10] [phred_Qscore_offset]: phred Q score offset (33 is default and very standard)
6                              ## [11] [mindepth_statistical]: Min depth for statistical base calling
6                              ## [12] [mindepth_majrule]: Min depth for majority-rule base calling
10000                          ## [13] [maxdepth]: Max cluster depth within samples
0.85                           ## [14] [clust_threshold]: Clustering threshold for de novo assembly
0                              ## [15] [max_barcode_mismatch]: Max number of allowable mismatches in barcodes
2                              ## [16] [filter_adapters]: Filter for adapters/primers (1 or 2=stricter)
35                             ## [17] [filter_min_trim_len]: Min length of reads after adapter trim
2                              ## [18] [max_alleles_consens]: Max alleles per site in consensus sequences
0.05                           ## [19] [max_Ns_consens]: Max N's (uncalled bases) in consensus
0.05                           ## [20] [max_Hs_consens]: Max Hs (heterozygotes) in consensus
4                              ## [21] [min_samples_locus]: Min # samples per locus for output
0.2                            ## [22] [max_SNPs_locus]: Max # SNPs per locus
8                              ## [23] [max_Indels_locus]: Max # of indels per locus
0.5                            ## [24] [max_shared_Hs_locus]: Max # heterozygous sites per locus
0, 0, 0, 0                     ## [25] [trim_reads]: Trim raw read edges (R1>, <R1, R2>, <R2) (see docs)
0, 0, 0, 0                     ## [26] [trim_loci]: Trim locus edges (see docs) (R1>, <R1, R2>, <R2)
p, s, l                        ## [27] [output_formats]: Output formats (see docs)
                               ## [28] [pop_assign_file]: Path to population assignment file
                               ## [29] [reference_as_filter]: Reads mapped to this reference are removed in step 3
```
There's a lot of details about this params file here: https://ipyrad.readthedocs.io/en/master/tutorial_intro_cli.html#create-an-ipyrad-params-file. For now we will focus on editing the 'sorted_fastq_path', 'barcodes_path', 'reference_sequence', and 'assembly_method'. All of these parameters can influence the final output files, and we might want to consider the minimum number of samples to call a SNP and the output formats we want now.


If we're happy about the params file we can go ahead and run the assembly (setting number of cores sensibly). Note that this will run the program with all steps, sometimes it's useful to run one step at a time (particularly for troubleshooting):
```bash
ipyrad -p params-myproject.txt -s 1234567 -c 8
```
Note that the HPC at the Academy ins't using a job scheduler, so in order to exit the terminal without killing the job we need to do the following:
```bash
Control + z
bg
disown
```
