---
title: "Chopper and NanoComp"
parent: QC and Preprocessing of Raw Sequencing Data
nav_order: 4
nav_exclude: false
permalink: /chopper_nanocomp/
---

# Extra: Filtering ONT reads with `chopper` and comparing them with `NanoComp`


{: .note }
This is an optional extra tutorial. It repeats the filtering experiment from
the [`fastplong` tutorial](../fastplong/) with two tools from the NanoPack
suite. Complete the `fastplong` tutorial first.

{: .objectives }
> By the end of this practical, you should be able to:
> 
> - filter Oxford Nanopore reads by length and mean quality with `chopper`
> - compare raw and filtered datasets with `NanoComp`
> - explain why the same quality threshold gives different results in
>   `chopper` and `fastplong`

---

## Overview

In this practical, we use two tools from the NanoPack suite:

- `chopper` filters reads by minimum length and mean read quality. It reads
  FASTQ from standard input and writes the reads that pass to standard output.
- `NanoComp` compares one or more datasets and produces summary statistics and
  plots of read length, read quality, and yield.

Compared with `fastplong`, the work is split between two tools: `chopper`
filters, `NanoComp` reports. `chopper` does not trim adapters or split
chimeric reads.

We use the same R10.4.1 *Klebsiella pneumoniae* sample as in the `fastplong`
tutorial: `AR-1049-GC`.

---

## 1. Activate the Conda environment

Create and activate a Conda environment called `readqc` containing `chopper`
and `nanocomp`.

<details markdown="1">
<summary>Solution</summary>

```bash
conda create -n readqc -c conda-forge -c bioconda chopper nanocomp -y

conda activate readqc
```

</details>

Check that both tools are available:

```bash
chopper --version
NanoComp --version
```

If both commands print a version number, the environment is ready.

---

## 2. Prepare the working directory

Move to the main workshop directory:

```bash
cd ~/2026-bioinf-ToT-EPIC-Namibia
```

Create a working directory for this practical and move into it:

```bash
mkdir -p analyses/chopper_nanocomp
cd analyses/chopper_nanocomp
```

Create directories for the input reads, the filtered reads, and the
`NanoComp` reports:

```bash
mkdir -p input
mkdir -p filtered_reads
mkdir -p nanocomp
```

Create a symbolic link to the raw FASTQ file:

```bash
ln -s ~/2026-bioinf-ToT-EPIC-Namibia/data/raw_data/kp_ont/AR-1049-GC.fastq.gz input/
```

Check the link:

```bash
ls -lh input/
```

---

## 3. Basic syntax

### `chopper`

`chopper` reads uncompressed FASTQ from standard input and writes to standard
output. We therefore decompress the input with `gunzip -c`, pipe it into
`chopper`, and compress the output with `gzip`:

```bash
gunzip -c input_reads.fastq.gz \
  | chopper --quality 10 --minlength 1000 --threads 4 \
  | gzip > filtered_reads.fastq.gz
```

| Option | Meaning |
|---|---|
| `-q`, `--quality` | Minimum mean read quality required to pass |
| `-l`, `--minlength` | Minimum read length required to pass |
| `--maxlength` | Maximum read length allowed |
| `-t`, `--threads` | Number of threads to use |

Without `--quality` and `--minlength`, `chopper` keeps all reads. Unlike
`fastplong`, it does not trim adapters or filter anything by default.

Look up all options yourself:

```bash
chopper --help
```

### `NanoComp`

```bash
NanoComp \
  --fastq file1.fastq.gz file2.fastq.gz \
  --names name1 name2 \
  --outdir output_folder \
  --threads 4
```

| Option | Meaning |
|---|---|
| `--fastq` | One or more FASTQ files to compare |
| `--names` | One name per file, in the same order, used in plots and tables |
| `--outdir` | Output directory for the report, statistics, and plots |
| `-t`, `--threads` | Number of threads to use |

---

## 4. Inspect the raw reads with `NanoComp`

Run `NanoComp` on the raw `AR-1049-GC` reads. Name the dataset `raw` and write
the output to `nanocomp/raw/`.

<details markdown="1">
<summary>Solution</summary>

```bash
NanoComp \
  --fastq input/AR-1049-GC.fastq.gz \
  --names raw \
  --outdir nanocomp/raw \
  --threads 4
```

</details>

Open the HTML report in `nanocomp/raw/` in a web browser, and open
`NanoStats.txt` in the editor.

Answer the following questions:

1. How many reads and bases does the dataset contain? Do the numbers match the
   "before filtering" values in your `fastplong` report?
2. What are the median read length and the read N50?
3. What are the mean and median read quality? Compare them with the mean
   quality in the `fastplong` report.
4. In `NanoStats.txt`: what percentage of reads is above Q10, Q15, and Q20?

---

## 5. Filter the reads with `chopper`

Run as many of the following conditions as you like. **Before each run**, use
your raw report and `NanoStats.txt` to predict what percentage of reads and
bases will remain.

### Condition 1: Minimum length 1,000 bp

Discard reads shorter than 1,000 bp. Write the output to
`filtered_reads/AR-1049-GC_min1000.fastq.gz`.

<details markdown="1">
<summary>Solution</summary>

```bash
gunzip -c input/AR-1049-GC.fastq.gz \
  | chopper --minlength 1000 --threads 4 \
  | gzip > filtered_reads/AR-1049-GC_min1000.fastq.gz
```

</details>

### Condition 2: Minimum length 5,000 bp

Discard reads shorter than 5,000 bp. Write the output to
`filtered_reads/AR-1049-GC_min5000.fastq.gz`.

<details markdown="1">
<summary>Solution</summary>

```bash
gunzip -c input/AR-1049-GC.fastq.gz \
  | chopper --minlength 5000 --threads 4 \
  | gzip > filtered_reads/AR-1049-GC_min5000.fastq.gz
```

</details>

### Condition 3: Minimum length 10,000 bp

Discard reads shorter than 10,000 bp. Write the output to
`filtered_reads/AR-1049-GC_min10000.fastq.gz`.

<details markdown="1">
<summary>Solution</summary>

```bash
gunzip -c input/AR-1049-GC.fastq.gz \
  | chopper --minlength 10000 --threads 4 \
  | gzip > filtered_reads/AR-1049-GC_min10000.fastq.gz
```

</details>

### Condition 4: Mean quality 10

Keep only reads with a mean quality of at least Q10. Write the output to
`filtered_reads/AR-1049-GC_q10.fastq.gz`.

<details markdown="1">
<summary>Solution</summary>

```bash
gunzip -c input/AR-1049-GC.fastq.gz \
  | chopper --quality 10 --threads 4 \
  | gzip > filtered_reads/AR-1049-GC_q10.fastq.gz
```

</details>

### Condition 5: Mean quality 15

Keep only reads with a mean quality of at least Q15. Write the output to
`filtered_reads/AR-1049-GC_q15.fastq.gz`.

<details markdown="1">
<summary>Solution</summary>

```bash
gunzip -c input/AR-1049-GC.fastq.gz \
  | chopper --quality 15 --threads 4 \
  | gzip > filtered_reads/AR-1049-GC_q15.fastq.gz
```

</details>

### Condition 6: Mean quality 20

Keep only reads with a mean quality of at least Q20. Write the output to
`filtered_reads/AR-1049-GC_q20.fastq.gz`.

<details markdown="1">
<summary>Solution</summary>

```bash
gunzip -c input/AR-1049-GC.fastq.gz \
  | chopper --quality 20 --threads 4 \
  | gzip > filtered_reads/AR-1049-GC_q20.fastq.gz
```

</details>

### Condition 7: Minimum length 1,000 bp + mean quality 15

Combine both filters. Write the output to
`filtered_reads/AR-1049-GC_min1000_q15.fastq.gz`.

<details markdown="1">
<summary>Solution</summary>

```bash
gunzip -c input/AR-1049-GC.fastq.gz \
  | chopper --minlength 1000 --quality 15 --threads 4 \
  | gzip > filtered_reads/AR-1049-GC_min1000_q15.fastq.gz
```

</details>

### Condition 8: Minimum length 1,000 bp + mean quality 20

Combine both filters with a stricter quality threshold. Write the output to
`filtered_reads/AR-1049-GC_min1000_q20.fastq.gz`.

<details markdown="1">
<summary>Solution</summary>

```bash
gunzip -c input/AR-1049-GC.fastq.gz \
  | chopper --minlength 1000 --quality 20 --threads 4 \
  | gzip > filtered_reads/AR-1049-GC_min1000_q20.fastq.gz
```

</details>

Check the output files:

```bash
ls -lh filtered_reads/
```

---

## 6. Compare all conditions with `NanoComp`

Run `NanoComp` on the raw reads and all filtered files you created. Give each
dataset a short name and write the output to `nanocomp/all_conditions/`.

<details markdown="1">
<summary>Solution (all eight conditions)</summary>

```bash
NanoComp \
  --fastq \
    input/AR-1049-GC.fastq.gz \
    filtered_reads/AR-1049-GC_min1000.fastq.gz \
    filtered_reads/AR-1049-GC_min5000.fastq.gz \
    filtered_reads/AR-1049-GC_min10000.fastq.gz \
    filtered_reads/AR-1049-GC_q10.fastq.gz \
    filtered_reads/AR-1049-GC_q15.fastq.gz \
    filtered_reads/AR-1049-GC_q20.fastq.gz \
    filtered_reads/AR-1049-GC_min1000_q15.fastq.gz \
    filtered_reads/AR-1049-GC_min1000_q20.fastq.gz \
  --names raw min1000 min5000 min10000 q10 q15 q20 min1000_q15 min1000_q20 \
  --outdir nanocomp/all_conditions \
  --threads 4
```

</details>

{: .note }
`--names` must list exactly one name per file, in the same order as
`--fastq`. If you ran fewer conditions, remove the matching files and names.

Open the HTML report in `nanocomp/all_conditions/` and `NanoStats.txt`.

### 💬 Discussion

1. Which condition removed the most reads? Which removed the most bases?
2. Why does a length filter remove many reads but few bases?
3. Did Q10 remove anything? Why or why not?
4. Does combining length and quality filters remove more than each filter
   alone?
5. Look at the read length and read quality plots. How does the shape of each
   distribution change between the conditions?

---

## 7. Compare with `fastplong`

Put your `chopper` results next to your `fastplong` results.

1. Do the length filters (min 1,000, 5,000 and 10,000 bp) give similar
   results in both tools? Why would you expect that?
2. `fastplong --mean_qual 30` removed only a few percent of reads. What does
   `chopper --quality 20` remove? Why can't you compare the Q values of the
   two tools directly?
3. `fastplong` trimmed adapters and split chimeric reads by default. What
   happens to those reads in the `chopper` output?
4. Which workflow would you choose for your own data, and why?

{: .note }
`chopper` and `NanoComp` calculate mean read quality from the error
probabilities of all bases. This gives lower values than averaging the Phred
scores directly. The same threshold, for example Q20, is therefore stricter in
`chopper` than in `fastplong`.

---

## 📌 Summary

In this extra practical, you filtered long reads with `chopper` and compared
the results with `NanoComp`.

Different tools can apply the "same" threshold in different ways. Always check
how a tool calculates its metrics before comparing results across tools.
