---
title: Nanoplot
parent: Read QC and Filtering
nav_order: 1
nav_exclude: false
permalink: /nanoplot/
---

# Quality control of ONT reads with `NanoPlot`

{: .objectives }
> By the end of this practical, you should be able to:
>
> - run `NanoPlot` on Oxford Nanopore FASTQ files
> - inspect read length and read quality distributions
> - interpret basic ONT read quality statistics
> - decide whether the reads are suitable for downstream analyses

---

## Overview

`NanoPlot` is a quality-control and visualization tool for Oxford Nanopore sequencing data.

The input data are located in: `~/2026-bioinf-ToT-EPIC-Namibia/data/raw_data/kp_ont`

---

## 1. Setup the working environment

We are now working in the project folder, not in `scratch/` anymore.

Navigate to your project folder:

```bash
cd ~/2026-bioinf-ToT-EPIC-Namibia
```

Activate the `NanoPlot` environment:

```bash
conda activate nanoplot
```

Create your output directory:

```bash
mkdir -p analyses/nanoplot/
```

---

## 2. Inspect the data

View the whole file content without wrapped lines:

```bash
less -S data/raw_data/kp_ont/AR-1049-MV.fastq.gz
```

... with lines wrapped:

```bash
less data/raw_data/kp_ont/AR-1049-MV.fastq.gz
```

---

## 3. Run `NanoPlot` on toy data

Make sure you are in your project folder:

```bash
cd ~/2026-bioinf-ToT-EPIC-Namibia
```

Run `NanoPlot` on the FASTQ file from **AR-1049-MV**:

```bash
NanoPlot \
    --fastq data/raw_data/kp_ont/AR-1049-MV.fastq.gz \
    --title "AR-1049-MV (raw)" \
    --prefix AR-1049-MV_raw_ \
    --threads 8 \
    --outdir analyses/nanoplot/AR-1049-MV
```

{: .note }
The command is split into individual lines per option and argument for readability reasons. Alternatively, you can run a one-liner.

Run `NanoPlot` directly on the FASTQ file from **AR-1049-MV**:

```bash
NanoPlot --fastq data/raw_data/kp_ont/AR-1049-MV.fastq.gz --title "AR-1049-MV (raw)" --prefix AR-1049-MV_raw_ --N50 --threads 4 --outdir analyses/nanoplot/AR-1049-MV
```

Important options:

| Option | Meaning |
|---|---|
| `--fastq` | Input ONT FASTQ file(s) |
| `--title` | Title shown on top of every plot, useful to tell samples apart |
| `--prefix` | Prefix added to all output file names, so results from different runs don't overwrite each other |
| `--N50` | Marks the N50 read length in the read length histograms |
| `--threads` | Number of CPU threads to use |
| `--outdir` | Directory for the NanoPlot results |

---

## 4. Inspect the output files

List the generated files:

```bash
ls -lh analyses/nanoplot/AR-1049-MV/
```

Important outputs include:

```text
AR-1049-MV_raw_NanoPlot-report.html
AR-1049-MV_raw_NanoStats.txt
```

### 5. Interpret the report

Open the `NanoPlot` report in VSCodium.

Pay particular attention to:

- number of reads
- total number of bases
- mean and median read length
- read N50
- mean and median read quality
- read length distribution
- relationship between read length and read quality

{: .discussion }
> Use the NanoPlot report to answer:
>
> 1. How many reads were generated for `AR-1049-MV`?
> 2. What is the median read quality?
> 3. What is the median read length?
> 4. Are there many very short or unusually long reads?
> 5. Based on the QC results, would you proceed with genome reconstruction?

---

## 📌 Summary

In this practical, you used `NanoPlot` to inspect the quality of ONT reads.

You examined:

- read quality
- read length
- read N50
- read-length distribution
