---
title: Fastplong
parent: QC and Preprocessing of Raw Sequencing Data
nav_order: 2
nav_exclude: false
permalink: /fastplong/
---

# Quality control of ONT reads with `fastplong`

## 🎯 Learning goals

By the end of this practical, you should be able to:

- explain why read quality control is important before downstream analysis
- run `fastplong` on Oxford Nanopore FASTQ files
- generate filtered FASTQ, HTML, and JSON output files
- filter reads by minimum length
- filter reads by mean read quality
- compare quality-control results generated with different parameters
- interpret the effect of filtering on read retention, read length, and quality

---

## Overview

`fastplong` is a preprocessing and quality-control tool optimized for
Oxford Nanopore and other long-read FASTQ data. It can perform:

- quality assessment
- quality filtering
- length filtering
- adapter trimming
- HTML and JSON report generation

In this practical, we will analyse modern R10.4.1 *Klebsiella pneumoniae*
reads. We will first run `fastplong` with its default parameters and then
compare the results with explicit length and mean-quality thresholds.

The dataset contains the following samples:

| Sample | FASTQ file |
|---|---|
| `AR-1049-MV` | `AR-1049-MV.fastq.gz` |
| `AR-1054-GC` | `AR-1054-GC.fastq.gz` |
| `AR-1049-GC` | `AR-1049-GC.fastq.gz` |
| `AR-1054` | `AR-1054.fastq.gz` |

---

## 1. Activate the Conda environment

Create and activate a Conda environment containing `fastplong`.

```bash
conda create -n fastplong   -c conda-forge   -c bioconda   fastplong   -y

conda activate fastplong
```

Check that `fastplong` is available:

```bash
fastplong --version
```

If the command prints a version number, the environment is ready.

---

## 2. Prepare the working directory

Move to the main workshop directory:

```bash
cd ~/2026-bioinf-ToT-EPIC-Namibia
```

Create a working directory for this practical:

```bash
mkdir -p analysis/fastplong
cd analysis/fastplong
```

Create directories for the input reads and the different filtering
experiments:

```bash
mkdir -p input
mkdir -p filtered_reads/default_params
mkdir -p filtered_reads/by_length_500
mkdir -p filtered_reads/by_quality
```

Create symbolic links to the downloaded FASTQ files. Symbolic links point to
the original files and avoid creating additional copies.

```bash
ln -s   ~/2026-bioinf-ToT-EPIC-Namibia/data/raw_data/kp_ont/*.fastq.gz   input/
```

Check the links:

```bash
ls -lh input/
```

Expected files:

```bash
input/
├── AR-1049-GC.fastq.gz
├── AR-1049-MV.fastq.gz
├── AR-1054.fastq.gz
└── AR-1054-GC.fastq.gz
```

{: .note }
If the symbolic links already exist, `ln` may report that the files exist.
This does not affect the practical.

---

## 3. Basic `fastplong` syntax

The basic structure of a `fastplong` command is:

```bash
fastplong \
  --in input_reads.fastq.gz \
  --out filtered_reads.fastq.gz \
  --html fastplong.html \
  --json fastplong.json
```

Important options used in this practical:

| Option | Meaning |
|---|---|
| `--in` | Input FASTQ file |
| `--out` | Output FASTQ file after filtering and trimming |
| `--html` | Output HTML report |
| `--json` | Output JSON report |
| `--verbose` | Print detailed progress information |
| `--dont_overwrite` | Prevent existing output files from being overwritten |
| `--length_required` | Minimum read length required to pass |
| `--mean_qual` | Minimum mean read quality required to pass |

For ONT reads, use `--in` and `--out` rather than the paired-end options
commonly used for short-read data.

---

## 4. Run `fastplong` with default parameters

First, run `fastplong` on one sample using its default settings:

```bash
fastplong   \
  --verbose \
  --dont_overwrite \
  --in input/AR-1049-GC.fastq.gz \
  --out filtered_reads/default_params/AR-1049-GC_fastplong.fastq.gz \
  --html filtered_reads/default_params/AR-1049-GC_fastplong.html \
  --json filtered_reads/default_params/AR-1049-GC_fastplong.json
```

This command creates:

- a filtered FASTQ file
- an HTML report
- a JSON report

Check the output files:

```bash
ls -lh filtered_reads/default_params/
```

---

## 5. Open and inspect the HTML report

Open one of the HTML reports in a web browser:

Examine the report and answer the following questions:

1. How many reads were present before filtering?
2. How many reads passed filtering?
3. What percentage of reads was retained?
4. What was the read N50 before and after filtering?
5. Did the mean read quality change?
6. Were adapters detected and trimmed?

{: .note }
On a remote server without a graphical desktop, copy the HTML report to your
local computer and open it in a local web browser.

---

## 6. Filter reads by length

Length filtering removes reads shorter than a selected minimum length. This
can be useful for excluding very short fragments that contribute little to
long-read assembly.

The minimum-length option is:

```bash
--length_required <length>
```

The equivalent short option is:

```bash
-l <length>
```

Check the default value:

```bash
fastplong --help | grep length_required
```

Now discard reads shorter than 500 bp:

```bash
fastplong \
  --verbose \
  --dont_overwrite \
  --length_required 500 \
  --in input/AR-1049-GC.fastq.gz \
  --out filtered_reads/by_length_500/AR-1049-GC_min500.fastq.gz \
  --html filtered_reads/by_length_500/AR-1049-GC_min500_fastplong.html \
  --json filtered_reads/by_length_500/AR-1049-GC_min500_fastplong.json
```

Check the output files:

```bash
ls -lh filtered_reads/by_length_500/
```

### 💬 Discussion

Compare the default and minimum-length reports:

1. How many reads were removed by the 500 bp threshold?
2. What percentage of bases was retained?
3. Did the read N50 change?
4. Would a 500 bp minimum meaningfully affect assembly of this dataset?

---

## 7. Filter reads by mean quality

Long-read filtering is commonly performed using the mean quality score of the
entire read.

The relevant option is:

```bash
--mean_qual <Q-score>
```

Run `fastplong` with a minimum mean read quality of Q10:

```bash
fastplong \
  --verbose \
  --dont_overwrite \
  --mean_qual 10 \
  --in input/AR-1049-GC.fastq.gz \
  --out filtered_reads/by_quality/AR-1049-GC_q10.fastq.gz \
  --html filtered_reads/by_quality/AR-1049-GC_q10_fastplong.html \
  --json filtered_reads/by_quality/AR-1049-GC_q10_fastplong.json
```

{: .note }
The reads were originally basecalled with a minimum read score of Q10.
Applying `--mean_qual 10` may therefore remove relatively few additional
reads.

---

## 8. Compare different quality thresholds

Run the same sample with Q30

### Q30

```bash
fastplong \
  --verbose \
  --dont_overwrite \
  --mean_qual 30 \
  --in input/AR-1049-GC.fastq.gz \
  --out filtered_reads/by_quality/AR-1049-GC_q30.fastq.gz \
  --html filtered_reads/by_quality/AR-1049-GC_q30_fastplong.html \
  --json filtered_reads/by_quality/AR-1049-GC_q30_fastplong.json
```

### 💬 Discussion

Compare the Q10, and Q30 reports:

1. How does the percentage of passing reads change?
2. How does the total number of retained bases change?
3. Which threshold gives the highest mean quality?
4. Which threshold preserves the most sequencing depth?
5. Is Q30 unnecessarily strict for these reads?
6. How does the selected threshold relate to the original Dorado Q10
   basecalling cutoff?

---

## 9. Combine the reports with MultiQC

Opening every `fastplong` HTML report separately becomes inconvenient when
several samples and filtering parameters have been analysed. MultiQC can scan
the `fastplong` JSON reports and combine the main before- and after-filtering
statistics into one interactive HTML report.

Install MultiQC in the active Conda environment:

```bash
conda create -n multiqc -c conda-forge -c bioconda multiqc -y

conda activete multiqc
```

Create an output directory and run MultiQC on all `fastplong` results:

```bash
cd ~/2026-bioinf-ToT-EPIC-Namibia
mkdir -p ~/2026-bioinf-ToT-EPIC-Namibia/analysis/multiqc

multiqc \
  analysis/fastplong/filtered_reads/ \
  --module fastp \
  --filename fastplong_multiqc.html \
  --outdir analysis/multiqc \
  --force
```

{: .note }
MultiQC currently reads the `fastplong` JSON files through its `fastp` module
because the two tools use a compatible report structure. The section in the
combined report is therefore labelled **fastp**, although the input reports
were generated by `fastplong`.

The command recursively searches these directories:

```bash
filtered_reads/
├── default_params/
├── by_length_500/
└── by_quality/
```

The main output files are:

```bash
multiqc/
├── fastplong_multiqc.html
└── fastplong_multiqc_data/
```

Open the combined report and inspect it. 


{: .note }
MultiQC summarises the main filtering statistics across samples and parameter
sets. Keep the original `fastplong` HTML reports for detailed plots that are
not reproduced in the combined report.

### 💬 Discussion

Use the MultiQC report to compare the samples and filtering strategies:

1. Which analysis retained the largest percentage of reads?
2. Which parameter removed the largest number of reads?
3. Did length filtering remove many reads or bases?
4. How did Q10 and Q30 affect read retention?
5. Are the filtering effects consistent across all four samples?

---

## 📌 Summary

In this practical, you used `fastplong` to perform long-read quality control
and filtering.

The most appropriate filtering threshold depends on the sequencing chemistry,
basecalling model, read-quality distribution, sequencing depth, and intended
downstream analysis. Filtering should therefore be evaluated by comparing both
the retained read quality and the amount of data that remains.