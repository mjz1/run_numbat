# Run Numbat for Allele-Specific scRNA Copy Number

## Introduction

This is a wrapper for the `Numbat` copy number called for scRNA. Please refer to the [original Github repo]((https://github.com/kharchenkolab/numbat)) or the [manuscript](https://www.nature.com/articles/s41587-022-01468-y) for more information.

## How to run

To run a single sample use the `run_numbat.py` script as shown below. This script is a wrapper around Numbat's `pileup_and_phase.R` script, which performs SNP pileups and phasing preprocessing, and a custom written `numbat.R` script, which runs Numbat. 

If running with cell line or PDX or high purity tumors, ensure that `--high_purity` is set. This will detect clonal and exclude regions of clonal deletions/LOH prior to running numbat.

Samples from the same donor can be run simultaneously by passing comma delimited sample names, bam files, barcode files, and matrix directories to `--samples`, `--bams`, `--barcodes`, and `--mtxdirs` respectively. Note that this will typically require increased memory.

If a sample fails during the `numbat.R` script, simply pass the same submission command and the existing pileups will be detected and used.

## Using the Docker image

Defaults are built to work the provided docker image. The latest docker image for Numbat can be pulled as follows:
```bash
singularity pull numbat-rbase_latest.sif docker://pkharchenkolab/numbat-rbase:latest
```


## Usage help
```
usage: run_numbat.py [-h] [--numbat_img NUMBAT_IMG] [--pileup_script PILEUP_SCRIPT]
                     [--numbat_rscript NUMBAT_RSCRIPT] [--gmap GMAP] [--panel_dir PANEL_DIR]
                     [--snp_vcf SNP_VCF] [--patient PATIENT] [--samples SAMPLES] [--bams BAMS]
                     [--barcodes BARCODES] [--mtxdirs MTXDIRS] [--outdir OUTDIR] [--UMItag UMITAG]
                     [--cellTAG CELLTAG] [--genome_ver {hg19,hg38}] [--cores CORES] [--mem MEM]
                     [--walltime WALLTIME] [--trans TRANS] [--gamma GAMMA] [--min_cells MIN_CELLS]
                     [--min_LLR MIN_LLR] [--multi_allelic] [--high_purity]

Run the Numbat allele specific scRNA copy number pipeline

optional arguments:
  -h, --help            show this help message and exit
  --numbat_img NUMBAT_IMG
                        The numbat image file
  --pileup_script PILEUP_SCRIPT
                        The numbat preprocessing pileup and phasing Rscript (default:
                        /numbat/inst/bin/pileup_and_phase.R)
  --numbat_rscript NUMBAT_RSCRIPT
                        The Rscript to run numbat
  --gmap GMAP           Location of the Eagle genetic map for phasing (default:
                        /Eagle_v2.4.1/tables/genetic_map_hg38_withX.txt.gz)
  --panel_dir PANEL_DIR
                        Directory of the 1000g reference panel (default: /data/1000G_hg38/)
  --snp_vcf SNP_VCF     SNP vcf (default: /data/genome1K.phase3.SNP_AF5e2.chr1toX.hg38.vcf)
  --patient PATIENT     Patient name.
  --samples SAMPLES     Comma seperated list of samples
  --bams BAMS           Comma seperated list of sample bams.
  --barcodes BARCODES   Comma seperated list of sample barcode files
  --mtxdirs MTXDIRS     Comma seperated list of matrix folders (filtered_feature_bc_matrix folder from
                        10X cellranger)
  --outdir OUTDIR       output directory
  --UMItag UMITAG       UMItag option for pileup script (default: Auto)
  --cellTAG CELLTAG     cellTAG option for pileup script (default: CB)
  --genome_ver {hg19,hg38}
                        Genome version (hg19, hg38) (default: hg38)
  --cores CORES         number of cores to use (default: 4)
  --mem MEM             amount of memory to use (default: 8)
  --walltime WALLTIME   amount of walltime to use (default: 48:00)
  --trans TRANS         Numbat HMM transmission probability (default: 1e-05)
  --gamma GAMMA         Numbat overdispersion parameter in allele counts (default: 20)
  --min_cells MIN_CELLS
                        Numbat minimum number of cells for which an pseudobulk HMM will be run
                        (default: 20)
  --min_LLR MIN_LLR     Numbat minimum log-likelihood ratio threshold to filter CNVs by. (default: 50)
  --multi_allelic       Flag to enable multi-allelic calling (default: True)
  --high_purity         Flag to detect and exclude regions of clonal deletions/LOH before running
                        Numbat. Recommended for cell line data or high-purity tumors (default: False)
```