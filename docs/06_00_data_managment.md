---
title: Data Managment
nav_order: 6
nav_exclude: false
has_children: true
has_toc: false
permalink: /data_managment/
---

# 🗂️ Data Management

Good project organisation makes bioinformatics analyses easier to understand, reproduce, share, and maintain. A clear structure is especially important when a project contains many samples, large sequencing files, several tools, and multiple analysis versions.

---

## 🎯 Learning objectives

By the end of this lesson, you should be able to:

- organise a bioinformatics project using a consistent directory structure
- use clear and machine-readable file names
- separate raw data, scripts, intermediate files, and final results
- record software environments, versions, commands, and parameters
- reduce unnecessary storage use without losing important information

---

## 📁 Organise each project consistently

Create one main directory for each project and use the same structure whenever possible. 

Here is an idea for one way to organize it:

```text
project_name/
├── README.md
├── data/
│   ├── raw/
│   └── metadata/
├── envs/
├── scripts/
├── results/
│   ├── qc/
│   ├── assembly/
│   └── typing/
├── logs/
└── reports/
```

| Directory        | Contents                                       |
|------------------|------------------------------------------------|
| `data/raw/`      | Original sequencing files; do not modify them  |
| `data/metadata/` | Sample information and data dictionaries       |
| `envs/`          | Conda environment files and software records   |
| `scripts/`       | Reusable commands, scripts, and workflow files |
| `results/`       | Analysis outputs organised by analysis step    |
| `logs/`          | Program output, error messages, and run logs   |
| `reports/`       | Figures, tables, summaries, and final reports  |

{: .important }
> Keep the original raw data **unchanged**. Perform analyses on linked or clearly identified working files rather than editing the original FASTQ files.

Use a short `README.md` to document the project purpose, dataset, directory structure, analysis steps, and responsible people.

---

## 🏷️ Name files clearly

File names should be informative, consistent, and easy to process with command-line tools.

Recommended practices:

- use stable and **unique** sample identifiers
- use underscores `_` or hyphens `-` instead of spaces ` `
- avoid brackets, special characters, and very long names
- use the ISO date format `YYYY-MM-DD` when dates are needed
- include the analysis step or file content
- keep standard file extensions

✔️ Good examples: 

```text
NAM_001_R1.fastq.gz
NAM_001_R2.fastq.gz
NAM_001_flye_assembly.fasta
NAM_001_quast_report.tsv
sample_metadata_2026-07-16.tsv
```

❌ Avoid names such as:

```text
sample 1 final FINAL.fastq.gz
new_results(2).txt
assembly_latest.fasta
```

{: .tip }
> Decide on a naming convention **before** starting the analysis and use the same sample identifiers in sequencing files, metadata, results, and reports.

---

## 🧾 Record software and analysis steps

Bioinformatics results depend on software versions, databases, parameters, and input files. Record enough information to repeat the analysis later.

For each analysis, keep:

- the complete command or script
- software and database versions
- parameters that differ from the defaults
- input and output file names
- the date of the analysis
- relevant log files

Use a separate Conda environment for each workflow or group of compatible tools. Avoid installing analysis software into the Conda `base` environment.

Export a reproducible environment description:

```bash
conda env export --from-history > envs/environment.yml
```

Record tool versions when running an analysis:

```bash
flye --version > logs/software_versions.txt
quast.py --version >> logs/software_versions.txt
```

{: .note }
> Git is useful for scripts, Markdown files, environment definitions, and small configuration files. Large FASTQ, BAM, and assembly-result directories should normally not be committed to Git.

---

## 💾 Save storage space carefully

Sequencing projects can quickly consume large amounts of disk space. Check storage use regularly:

```bash
du -sh .
du -sh data/* results/*
```

To reduce unnecessary storage use:

- keep compressed FASTQ files as `.fastq.gz`
- use BAM or CRAM instead of uncompressed SAM files
- create **symbolic links** instead of copying large shared datasets
- avoid keeping several identical copies of the same file
- remove temporary files only after confirming that the analysis completed successfully
- keep final results, scripts, environment files, logs, and essential quality reports

Create a symbolic link to a shared file:

```bash
ln -s /shared/data/NAM_001.fastq.gz data/raw/NAM_001.fastq.gz
```

{: .warning }
> Do not delete intermediate files until the final outputs have been checked and the analysis can be reproduced from the retained raw data, scripts, parameters, and software environment.

---

## 🔐 Protect and verify your data

Important data should exist in more than one location. Maintain an appropriate backup of raw data, metadata, scripts, and final results.

Checksums can be used to confirm that files were copied or transferred correctly:

```bash
sha256sum data/raw/NAM_001.fastq.gz > data/raw/NAM_001.fastq.gz.sha256
sha256sum -c data/raw/NAM_001.fastq.gz.sha256
```

Sensitive sample or patient information must be stored and shared according to the applicable data-protection and institutional requirements.

---

## ✅ Project checklist

Before considering an analysis complete, confirm that:

- raw data are unchanged and backed up
- sample identifiers are consistent
- metadata are complete and understandable
- commands and parameters are saved
- software and database versions are recorded
- final results are separated from temporary files
- storage-heavy duplicates are removed safely
- another person could understand and repeat the analysis

{: .discussion }
> Which files in your current projects would be difficult for another person to interpret, and what information would make them easier to reuse?

---