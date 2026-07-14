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

## 🎯 Learning objectives

By the end of this practical, you should be able to:

- explain why genome assembly quality cannot be described by a single metric
- create and activate a Conda environment containing QUAST
- run QUAST with and without a reference genome
- locate the main QUAST output files
- interpret contig count, total length, N50, L50, GC content, and ambiguous bases
- interpret reference-based metrics such as genome fraction, duplication ratio, misassemblies, mismatches, and indels
- compare multiple assemblies using the same evaluation settings
- recognize limitations of QUAST and identify additional assembly QC steps

## Overview

After generating an assembly with Flye, the next step is to evaluate whether the result is plausible and suitable for downstream analysis.

`QUAST` stands for **QUality ASsessment Tool**. It calculates statistics describing assembly contiguity and composition. When a suitable reference genome is available, QUAST can also align the assembly to the reference and report completeness, structural inconsistencies, unaligned sequence, mismatches, and indels.

QUAST can therefore be used in two main modes:

| Mode | Required input | What it can assess |
|---|---|---|
| Reference-free | Assembly FASTA | Contig count, total length, largest contig, N50, L50, GC content, ambiguous bases, and related size statistics |
| Reference-based | Assembly FASTA and reference FASTA | Reference-free metrics plus genome fraction, duplication, misassemblies, unaligned sequence, mismatches, indels, NGA50, and alignment visualizations |

{: .important }
> QUAST summarizes an assembly, but it does not automatically decide whether the assembly is biologically correct. A high N50 or a single large contig can still occur in a misassembled or contaminated genome.

{: .note }
> This practical continues from the Flye tutorial and expects a Flye assembly at `analyses/flye/output/<sample>/assembly.fasta`.

---

## 1. Create and activate the Conda environment

Move to the main workshop directory.

```bash
cd ~/2026-bioinf-ToT-EPIC-Namibia
```

Create a Conda environment containing QUAST.

```bash
conda create -n quast \
  -c conda-forge \
  -c bioconda \
  quast \
  -y
```

Activate the environment.

```bash
conda activate quast
```

Check that QUAST is available.

```bash
quast.py --version
```

Display the help page.

```bash
quast.py --help | less
```

Press `q` to exit `less`.

{: .question }
> Why should the QUAST version be recorded together with the assembly and the command used for evaluation?

---

## 2. Prepare the working directory

Move to the main workshop directory and create a directory for QUAST results.

```bash
cd ~/2026-bioinf-ToT-EPIC-Namibia
mkdir -p analyses/quast/output
```

Set the sample name used in the Flye practical.

Replace `ONT_SAMPLE` with your actual sample name.

```bash
SAMPLE=ONT_SAMPLE
```

Define the Flye assembly path and the QUAST analysis directory.

```bash
ASSEMBLY=analyses/flye/output/${SAMPLE}/assembly.fasta
QUAST_DIR=analyses/quast
```

Confirm that the assembly exists.

```bash
ls -lh "$ASSEMBLY"
```

Count the FASTA records.

```bash
grep -c '^>' "$ASSEMBLY"
```

Display the FASTA headers.

```bash
grep '^>' "$ASSEMBLY"
```

{: .warning }
> If `ls` reports that the file does not exist, check the value of `SAMPLE` and the Flye output directory before continuing.

{: .task }
> Record the assembly filename, file size, and number of FASTA records before running QUAST.

---

## 3. Understand what QUAST will measure

A genome assembly should be evaluated using several complementary properties.

| Property | Example metrics | Main question |
|---|---|---|
| Contiguity | Number of contigs, largest contig, N50, L50, auN | How fragmented is the assembly? |
| Size | Total assembly length | Is the assembly size plausible for the organism? |
| Composition | GC content, number of `N` bases | Does the sequence composition look plausible? |
| Reference coverage | Genome fraction, unaligned length | How much of a close reference is represented? |
| Redundancy | Duplication ratio | Are reference regions represented more than once? |
| Structural consistency | Misassemblies, local misassemblies | Does the contig structure agree with the reference? |
| Base-level agreement | Mismatches and indels per 100 kbp | How different is the assembly from the reference? |

{: .discussion }
> Why can an assembly with fewer contigs be worse than an assembly with more contigs?

---

## 4. Run QUAST without a reference genome

A reference genome is not required for basic assembly statistics.

Run QUAST on the Flye assembly.

```bash
quast.py "$ASSEMBLY" \
  --threads 8 \
  -o "$QUAST_DIR/output/${SAMPLE}_no_reference"
```

The options used here are:

| Option | Meaning |
|---|---|
| `"$ASSEMBLY"` | Input assembly in FASTA format |
| `--threads 8` | Use up to eight CPU threads |
| `-o` | Write the results to a defined directory |

After the run finishes, inspect the output directory.

```bash
ls -lh "$QUAST_DIR/output/${SAMPLE}_no_reference"
```

Check the end of the QUAST log.

```bash
tail -n 25 "$QUAST_DIR/output/${SAMPLE}_no_reference/quast.log"
```

{: .task }
> Run QUAST without a reference genome and confirm that `report.txt`, `report.tsv`, and `report.html` were created.

---

## 5. Inspect the main output files

QUAST creates several complementary reports.

| File or directory | Description |
|---|---|
| `report.txt` | Plain-text summary suitable for inspection in the terminal |
| `report.tsv` | Tab-separated summary suitable for spreadsheets and scripts |
| `report.html` | Interactive HTML report containing tables and plots |
| `report.pdf` | Static PDF report, when plotting dependencies are available |
| `icarus.html` | Entry page for the Icarus assembly visualizations |
| `icarus_viewers/` | Interactive contig-size and alignment viewers |
| `basic_stats/` | Files and plots used for basic assembly statistics |
| `quast.log` | Detailed log of the QUAST run |

Open the text report.

```bash
less "$QUAST_DIR/output/${SAMPLE}_no_reference/report.txt"
```

Press `q` to exit.

Format the tab-separated report in the terminal.

```bash
column -t -s $'\t' \
  "$QUAST_DIR/output/${SAMPLE}_no_reference/report.tsv" \
  | less -S
```

Use the left and right arrow keys to move horizontally in `less -S`, and press `q` to exit.

{: .note }
> `report.tsv` is usually the most convenient file for automated parsing or import into a spreadsheet. `report.html` is usually the easiest report to explore interactively.

---

## 6. Interpret the reference-free metrics

The most important reference-free metrics include:

| Metric | Interpretation |
|---|---|
| `# contigs` | Number of contigs at or above the active minimum-length threshold |
| `Largest contig` | Length of the longest contig |
| `Total length` | Sum of the evaluated contig lengths |
| `GC (%)` | Percentage of bases that are `G` or `C` |
| `N50` | Contig length at which contigs of that length or longer contain at least 50% of the assembly |
| `L50` | Minimum number of the largest contigs needed to contain at least 50% of the assembly |
| `auN` | Area under the Nx curve; summarizes the complete contig-length distribution |
| `# N's per 100 kbp` | Number of ambiguous `N` bases per 100,000 assembly bases |

### Example of N50 and L50

Consider an assembly containing contigs of:

```text
3,000,000 bp
1,000,000 bp
500,000 bp
300,000 bp
200,000 bp
```

The total assembly length is 5,000,000 bp. Half of the assembly is 2,500,000 bp.

The largest contig alone contains more than 2,500,000 bp. Therefore:

```text
N50 = 3,000,000 bp
L50 = 1
```

{: .important }
> N50 measures contiguity, not correctness. It does not show whether contigs are contaminated, structurally incorrect, duplicated, or derived from the expected organism.

{: .question }
> Two assemblies have the same total length. Assembly A has an N50 of 4 Mb, while Assembly B has an N50 of 1 Mb. What can you conclude, and what can you not conclude, from this information alone?

---

## 7. Understand the minimum contig threshold

By default, most QUAST metrics are calculated using contigs of at least 500 bp.

The report may therefore contain both:

```text
# contigs (>= 0 bp)
# contigs
```

These values can differ when the assembly contains contigs shorter than 500 bp.

Check the first lines of the report.

```bash
head -n 15 "$QUAST_DIR/output/${SAMPLE}_no_reference/report.txt"
```

To evaluate all contigs without the default 500 bp filter, run QUAST with `--min-contig 0`.

```bash
quast.py "$ASSEMBLY" \
  --min-contig 0 \
  --threads 8 \
  -o "$QUAST_DIR/output/${SAMPLE}_min_contig_0"
```

Compare the two reports.

```bash
diff \
  "$QUAST_DIR/output/${SAMPLE}_no_reference/report.txt" \
  "$QUAST_DIR/output/${SAMPLE}_min_contig_0/report.txt" \
  | less
```

{: .warning }
> Do not change `--min-contig` only to make an assembly look better. Use the same threshold when comparing assemblies, and report the threshold used.

{: .task }
> Determine whether your Flye assembly contains contigs shorter than 500 bp. Explain which QUAST rows reveal this.

---

## 8. Evaluate assembly size and composition

For a bacterial isolate, compare the following values with what is biologically expected:

- total assembly length
- number of contigs
- largest contig
- GC content
- number of ambiguous bases
- contig coverage reported by Flye

Possible interpretations include:

| Observation | Possible explanation |
|---|---|
| Total length close to the expected genome size | Assembly size is plausible, but correctness still needs confirmation |
| Total length much larger than expected | Contamination, mixed culture, duplicated sequence, or an incorrect expectation |
| Total length much smaller than expected | Missing genomic regions, low coverage, aggressive filtering, or incomplete assembly |
| Multiple GC-content peaks | Possible contamination or sequences from organisms with different composition |
| Many `N` bases | Scaffold gaps or uncertain sequence; unusual in a typical Flye contig assembly |
| One chromosome-sized contig plus small contigs | Possible chromosome and plasmids, but identities must be verified |

{: .discussion }
> Compare QUAST total length with the sum of contig lengths reported by `seqkit stats` in the Flye practical. Should they always be identical when QUAST uses its default settings?

---

## 9. Open the HTML report

The interactive report is located at:

```text
analyses/quast/output/<sample>_no_reference/report.html
```

When working on a local computer, open the file in a web browser.

When working on a remote server, one option is to start a temporary web server inside the QUAST output directory.

```bash
cd "$QUAST_DIR/output/${SAMPLE}_no_reference"
python -m http.server 8000
```

Leave that terminal running and open the address provided by the instructor or your remote-development environment.

Stop the temporary server with:

```text
Ctrl+C
```

Return to the project directory.

```bash
cd ~/2026-bioinf-ToT-EPIC-Namibia
```

In the HTML report, inspect:

- the main metric table
- the cumulative contig-length plot
- the Nx plot
- the GC-content plot
- the Icarus contig-size viewer

{: .warning }
> Do not expose an unrestricted temporary web server on a public network. Follow the instructor's method for accessing files on the workshop server.

{: .task }
> Open `report.html` and identify which plot best illustrates assembly fragmentation.

---

## 10. Optional: run QUAST with an estimated genome size

When no reference sequence is available but the approximate genome size is known, QUAST can calculate NG statistics using `--est-ref-size`.

For an expected genome size of 5.5 Mb:

```bash
quast.py "$ASSEMBLY" \
  --est-ref-size 5500000 \
  --threads 8 \
  -o "$QUAST_DIR/output/${SAMPLE}_estimated_size"
```

This adds metrics such as `NG50` and `LG50`.

| Metric | Basis of calculation |
|---|---|
| `N50` | Half of the assembly length |
| `NG50` | Half of the estimated genome length |
| `L50` | Number of contigs needed to reach half of the assembly length |
| `LG50` | Number of contigs needed to reach half of the estimated genome length |

{: .important }
> NG statistics are only as reliable as the genome-size estimate. A poor estimate can make the values misleading.

---

## 11. Optional: prepare a reference genome

A reference-based evaluation requires a suitable reference FASTA.

The reference should ideally be:

- from the same species
- complete or of high quality
- sufficiently closely related to the sample
- free from contamination
- appropriate for the biological question

Set the reference path provided by the instructor.

Replace `REFERENCE.fasta` with the real filename.

```bash
REFERENCE=/shared/reference_genomes/REFERENCE.fasta
```

Check the file.

```bash
ls -lh "$REFERENCE"
grep -c '^>' "$REFERENCE"
grep '^>' "$REFERENCE" | head
```

{: .warning }
> A reference from the wrong species, an unusually distant lineage, or a poor-quality reference can produce misleading values for misassemblies, unaligned sequence, mismatches, and indels.

{: .question }
> Why is the same-species reference not necessarily an exact representation of the genome being assembled?

---

## 12. Run QUAST with a reference genome

Run reference-based QUAST.

```bash
quast.py "$ASSEMBLY" \
  -r "$REFERENCE" \
  --labels "${SAMPLE}_Flye" \
  --threads 8 \
  -o "$QUAST_DIR/output/${SAMPLE}_with_reference"
```

The additional options are:

| Option | Meaning |
|---|---|
| `-r "$REFERENCE"` | Reference genome in FASTA format |
| `--labels "${SAMPLE}_Flye"` | Human-readable assembly name used in reports and plots |

Inspect the output.

```bash
less "$QUAST_DIR/output/${SAMPLE}_with_reference/report.txt"
```

List the reference-specific reports.

```bash
find "$QUAST_DIR/output/${SAMPLE}_with_reference/contigs_reports" \
  -maxdepth 2 \
  -type f \
  | sort \
  | less
```

{: .task }
> Run reference-based QUAST using the reference provided by the instructor. Record the genome fraction, duplication ratio, number of misassemblies, unaligned length, mismatches per 100 kbp, and indels per 100 kbp.

---

## 13. Interpret reference-based metrics

Reference-based metrics provide additional evidence, but they must be interpreted in the context of biological divergence between the sample and the reference.

| Metric | Interpretation |
|---|---|
| `Genome fraction (%)` | Percentage of reference bases covered by at least one assembly alignment |
| `Duplication ratio` | Aligned assembly bases divided by aligned reference bases; values above 1 can indicate overlapping or duplicated representation |
| `# misassemblies` | Structural breakpoints where adjacent contig regions align inconsistently to the reference |
| `# misassembled contigs` | Number of contigs containing one or more misassembly events |
| `# local misassemblies` | Smaller local inconsistencies between adjacent alignments |
| `Unaligned length` | Assembly sequence that does not align to the reference |
| `# unaligned contigs` | Fully or partly unaligned contigs |
| `# mismatches per 100 kbp` | Mismatching bases per 100,000 aligned assembly bases |
| `# indels per 100 kbp` | Insertion/deletion events per 100,000 aligned assembly bases |
| `NGA50` | NGA50 calculated after contigs are broken at misassembly events and relative to reference length |

### Important interpretation points

- A high genome fraction suggests that much of the reference is represented in the assembly.
- A genome fraction below 100% can result from incomplete assembly or genuine biological absence relative to the reference.
- A duplication ratio substantially above 1 can indicate duplicated or overlapping assembly regions.
- Misassemblies can represent real assembly errors, but they can also reflect genuine structural differences between the sample and reference.
- Mismatches and indels include both assembly errors and true biological variants.
- Unaligned contigs can represent plasmids, contamination, novel sequence, or low-quality sequence.

{: .important }
> QUAST cannot distinguish every true biological difference from every assembly error. Reference-based results become less reliable as the evolutionary distance between the sample and reference increases.

{: .discussion }
> A small circular contig is completely unaligned to the chromosome reference. List at least three biologically different explanations for this observation.

---

## 14. Inspect misassembled and unaligned contigs

When a reference is supplied, QUAST creates detailed reports inside `contigs_reports/`.

Search for files describing misassembled contigs.

```bash
find "$QUAST_DIR/output/${SAMPLE}_with_reference/contigs_reports" \
  -type f \
  -iname '*mis*' \
  | sort
```

Search for files describing unaligned contigs or fragments.

```bash
find "$QUAST_DIR/output/${SAMPLE}_with_reference/contigs_reports" \
  -type f \
  -iname '*unaligned*' \
  | sort
```

Search the detailed reports for extensive misassemblies.

```bash
grep -R "Extensive misassembly" \
  "$QUAST_DIR/output/${SAMPLE}_with_reference/contigs_reports" \
  | less -S
```

Open the Icarus page.

```text
analyses/quast/output/<sample>_with_reference/icarus.html
```

The reference-based Icarus viewer places contig alignments along the reference and helps visualize:

- inversions
- relocations
- translocations
- unaligned contig regions
- repetitive or ambiguous mappings

{: .task }
> For each reported misassembled or unaligned contig, check its length and coverage in Flye's `assembly_info.txt`. Does the additional information suggest contamination, plasmid sequence, a repeat, or a possible assembly error?

---

## 15. Compare multiple assemblies

QUAST can compare several assemblies in one run, provided the same settings are used for all of them.

For example, compare two Flye runs:

```bash
ASSEMBLY_HQ=analyses/flye/output/${SAMPLE}/assembly.fasta
ASSEMBLY_RAW=analyses/flye/output/${SAMPLE}_nano_raw/assembly.fasta
```

Confirm that both files exist.

```bash
ls -lh "$ASSEMBLY_HQ" "$ASSEMBLY_RAW"
```

Run QUAST without a reference.

```bash
quast.py \
  "$ASSEMBLY_HQ" \
  "$ASSEMBLY_RAW" \
  --labels "Flye_nano_hq,Flye_nano_raw" \
  --threads 8 \
  -o "$QUAST_DIR/output/${SAMPLE}_comparison"
```

With a reference genome:

```bash
quast.py \
  "$ASSEMBLY_HQ" \
  "$ASSEMBLY_RAW" \
  -r "$REFERENCE" \
  --labels "Flye_nano_hq,Flye_nano_raw" \
  --threads 8 \
  -o "$QUAST_DIR/output/${SAMPLE}_comparison_with_reference"
```

When comparing assemblies, consider all of the following:

| Prefer lower values | Prefer higher values | Values expected to be plausible |
|---|---|---|
| Number of contigs | Largest contig | Total length |
| L50 | N50 and auN | GC content |
| Misassemblies | Genome fraction | Duplication ratio close to 1 |
| Unaligned length | NGA50 | Mismatch and indel rates consistent with expected divergence and read accuracy |
| Ambiguous bases |  | Number and sizes of plasmid-like contigs |

{: .warning }
> Do not select an assembly only because it has the highest N50. A more contiguous assembly may contain structural errors or contamination.

{: .question }
> Which metrics would you prioritize when choosing between one highly contiguous assembly with several misassemblies and one slightly more fragmented assembly with no detected misassemblies?

---

## 16. Combine QUAST with other assembly evidence

QUAST should be interpreted together with other results.

| Evidence source | What it contributes |
|---|---|
| Flye `assembly_info.txt` | Contig length, estimated coverage, repeat status, multiplicity, and circularity |
| Flye assembly graph | Branches, unresolved repeats, alternative paths, and graph structure |
| Read mapping | Coverage uniformity, unsupported joins, low-coverage regions, and read support |
| Taxonomic classification | Evidence for contamination or mixed organisms |
| BUSCO or lineage-specific completeness tools | Conserved-gene completeness and duplication |
| Genome annotation | Gene count, disrupted genes, and biological plausibility |
| Plasmid analysis | Evidence that smaller contigs are plasmids rather than contamination |

{: .important }
> QUAST does not verify Flye's circularity calls and does not replace inspection of the assembly graph or read support across contig joins.

---

## 17. Common problems

### `quast.py: command not found`

Activate the correct Conda environment.

```bash
conda activate quast
which quast.py
quast.py --version
```

### The assembly file is missing

Check the variables and directory structure.

```bash
echo "$SAMPLE"
echo "$ASSEMBLY"
ls -lh analyses/flye/output/
```

### QUAST reports zero contigs

Check whether the assembly contains valid FASTA records and whether contigs are shorter than the active minimum threshold.

```bash
grep -c '^>' "$ASSEMBLY"
head "$ASSEMBLY"
```

Re-run with a lower threshold only when biologically justified.

```bash
quast.py "$ASSEMBLY" \
  --min-contig 0 \
  --threads 8 \
  -o "$QUAST_DIR/output/${SAMPLE}_all_contigs"
```

### The output directory already exists

Use a new output directory or remove the old directory only after confirming that it is no longer needed.

```bash
quast.py "$ASSEMBLY" \
  --threads 8 \
  -o "$QUAST_DIR/output/${SAMPLE}_run2"
```

### Reference-based QUAST is slow or uses too much memory

Reduce the thread count first.

```bash
quast.py "$ASSEMBLY" \
  -r "$REFERENCE" \
  --threads 2 \
  -o "$QUAST_DIR/output/${SAMPLE}_with_reference_t2"
```

For very large genomes, QUAST also provides large-genome and memory-efficient modes, but these are generally unnecessary for bacterial isolate assemblies.

### The HTML report does not open from the remote server

Download the QUAST output directory or follow the instructor's remote-port or file-browser procedure. The command-line `report.txt` and `report.tsv` contain the core metrics even when the HTML report cannot be displayed.

### The reference-based report contains many mismatches or misassemblies

Check:

- whether the correct reference was selected
- whether the reference is from the same species
- whether the sample and reference are expected to be closely related
- whether unaligned contigs may be plasmids or contaminants
- whether Flye coverage and graph structure support the contigs
- whether the differences could be genuine biological variation

---

## 18. Practical questions

{: .question }
> 1. Which assembly file did you evaluate?
> 2. Which QUAST version did you use?
> 3. What minimum contig length was used?
> 4. How many contigs were reported?
> 5. What was the total assembly length?
> 6. What was the largest contig length?
> 7. What were the N50 and L50 values?
> 8. What was the GC content?
> 9. Were ambiguous `N` bases present?
> 10. Was the assembly size plausible for the expected organism?
> 11. If a reference was used, what was the genome fraction?
> 12. What was the duplication ratio?
> 13. Were misassemblies reported?
> 14. Were any contigs fully or partially unaligned?
> 15. Could observed mismatches and indels represent genuine biological differences?
> 16. Which additional QC analyses are required before accepting the assembly?

---

## 📌  Summary

In this practical, you used QUAST to summarize and evaluate a Flye assembly.

| Step | Main command or file |
|---|---|
| Create environment | `conda create -n quast -c conda-forge -c bioconda quast -y` |
| Activate environment | `conda activate quast` |
| Flye assembly | `analyses/flye/output/${SAMPLE}/assembly.fasta` |
| Reference-free QUAST | `quast.py "$ASSEMBLY" --threads 8 -o <directory>` |
| Reference-based QUAST | Add `-r "$REFERENCE"` |
| Text report | `report.txt` |
| Spreadsheet-friendly report | `report.tsv` |
| Interactive report | `report.html` |
| Icarus viewer | `icarus.html` |
| Detailed alignment reports | `contigs_reports/` |

A reliable assembly assessment combines QUAST metrics with Flye coverage and circularity information, assembly-graph inspection, read mapping, contamination screening, and biological expectations.

---

## 📚 Further reading

- [QUAST repository and overview](https://github.com/ablab/quast)
- [QUAST manual](https://quast.sourceforge.net/docs/manual.html)
