# nanopore-metagenomics-pipeline
End-to-end Oxford Nanopore metagenomics workflow for quality control, metaFlye assembly, genome binning, MAG reconstruction, CheckM quality assessment, and GTDB-Tk taxonomic classification on HPC systems.
# Nanopore Metagenomics Pipeline

## Overview

This repository contains an end-to-end bioinformatics workflow for analyzing Oxford Nanopore long-read metagenomic sequencing data. The pipeline performs quality control, metagenome assembly, gene prediction, genome binning, quality assessment, and taxonomic classification of microbial genomes recovered from complex metagenomic samples.

---

## Objectives

* Process raw Oxford Nanopore sequencing reads
* Assemble metagenomic contigs using long-read assembly
* Predict genes and proteins
* Reconstruct metagenome-assembled genomes (MAGs)
* Assess genome completeness and contamination
* Classify microbial genomes taxonomically
* Generate a framework for downstream functional analysis

---

## Workflow

```text
Raw Nanopore Reads
        │
        ▼
NanoPlot / NanoFilt
        │
        ▼
metaFlye Assembly
        │
        ▼
Prodigal Gene Prediction
        │
        ▼
Minimap2 Mapping
        │
        ▼
Samtools Processing
        │
        ▼
Coverage Estimation
        │
        ▼
MetaBAT2 Binning
        │
        ▼
CheckM Quality Assessment
        │
        ▼
GTDB-Tk Taxonomic Classification
        │
        ▼
Functional Annotation (Future Work)
```

## Tools Used

| Tool             | Purpose                    |
| ---------------- | -------------------------- |
| NanoPlot         | Read quality assessment    |
| NanoFilt         | Read filtering             |
| Flye (meta mode) | Metagenome assembly        |
| Prodigal         | Gene prediction            |
| Minimap2         | Read mapping               |
| Samtools         | BAM processing             |
| MetaBAT2         | Genome binning             |
| CheckM           | MAG quality assessment     |
| GTDB-Tk          | Taxonomic classification   |
| MetaQUAST        | Assembly evaluation        |
| Nextflow         | Workflow management        |
| SLURM HPC        | High-performance computing |

---

## Dataset Summary

### Input

* Oxford Nanopore long-read metagenomic sequencing data
* Total sequencing yield: ~24.3 Gb

### Assembly Statistics

| Metric            | Value          |
| ----------------- | -------------- |
| Assembly size     | 311,250,233 bp |
| Number of contigs | 6,581          |
| Largest contig    | 3,566,320 bp   |

### Gene Prediction

| Metric            | Value   |
| ----------------- | ------- |
| Predicted genes   | 318,224 |
| Protein sequences | 318,224 |

### Binning Results

| Metric                   | Value |
| ------------------------ | ----- |
| Initial bins             | 254   |
| Medium/High-quality MAGs | 30    |

---

## Taxonomic Classification

GTDB-Tk identified representative bacterial genomes including:

* Porphyromonas pasteri
* Bulleidia sp.
* Catonella sp.
* Oribacterium sp.

Several MAGs remained unclassified due to insufficient marker gene coverage, indicating either incomplete genomes or potentially novel microbial diversity.

---

## Example Commands

### Assembly

```bash
flye \
  --nano-raw filtered.fastq \
  --meta \
  --threads 40 \
  --out-dir flye_out
```

### Gene Prediction

```bash
prodigal \
  -i assembly.fasta \
  -o genes.gff \
  -a proteins.faa \
  -d genes.fna \
  -p meta
```

### Mapping

```bash
minimap2 -ax map-ont assembly.fasta filtered.fastq | \
samtools sort -o reads_to_assembly.sorted.bam
```

### Binning

```bash
metabat2 \
  -i assembly.fasta \
  -a depth.txt \
  -o bins/bin
```

### Quality Assessment

```bash
checkm lineage_wf bins checkm -t 24
```

### Taxonomy

```bash
gtdbtk classify_wf \
  --genome_dir good_bins \
  --out_dir gtdbtk \
  --extension fa \
  --cpus 24
```

---

## Key Skills Demonstrated

* Metagenomics
* Microbiome Analysis
* Long-read Genome Assembly
* Metagenome-Assembled Genomes (MAGs)
* Genome Binning
* Taxonomic Classification
* Linux Command Line
* HPC Computing
* Workflow Development
* Nextflow
* Bioinformatics Pipeline Design

---

## Future Work

* Functional annotation using eggNOG-mapper
* KEGG pathway analysis
* Antibiotic resistance gene identification
* Virulence factor screening
* Comparative genome analysis
* Microbial community visualization

---

## Author

**Dr. Krupali V. Poharkar**

PhD Biotechnology (Microbiology)
Computational Biology | Bioinformatics | Metagenomics | Microbiome Research

GitHub: https://github.com/<your-github-username>
LinkedIn: https://linkedin.com/in/<your-linkedin-profile>
