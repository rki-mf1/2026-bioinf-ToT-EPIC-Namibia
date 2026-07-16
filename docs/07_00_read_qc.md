---
title: QC and Preprocessing of Raw Sequencing Data
nav_order: 7
nav_exclude: false
has_children: true
has_toc: false
permalink: /read_qc/
---

# Quality Control (QC) and Preprocessing of Raw Sequencing Data

---

Quality control should be performed before downstream analysis to assess whether raw sequencing reads are suitable for use. It can reveal low-quality bases, adapter contamination, unusual GC content, duplicated reads, and other technical issues that may affect assembly, mapping, or variant calling.

For Illumina data, tools such as **fastp** can generate quality reports while also trimming adapters and filtering low-quality reads. The next tutorial demonstrates how to inspect and clean the workshop dataset with fastp.