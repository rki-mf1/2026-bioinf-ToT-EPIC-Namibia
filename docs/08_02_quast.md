---
title: QUAST
parent: Assembly
nav_order: 2
nav_exclude: false
has_children: false
has_toc: true
permalink: /quast/
---

# Assessing genome assemblies with `QUAST`

---

## 🎯 Learning objectives

By the end of this practical, you should be able to:

- run QUAST on a Flye assembly
- locate the main QUAST reports
- interpret basic assembly metrics
- compare assemblies using the same settings
- recognize limitations of reference-free assembly QC

---

## Overview

`QUAST` summarizes genome assemblies using metrics such as assembly length, number of contigs, largest contig, N50, L50, GC content, and ambiguous bases.

This practical continues from the Flye lesson. It expects an assembly at:

```text
analyses/flye/output/<sample>/assembly.fasta
```

{: .important }
> QUAST describes assembly contiguity and composition, but it does not prove that an assembly is complete or biologically correct. A highly contiguous assembly can still contain contamination or misassemblies.

---

## 1. Install QUAST

Move to the workshop directory and create the Conda environment:

```bash
cd ~/2026-bioinf-ToT-EPIC-Namibia

conda create -n quast \
  -c conda-forge \
  -c bioconda \
  quast \
  -y
```

Activate the environment and check the installed version:

```bash
conda activate quast
quast.py --version
```

---

## 2. Select a Flye assembly

The Flye practical used one of the following ONT samples:

```text
AR-1049-GC
AR-1049-MV
AR-1054
AR-1054-GC
```

Select the same sample. This example uses `AR-1049-MV`:

```bash
cd ~/2026-bioinf-ToT-EPIC-Namibia

SAMPLE=AR-1049-MV
ASSEMBLY="analyses/flye/output/${SAMPLE}/assembly.fasta"
OUTDIR="analyses/quast/output/${SAMPLE}"

mkdir -p analyses/quast/output
ls -lh "$ASSEMBLY"
```

Confirm that the file contains FASTA records:

```bash
grep '^>' "$ASSEMBLY"
```

{: .note }
> To analyse another isolate, change only the value assigned to `SAMPLE`.

---

## 3. Run QUAST

Run QUAST with eight CPU threads:

```bash
quast.py "$ASSEMBLY" \
  --labels "${SAMPLE}_Flye" \
  --threads 8 \
  -o "$OUTDIR"
```

Check that the run completed and inspect the output files:

```bash
tail -n 20 "$OUTDIR/quast.log"
ls -lh "$OUTDIR"
```

The main reports are:

| File | Description |
|---|---|
| `report.txt` | Plain-text summary for terminal inspection |
| `report.tsv` | Tab-separated report for scripts or spreadsheets |
| `report.html` | Interactive report with tables and plots |
| `icarus.html` | Interactive contig-size visualization |
| `quast.log` | QUAST command and run log |

Open the text report:

```bash
less "$OUTDIR/report.txt"
```

Press `q` to exit `less`.

{: .task }
> Confirm that `report.txt`, `report.tsv`, and `report.html` were created.

---

## 4. Interpret the report

The most useful reference-free metrics are:

| Metric | Interpretation |
|---|---|
| `# contigs` | Number of contigs included at the active length threshold |
| `Largest contig` | Length of the longest contig |
| `Total length` | Sum of the evaluated contig lengths |
| `N50` | Contig length at which at least 50% of the assembly is contained in contigs of that length or longer |
| `L50` | Minimum number of largest contigs required to reach 50% of the assembly length |
| `GC (%)` | Percentage of bases that are G or C |
| `# N's per 100 kbp` | Number of ambiguous bases per 100,000 assembly bases |

For these *Klebsiella pneumoniae* isolates, compare the total assembly length with the expected genome size of approximately 5.7–5.8 Mb.

Check whether:

- the total length is biologically plausible
- one chromosome-sized contig is present
- smaller contigs may represent plasmids
- the N50 and largest-contig values are consistent with a contiguous assembly
- the assembly contains ambiguous `N` bases

{: .important }
> N50 measures contiguity, not correctness. It does not detect contamination, duplicated sequence, unsupported joins, or all structural errors.

{: .note }
> By default, QUAST calculates its main statistics using contigs of at least 500 bp. Therefore, `# contigs (>= 0 bp)` and `# contigs` may differ. Use the same minimum-contig threshold when comparing assemblies.

{: .task }
> Record the number of contigs, total length, largest contig, N50, L50, GC content, and number of ambiguous bases. Does the assembly appear plausible for a *K. pneumoniae* isolate?

---

## 5. Compare all four assemblies

After all four Flye assemblies have been generated, QUAST can compare them in one report.

Confirm that the assemblies exist:

```bash
for sample in AR-1049-GC AR-1049-MV AR-1054 AR-1054-GC; do
  ls -lh "analyses/flye/output/${sample}/assembly.fasta"
done
```

Run the comparison:

```bash
quast.py \
  analyses/flye/output/AR-1049-GC/assembly.fasta \
  analyses/flye/output/AR-1049-MV/assembly.fasta \
  analyses/flye/output/AR-1054/assembly.fasta \
  analyses/flye/output/AR-1054-GC/assembly.fasta \
  --labels AR-1049-GC,AR-1049-MV,AR-1054,AR-1054-GC \
  --threads 8 \
  -o analyses/quast/output/all_samples
```

Inspect the comparison report:

```bash
less analyses/quast/output/all_samples/report.txt
```

When comparing assemblies, prefer results that combine:

- a plausible total genome size
- few contigs
- a large chromosome-sized contig
- high N50 and low L50
- little or no ambiguous sequence
- plausible GC content

{: .warning }
> Do not select the best assembly using N50 alone. Interpret QUAST together with Flye's `assembly_info.txt`, contig coverage, circularity, the assembly graph, read mapping, and contamination screening.

---

## Summary

| Step | Command or file |
|---|---|
| Flye assembly | `analyses/flye/output/${SAMPLE}/assembly.fasta` |
| Run QUAST | `quast.py "$ASSEMBLY" --threads 8 -o "$OUTDIR"` |
| Text report | `analyses/quast/output/${SAMPLE}/report.txt` |
| Tabular report | `analyses/quast/output/${SAMPLE}/report.tsv` |
| Interactive report | `analyses/quast/output/${SAMPLE}/report.html` |
| Compare all samples | `analyses/quast/output/all_samples/` |

This practical uses reference-free QUAST because the dataset lesson does not download a reference genome. Reference-based metrics should only be added after selecting and documenting a suitable, closely related reference sequence.

---

## Further reading

- [QUAST repository](https://github.com/ablab/quast)
- [QUAST manual](https://quast.sourceforge.net/docs/manual.html)