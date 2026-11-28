---
title: Fastplong
parent: Read QC and Filtering
nav_order: 2
nav_exclude: false
permalink: /fastplong/
---

# Quality control of ONT reads with `fastplong`


{: .objectives }
> By the end of this practical, you should be able to:
> 
> - run `fastplong` on Oxford Nanopore FASTQ files to generate filtered FASTQ, HTML, and JSON output files
> - compare quality-control results generated with different parameters
> - interpret the effect of filtering on read retention, read length, and quality

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
reads. Everyone first runs `fastplong` with its default parameters. Then the
group runs eight filtering conditions together and compares the results.

The dataset contains the following samples:

| Sample | FASTQ file |
|---|---|
| `AR-1049-MV` | `AR-1049-MV.fastq.gz` |
| `AR-1054-GC` | `AR-1054-GC.fastq.gz` |
| `AR-1049-GC` | `AR-1049-GC.fastq.gz` |
| `AR-1054` | `AR-1054.fastq.gz` |

In this practical, we work with `AR-1049-GC`.

---

## 1. Activate the Conda environment

Create and activate a Conda environment containing `fastplong`.

<details markdown="1">
<summary>Solution</summary>

```bash
conda create -n fastplong -c conda-forge -c bioconda fastplong -y

conda activate fastplong
```

</details>

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

Create a working directory for this practical and move into it:

```bash
mkdir -p analyses/fastplong
cd analyses/fastplong
```

Create directories for the input reads and the different filtering
conditions:

```bash
mkdir -p input
mkdir -p filtered_reads/default_params
mkdir -p filtered_reads/no_defaults
mkdir -p filtered_reads/by_quality
mkdir -p filtered_reads/by_length_1000
mkdir -p filtered_reads/by_length_5000
mkdir -p filtered_reads/by_length_10000
mkdir -p filtered_reads/combined
```

Create symbolic links to the downloaded FASTQ files. Symbolic links point to
the original files and avoid creating additional copies.

```bash
ln -s ~/2026-bioinf-ToT-EPIC-Namibia/data/raw_data/kp_ont/*.fastq.gz input/
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
| `--disable_quality_filtering` | Switch off the default quality filter |
| `--disable_adapter_trimming` | Switch off the default adapter trimming |
| `--disable_length_filtering` | Switch off the default length filter |
| `--disable_chimera_splitting` | Switch off splitting of reads at internal adapters |

For ONT reads, use `--in` and `--out` rather than the paired-end options
commonly used for short-read data.

Look up the default values yourself:

```bash
fastplong --help
```

---

## 4. Run `fastplong` with default parameters

Everyone runs this step.

Run `fastplong` on `AR-1049-GC` with its default settings. Write the output
files to `filtered_reads/default_params/` and name them
`AR-1049-GC_fastplong.fastq.gz`, `AR-1049-GC_fastplong.html` and
`AR-1049-GC_fastplong.json`.

<details markdown="1">
<summary>Solution</summary>

```bash
fastplong \
  --verbose \
  --dont_overwrite \
  --in input/AR-1049-GC.fastq.gz \
  --out filtered_reads/default_params/AR-1049-GC_fastplong.fastq.gz \
  --html filtered_reads/default_params/AR-1049-GC_fastplong.html \
  --json filtered_reads/default_params/AR-1049-GC_fastplong.json
```

</details>

Check the output files:

```bash
ls -lh filtered_reads/default_params/
```

---

## 5. Open and inspect the HTML report

Open the HTML report in a web browser, for example by double-clicking it in
the file explorer of VSCodium.

Examine the report and answer the following questions:

1. How many reads and bases were present before filtering?
2. How many reads passed filtering? What percentage is that?
3. What are the median read length and the read N50? Why are they so different?
4. Did the mean read quality change after filtering?
5. Were adapters detected and trimmed? How many chimeric reads were split?
6. In the plot **Cumulative yield by minimum read length**: what percentage
   of reads and of bases would remain with a minimum length of 1,000 bp?
7. In the plot **Read mean quality distribution**: where is the peak?

{: .note }
On a remote server without a graphical desktop, copy the HTML report to your
local computer and open it in a local web browser.

---

## 6. Filtering conditions: divide and conquer

The group runs eight filtering conditions on `AR-1049-GC`. Each condition is
on a card on the flipchart.

1. Pick a card and mark it as taken.
2. **Before you run:** use the plots from your default report to predict what
   percentage of reads and bases will remain. Write down your prediction.
3. Write the command yourself, then check it against the solution.
4. Open your report and add your results to the results grid on the flipchart.
5. Pick the next free card. We continue when all eight conditions have run.

Results grid:

| Condition | Reads kept (%) | Bases kept (%) | Depth (bases ÷ 5.5 Mb) | N50 (bp) | Prediction correct? |
|---|---|---|---|---|---|

### Condition 1: No defaults

Switch off quality filtering, adapter trimming, length filtering and chimera
splitting. Write the output to `filtered_reads/no_defaults/` with the
prefix `AR-1049-GC_no_defaults`.

<details markdown="1">
<summary>Solution</summary>

```bash
fastplong \
  --verbose \
  --dont_overwrite \
  --disable_quality_filtering \
  --disable_adapter_trimming \
  --disable_length_filtering \
  --disable_chimera_splitting \
  --in input/AR-1049-GC.fastq.gz \
  --out filtered_reads/no_defaults/AR-1049-GC_no_defaults.fastq.gz \
  --html filtered_reads/no_defaults/AR-1049-GC_no_defaults_fastplong.html \
  --json filtered_reads/no_defaults/AR-1049-GC_no_defaults_fastplong.json
```

</details>

### Condition 2: Mean quality 30

Keep only reads with a mean quality of at least Q30. Write the output to
`filtered_reads/by_quality/` with the prefix `AR-1049-GC_q30`.

<details markdown="1">
<summary>Solution</summary>

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

</details>

### Condition 3: Mean quality 40

Keep only reads with a mean quality of at least Q40. Write the output to
`filtered_reads/by_quality/` with the prefix `AR-1049-GC_q40`.

<details markdown="1">
<summary>Solution</summary>

```bash
fastplong \
  --verbose \
  --dont_overwrite \
  --mean_qual 40 \
  --in input/AR-1049-GC.fastq.gz \
  --out filtered_reads/by_quality/AR-1049-GC_q40.fastq.gz \
  --html filtered_reads/by_quality/AR-1049-GC_q40_fastplong.html \
  --json filtered_reads/by_quality/AR-1049-GC_q40_fastplong.json
```

</details>

### Condition 4: Minimum length 1,000 bp

Discard reads shorter than 1,000 bp. Write the output to
`filtered_reads/by_length_1000/` with the prefix `AR-1049-GC_min1000`.

<details markdown="1">
<summary>Solution</summary>

```bash
fastplong \
  --verbose \
  --dont_overwrite \
  --length_required 1000 \
  --in input/AR-1049-GC.fastq.gz \
  --out filtered_reads/by_length_1000/AR-1049-GC_min1000.fastq.gz \
  --html filtered_reads/by_length_1000/AR-1049-GC_min1000_fastplong.html \
  --json filtered_reads/by_length_1000/AR-1049-GC_min1000_fastplong.json
```

</details>

### Condition 5: Minimum length 5,000 bp

Discard reads shorter than 5,000 bp. Write the output to
`filtered_reads/by_length_5000/` with the prefix `AR-1049-GC_min5000`.

<details markdown="1">
<summary>Solution</summary>

```bash
fastplong \
  --verbose \
  --dont_overwrite \
  --length_required 5000 \
  --in input/AR-1049-GC.fastq.gz \
  --out filtered_reads/by_length_5000/AR-1049-GC_min5000.fastq.gz \
  --html filtered_reads/by_length_5000/AR-1049-GC_min5000_fastplong.html \
  --json filtered_reads/by_length_5000/AR-1049-GC_min5000_fastplong.json
```

</details>

### Condition 6: Minimum length 10,000 bp

Discard reads shorter than 10,000 bp. Write the output to
`filtered_reads/by_length_10000/` with the prefix `AR-1049-GC_min10000`.

<details markdown="1">
<summary>Solution</summary>

```bash
fastplong \
  --verbose \
  --dont_overwrite \
  --length_required 10000 \
  --in input/AR-1049-GC.fastq.gz \
  --out filtered_reads/by_length_10000/AR-1049-GC_min10000.fastq.gz \
  --html filtered_reads/by_length_10000/AR-1049-GC_min10000_fastplong.html \
  --json filtered_reads/by_length_10000/AR-1049-GC_min10000_fastplong.json
```

</details>

### Condition 7: Minimum length 1,000 bp + mean quality 30

Combine both filters. Write the output to `filtered_reads/combined/` with the
prefix `AR-1049-GC_min1000_q30`.

<details markdown="1">
<summary>Solution</summary>

```bash
fastplong \
  --verbose \
  --dont_overwrite \
  --length_required 1000 \
  --mean_qual 30 \
  --in input/AR-1049-GC.fastq.gz \
  --out filtered_reads/combined/AR-1049-GC_min1000_q30.fastq.gz \
  --html filtered_reads/combined/AR-1049-GC_min1000_q30_fastplong.html \
  --json filtered_reads/combined/AR-1049-GC_min1000_q30_fastplong.json
```

</details>

### Condition 8: Minimum length 1,000 bp + mean quality 40

Combine both filters with a stricter quality threshold. Write the output to
`filtered_reads/combined/` with the prefix `AR-1049-GC_min1000_q40`.

<details markdown="1">
<summary>Solution</summary>

```bash
fastplong \
  --verbose \
  --dont_overwrite \
  --length_required 1000 \
  --mean_qual 40 \
  --in input/AR-1049-GC.fastq.gz \
  --out filtered_reads/combined/AR-1049-GC_min1000_q40.fastq.gz \
  --html filtered_reads/combined/AR-1049-GC_min1000_q40_fastplong.html \
  --json filtered_reads/combined/AR-1049-GC_min1000_q40_fastplong.json
```

</details>

### 💬 Discussion

Use the results grid to compare the conditions:

1. What did the default settings remove compared with *No defaults*?
2. Which condition removed the most reads? Which removed the most bases?
3. Why does a length filter remove many reads but few bases?
4. What would `--mean_qual 10` do? Use the mean quality plot to answer
   without running it.
5. Does combining length and quality filters remove more than each filter
   alone? Why so little?
6. Which filtered file would you use for genome assembly, and why?
7. Which filter could you still afford if the sample had only 40× depth?

---

## 7. Combine the reports with MultiQC

Opening every `fastplong` HTML report separately becomes inconvenient when
several filtering conditions have been analysed. MultiQC can scan the
`fastplong` JSON reports and combine the main before- and after-filtering
statistics into one interactive HTML report.

Create and activate a new Conda environment containing MultiQC.

<details markdown="1">
<summary>Solution</summary>

```bash
conda create -n multiqc -c conda-forge -c bioconda multiqc -y

conda activate multiqc
```

</details>

Create an output directory and run MultiQC on all `fastplong` results:

```bash
cd ~/2026-bioinf-ToT-EPIC-Namibia
mkdir -p analyses/multiqc

multiqc \
  analyses/fastplong/filtered_reads/ \
  --module fastp \
  --filename fastplong_multiqc.html \
  --outdir analyses/multiqc \
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
├── no_defaults/
├── by_quality/
├── by_length_1000/
├── by_length_5000/
├── by_length_10000/
└── combined/
```

The main output files are:

```bash
multiqc/
├── fastplong_multiqc.html
└── fastplong_multiqc_data/
```

Open the combined report and inspect it.

{: .note }
MultiQC summarises the main filtering statistics across parameter sets. Keep
the original `fastplong` HTML reports for detailed plots that are not
reproduced in the combined report.

### 💬 Discussion

1. Do the MultiQC values match the results grid on the flipchart?
2. The column **% > Q30** is described as "Percentage of reads > Q30". Compare
   it with the **Q30 bases** value in your own HTML report. Is it reads or
   bases?
3. Why does the column **Reads After Filtering** show 0.0 M for one condition?

---

## 📌 Summary

In this practical, you used `fastplong` to perform long-read quality control
and filtering.

The most appropriate filtering threshold depends on the sequencing chemistry,
basecalling model, read-quality distribution, sequencing depth, and intended
downstream analysis. Filtering should therefore be evaluated by comparing both
the retained read quality and the amount of data that remains.
