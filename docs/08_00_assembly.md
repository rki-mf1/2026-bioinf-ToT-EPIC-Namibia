---
title: Assembly
nav_order: 8
nav_exclude: false
has_children: true
has_toc: false
permalink: /assembly/
---

# Genome assembly

---

Genome assembly reconstructs longer DNA sequences, called **contigs**, from sequencing reads. Assembly quality depends on read length, read accuracy, sequencing depth, contamination, repetitive regions, and genome complexity.

---

## De novo assembly

In **de novo assembly**, reads are assembled without using an existing reference genome to determine their order. This approach can recover novel regions, plasmids, insertions, deletions, and structural differences that may be absent from a reference.

De novo assembly is therefore useful when studying a new organism or when the complete genome structure is important. However, repeated regions can cause fragmented or incorrect assemblies, especially when only short reads are available.

Common tools include:

- **SPAdes** or **Shovill** for Illumina short reads
- **Flye**, **Canu**, or **Raven** for Oxford Nanopore and PacBio long reads

---

## Reference-based analysis

In a **reference-based approach**, reads are aligned to an existing genome and a **consensus sequence** or set of variants is produced. This is often used to compare closely related bacterial isolates or identify SNPs during outbreak investigations.

Reference-based analysis is usually faster and produces positions that can be compared directly between samples. However, it is affected by **reference bias**: novel regions, large rearrangements, plasmids, and sequences that differ strongly from the reference may be missed or represented poorly.

{: .note }
Reference-based reconstruction is not a true de novo assembly because the reference genome guides the result. Common workflows use **BWA** or **minimap2** for read mapping and tools such as **SAMtools**, **BCFtools**, or **Snippy** for consensus generation and variant calling.

---

## Hybrid assembly

A **hybrid assembly** combines long and short reads from the same isolate. Long reads help resolve repeats and connect genomic regions, while accurate short reads can improve the final consensus sequence.

Two main strategies are commonly used:

### Long-read-first assembly

Long reads are assembled first to reconstruct the chromosome and plasmids. The assembly is then **polished** with long reads and, when available, short reads.

Typical tools include:

- **Flye** for the initial long-read assembly
- **Medaka** or **Racon** for long-read polishing
- **Polypolish**, **Pilon**, or **Pypolca** for short-read polishing
- **Hybracter** for an automated bacterial long-read-first workflow

With sufficient long-read depth and accuracy, this is often the preferred strategy for bacterial isolate genomes.

### Short-read-first assembly

Short reads are assembled first, and long reads are then used to connect contigs and resolve paths through the assembly graph.

Typical tools include:

- **Unicycler**
- **hybridSPAdes**

This approach can be useful when long-read coverage is limited, but its success depends strongly on the quality of the initial short-read assembly graph.

---

## 📋 Choosing an approach

| Available data | Common approach | Example tools |
|---|---|---|
| Illumina only | Short-read de novo assembly | SPAdes, Shovill |
| ONT or PacBio only | Long-read de novo assembly | Flye, Canu, Raven |
| Long and short reads | Long-read-first hybrid assembly | Hybracter, Flye with polishing |
| Good short reads but limited long-read depth | Short-read-first hybrid assembly | Unicycler, hybridSPAdes |
| Closely related isolates and a suitable reference | Reference mapping and variant calling | Snippy, BWA, minimap2 |

{: .important }
Short and long reads used for a hybrid assembly must come from the same isolate. Mixing reads from different isolates can create misleading or incorrect assemblies.

The next tutorial demonstrates **de novo long-read assembly with Flye**.