---
title: Fastp
parent: QC and Preprocessing of Raw Sequencing Data
nav_order: 1
nav_exclude: false
permalink: /fastp/
---

# Quality control of Illumina paired-end reads with `fastp`

---

## 🎯 Learning objectives

By the end of this practical, you should be able to:

- understand why read quality control is important before downstream analysis
- run `fastp` on Illumina paired-end FASTQ files
- create filtered paired FASTQ output files
- generate and open an HTML quality-control report
- understand adapter trimming and quality filtering for short-read data
- filter reads by minimum length after trimming
- compare quality-control results generated with different parameters

---

## Overview

`fastp` is a fast all-in-one preprocessing tool for FASTQ files. It is commonly used for Illumina short-read data and supports both single-end and paired-end reads.

For Illumina paired-end data, `fastp` can perform:

- quality control before and after filtering
- adapter trimming
- low-quality base filtering
- read-length filtering
- optional polyG trimming for NextSeq/NovaSeq-style data
- HTML and JSON report generation

In this practical, we will use `fastp` to inspect and filter paired-end Illumina sequencing reads. We will first run `fastp` with default parameters, then repeat the analysis with length-based and quality-based filtering.

`fastp` automatically creates HTML and JSON reports. The HTML report can be opened in a web browser and is useful for checking read counts, read quality, GC content, duplication, adapter content, insert-size information, and filtering results.

---

## 1. Activate the Conda environment

First, create and activate the Conda environment that contains `fastp`.

```bash
conda create -n fastp -c conda-forge -c bioconda fastp -y
conda activate fastp
```

Check that `fastp` is available.

```bash
fastp --version
```

If the command prints a version number, the environment is ready.

---

## 2. Prepare the working directory

Move to the main workshop repository.

```bash
cd ~/2026-bioinf-ToT-EPIC-Namibia
```

Create a working directory for this practical.

```bash
mkdir -p analyses/fastp
cd analyses/fastp
```

Create directories for input reads and filtered output files.

```bash
mkdir -p input
mkdir -p filtered_reads/default_params
mkdir -p filtered_reads/by_length
mkdir -p filtered_reads/by_quality
```

Create a symlink the paired-end FASTQ files into the `input` directory.

```bash
ln -s /shared/fastq_files input/
```

Check that both files were copied successfully.

```bash
ls -lh input/
```

You should see two files, one for read 1 and one for read 2.

Example paired-end naming patterns include:

```bash
ERR16718582_1.fastq.gz
ERR16718582_2.fastq.gz
```

or:

```bash
ERR16718582_R1.fastq.gz
ERR16718582_R2.fastq.gz
```

> If your FASTQ files have different names, adjust the commands below accordingly.

---

## 3. Basic `fastp` syntax for paired-end data

The basic structure of a paired-end `fastp` command is:

```bash
fastp \
  --in1 sample_R1.fastq.gz \
  --in2 sample_R2.fastq.gz \
  --out1 sample_R1.fastp.fastq.gz \
  --out2 sample_R2.fastp.fastq.gz \
  --html sample_fastp.html \
  --json sample_fastp.json
```

You can also use the short option names:

```bash
fastp \
  -i sample_R1.fastq.gz \
  -I sample_R2.fastq.gz \
  -o sample_R1.fastp.fastq.gz \
  -O sample_R2.fastp.fastq.gz \
  -h sample_fastp.html \
  -j sample_fastp.json
```

Important options used in this practical:

| Option | Short option | Meaning |
|---|---:|---|
| `--in1` | `-i` | Input FASTQ file for read 1 |
| `--in2` | `-I` | Input FASTQ file for read 2 |
| `--out1` | `-o` | Output FASTQ file for filtered read 1 |
| `--out2` | `-O` | Output FASTQ file for filtered read 2 |
| `--html` | `-h` | Output HTML report |
| `--json` | `-j` | Output JSON report |
| `--verbose` | `-V` | Print more detailed information to the terminal |
| `--dont_overwrite` |  | Prevent existing output files from being overwritten |
| `--length_required` | `-l` | Discard reads shorter than this length after trimming |
| `--qualified_quality_phred` | `-q` | Phred score at which a base is considered qualified |
| `--unqualified_percent_limit` | `-u` | Maximum percentage of unqualified bases allowed in a read |
| `--n_base_limit` | `-n` | Discard reads with more than this number of `N` bases |
| `--average_qual` | `-e` | Discard reads whose average quality is below this value |

For paired-end reads, always provide both input files and both output files:

```bash
--in1 / --out1 for read 1
--in2 / --out2 for read 2
```

---

## 4. Run `fastp` with default parameters

First, run `fastp` using its default settings.

```bash
fastp \
  --verbose \
  --dont_overwrite \
  --in1 input/ERR16718582_1.fastq.gz \
  --in2 input/ERR16718582_2.fastq.gz \
  --out1 filtered_reads/default_params/ERR16718582_R1_fastp.fastq.gz \
  --out2 filtered_reads/default_params/ERR16718582_R2_fastp.fastq.gz \
  --html filtered_reads/default_params/ERR16718582_fastp.html \
  --json filtered_reads/default_params/ERR16718582_fastp.json
```

This command creates:

- a filtered FASTQ file for read 1
- a filtered FASTQ file for read 2
- an HTML report
- a JSON report

Check the output files.

```bash
ls -lh filtered_reads/default_params/
```

---

## 5. Open the HTML report

Now you can download the `.html` file and open it on your presonal computer.

If server has `firefox` setup the HTML report can be opened with a web browser.

```bash
firefox filtered_reads/default_params/ERR16718582_fastp.html
```

Look at the report and try to answer:

1. How many read pairs were present before filtering?
2. How many read pairs passed filtering?
3. What are the read lengths for read 1 and read 2?
4. Is there evidence of low-quality bases toward the 3′ ends of reads?
5. Is adapter content detected?
6. How do the read 1 and read 2 quality profiles compare?
7. Did `fastp` report an insert-size estimate?

---

## 6. Filter reads by length

Length filtering removes reads shorter than a selected minimum length after trimming. For Illumina short-read data, the threshold should usually be much shorter than the original read length.

For example, if your reads are 150 bp, a minimum length such as 50 bp is commonly used to remove very short reads while retaining useful data.

The option for minimum length filtering is:

```bash
--length_required <length>
```

The short option is:

```bash
-l <length>
```

Check the `fastp` help page to find the default value.

```bash
fastp --help | grep length_required
```

Now discard reads shorter than 50 bp after trimming.

```bash
fastp \
  --verbose \
  --dont_overwrite \
  --length_required 50 \
  --in1 input/ERR16718582_1.fastq.gz \
  --in2 input/ERR16718582_2.fastq.gz \
  --out1 filtered_reads/by_length/ERR16718582_R1_min50.fastq.gz \
  --out2 filtered_reads/by_length/ERR16718582_R2_min50.fastq.gz \
  --html filtered_reads/by_length/ERR16718582_min50_fastp.html \
  --json filtered_reads/by_length/ERR16718582_min50_fastp.json
```

Check the output files.

```bash
ls -lh filtered_reads/by_length/
```

Open the report.

```bash
firefox filtered_reads/by_length/ERR16718582_min50_fastp.html
```

{: .discussion}
> Compare the reports from the default run and the length-filtered run.
> 
> Try to answer:
> 
> 1. How many read pairs were removed by the 50 bp minimum-length filter?
> 2. Did the read length distribution change?
> 3. Did length filtering change the total number of bases retained?
> 4. Did the read 1 and read 2 outputs remain paired?

---

## 7. Filter reads by quality

For Illumina data, quality often decreases toward the 3′ ends of reads. `fastp` provides several ways to filter reads by quality.

Important quality-filtering options in `fastp`:

| Option | Short option | Meaning |
|---|---:|---|
| `--qualified_quality_phred` | `-q` | Phred score at which a base is considered qualified |
| `--unqualified_percent_limit` | `-u` | Maximum percentage of unqualified bases allowed in a read |
| `--average_qual` | `-e` | Minimum average quality score required for a read to pass |
| `--n_base_limit` | `-n` | Maximum number of unknown bases (`N`) allowed |

By default, `fastp` uses quality filtering. In this section, we will make the filter more explicit by requiring bases below Q20 to be treated as unqualified and allowing at most 30% unqualified bases per read.

```bash
fastp \
  --verbose \
  --dont_overwrite \
  --qualified_quality_phred 20 \
  --unqualified_percent_limit 30 \
  --in1 input/ERR16718582_1.fastq.gz \
  --in2 input/ERR16718582_2.fastq.gz \
  --out1 filtered_reads/by_quality/ERR16718582_R1_q20_u30.fastq.gz \
  --out2 filtered_reads/by_quality/ERR16718582_R2_q20_u30.fastq.gz \
  --html filtered_reads/by_quality/ERR16718582_q20_u30_fastp.html \
  --json filtered_reads/by_quality/ERR16718582_q20_u30_fastp.json
```

Check the output files.

```bash
ls -lh filtered_reads/by_quality/
```

Open the report.

```bash
firefox filtered_reads/by_quality/ERR16718582_q20_u30_fastp.html
```

### Optional: require a minimum average read quality

You can also require a minimum average quality score across each read using `--average_qual`.

For example, require an average read quality of at least Q20:

```bash
fastp \
  --verbose \
  --dont_overwrite \
  --average_qual 20 \
  --in1 input/ERR16718582_1.fastq.gz \
  --in2 input/ERR16718582_2.fastq.gz \
  --out1 filtered_reads/by_quality/ERR16718582_R1_avgq20.fastq.gz \
  --out2 filtered_reads/by_quality/ERR16718582_R2_avgq20.fastq.gz \
  --html filtered_reads/by_quality/ERR16718582_avgq20_fastp.html \
  --json filtered_reads/by_quality/ERR16718582_avgq20_fastp.json
```

---

## 8. Try different quality thresholds

Now try a stricter setting. For example, require bases below Q30 to be treated as unqualified and allow at most 20% unqualified bases per read.

```bash
fastp \
  --verbose \
  --dont_overwrite \
  --qualified_quality_phred 30 \
  --unqualified_percent_limit 20 \
  --in1 input/ERR16718582_1.fastq.gz \
  --in2 input/ERR16718582_2.fastq.gz \
  --out1 filtered_reads/by_quality/ERR16718582_R1_q30_u20.fastq.gz \
  --out2 filtered_reads/by_quality/ERR16718582_R2_q30_u20.fastq.gz \
  --html filtered_reads/by_quality/ERR16718582_q30_u20_fastp.html \
  --json filtered_reads/by_quality/ERR16718582_q30_u20_fastp.json
```

You can also combine quality and length filtering.

```bash
fastp \
  --verbose \
  --dont_overwrite \
  --qualified_quality_phred 20 \
  --unqualified_percent_limit 30 \
  --length_required 50 \
  --in1 input/ERR16718582_1.fastq.gz \
  --in2 input/ERR16718582_2.fastq.gz \
  --out1 filtered_reads/by_quality/ERR16718582_R1_q20_u30_min50.fastq.gz \
  --out2 filtered_reads/by_quality/ERR16718582_R2_q20_u30_min50.fastq.gz \
  --html filtered_reads/by_quality/ERR16718582_q20_u30_min50_fastp.html \
  --json filtered_reads/by_quality/ERR16718582_q20_u30_min50_fastp.json
```

{: .discussion}

Compare the default, Q20/U30, Q30/U20, and combined Q20/U30/min50 reports.

Try to answer:

1. What happens to the passed-filter percentage as the quality threshold becomes stricter?
2. Which setting removes the most read pairs?
3. Which setting seems reasonable for preserving enough data while removing low-quality reads?
4. Does read 2 have lower quality than read 1?
5. How does filtering affect GC content, duplication, and adapter content?
6. How does filtering affect the total number of bases retained?

---

## 9. Check all generated reports

List all HTML reports created during this practical.

```bash
find filtered_reads -name "*.html"
```

You can open any report with:

```bash
firefox path/to/report.html
```

For example:

```bash
firefox filtered_reads/by_quality/ERR16718582_q30_u20_fastp.html
```

---

## 📌 Summary

In this practical, you used `fastp` to perform quality control and filtering of Illumina paired-end FASTQ files.

| Analysis | Main option used | Output directory |
|---|---|---|
| Default QC | Default parameters | `filtered_reads/default_params/` |
| Length filtering | `--length_required 50` | `filtered_reads/by_length/` |
| Quality filtering | `--qualified_quality_phred 20 --unqualified_percent_limit 30` | `filtered_reads/by_quality/` |
| Average-quality filtering | `--average_qual 20` | `filtered_reads/by_quality/` |
| Combined quality and length filtering | `--qualified_quality_phred 20 --unqualified_percent_limit 30 --length_required 50` | `filtered_reads/by_quality/` |

Key corrections from `fastplong`-style syntax to Illumina paired-end `fastp` syntax:

| Incorrect or inappropriate here | Correct for paired-end `fastp` |
|---|---|
| `--in` | `--in1` and `--in2` |
| `--out` | `--out1` and `--out2` |
| one output FASTQ | two paired output FASTQ files |
| `--mean_qual` | `--average_qual` if filtering by average read quality |
| ONT-style long-read discussion | Illumina short-read adapter, per-base quality, and paired-end QC discussion |
| `--length_required 500` for short reads | a short-read threshold such as `--length_required 50` |
