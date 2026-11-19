---
title: Nanoplot
parent: QC and Preprocessing of Raw Sequencing Data
nav_order: 1
nav_exclude: false
permalink: /nanoplot/
---

# Quality control of ONT reads with `NanoPlot`

{: .objectives }
> By the end of this practical, you should be able to:
>
> - LO1

---

## Overview

---

## 1. 

---

## 2. 
---

## 3. 

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

## 4. 

### 5. Interpret the report
Open the `NanoPlot` report in VScodium.

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
> 1. How many reads were generated for `barcode08`?
> 2. What is the median read quality?
> 3. What is the median read length?
> 4. Is the read-length distribution consistent with approximately 2-kb amplicons?
> 5. Are there many very short or unusually long reads?
> 6. Based on the QC results, would you proceed with genome reconstruction?

---

## 5. 
---


## 📌 Summary

In this practical, you used `NanoPlot` to inspect the quality of ONT sequencing reads.

You examined:

- read quality
- read length
- read N50
- read-length distribution
- whether the reads match the expected amplicon size

The reads will next be used directly for genome reconstruction with `artic-mpxv-nf`.
