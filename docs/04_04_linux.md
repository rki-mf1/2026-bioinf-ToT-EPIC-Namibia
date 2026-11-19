---
title: Viewing, Editing, Compressing, and Searching Files
parent: Linux
nav_order: 4
nav_exclude: false
permalink: /linux_files/
---

# Viewing, Editing, Compressing, and Searching Files

---

## 🎯 Learning objectives

By the end of this lesson, you should be able to:

- search text with `grep`
- combine commands using pipes (`|`)
- extract columns from tabular files

--- 

## Before you start

Open a terminal and move into the workshop repository:

```bash
cd ~/2026-bioinf-ToT-EPIC-Namibia
```

Create a safe practice area for this session:

```bash
mkdir -p scratch
```

---

## 2. Compression with `gzip`

Genomics files are often very large, frequently reaching several gigabytes. To reduce storage requirements, they are commonly compressed with `gzip`. Although these files contain binary data and cannot be read directly with standard text tools, you can still inspect them from the command line without decompressing them first.

First, let’s try seeing `.gz` file using `head` command.

```bash
head data/tutorial_data/ERR16718636.fna.gz
```

🎉 Congratulations! Instead of a readable FASTA file, you have discovered mysterious alien 👽 symbols. What actually happened is that head is showing you the raw compressed binary data of the `.gz` file — which looks like nonsense. To properly look inside compressed FASTA files, you need to use tools that understand `gzip` compression, such as:

```bash
zcat data/tutorial_data/ERR16718636.fna.gz | head
```

Let's copy it in scratch directory, and then uncompress it:

```bash
cp data/tutorial_data/ERR16718636.fna.gz scratch/ 
cd scratch

# Uncompress but keep compressed file to compare sizes
gunzip -k ERR16718636.fna.gz
ls -lh

# Remove gz file
rm ERR16718636.fna.gz
ls -lh 
```

To compress file:

```bash
gzip ERR16718636.fna
ls -lh
```

{: .discussion}
> - What changed in the file size?
> - Why is compression especially common for FASTA and FASTQ files?
> - What is the difference between `gzip` and `gunzip`?

---

## 3. Inspect compressed files without unpacking them

Try and compare these commands:

```bash
cat ERR16718636.fna.gz
```

```bash
zcat ERR16718636.fna.gz
```

```bash
zcat ERR16718636.fna.gz | head
```

```bash
zcat ERR16718636.fna.gz | tail
```

```bash
zless ERR16718636.fna.gz
```

{: .discussion}
> - Why is `cat` not useful on a `.gz` file?
> - When is `zless` better than unzipping a large file first?

---

## 4. Search text with `grep`

Let's copy `amrfinderplus.tsv` in our current working directory to explore it:

```bash
cd ~/2026-bioinf-ToT-EPIC-Namibia
cp data/tutorial_data/amrfinderplus.tsv scratch/
cd scratch
```

Search for one word:

```bash
grep "OXA" amrfinderplus.tsv
```

Count matching lines:

```bash
grep -c "OXA" amrfinderplus.tsv
```

Search case-insensitively:

```bash
grep "oXa" amrfinderplus.tsv
grep -i "OxA" amrfinderplus.tsv
```

Search for lines that start with a pattern:

```bash
zgrep "^>" ERR16718636.fna.gz
```

{: .discussion}
> - What does `-c` do?
> - What does `-i` do?
> - What does `^>` mean?

---

## 5. Pipes

A pipe sends the output of one command into the next command.

Examples:

```bash
cat amrfinderplus.tsv | wc -l
```

```bash
grep "OXA" amrfinderplus.tsv | wc -l
```

```bash
grep "OXA" amrfinderplus.tsv | less
```

```bash
grep "OXA" amrfinderplus.tsv | head
```

{: .discussion}
> - Why is `|` useful?
> - Which part runs first in `grep "OXA" amrfinderplus.tsv | less`?

---

## 6. Work with tabular data from the command line


### Extract columns with `cut`

Get the `Contig id` and `Element symbol` columns from `amrfinderplus.tsv`:

```bash
cut -f 3,7 amrfinderplus.tsv
```

Get only `Element symbol` containing `OXA`:

```bash
grep "OXA" amrfinderplus.tsv | cut -f 7
```

### Sort values with `sort`

```bash
grep "OXA" amrfinderplus.tsv | cut -f 7 | sort
```

### Find unique values with `uniq`

```bash
grep "OXA" amrfinderplus.tsv | cut -f 7 | sort | uniq
```

Count unique values:

```bash
grep "OXA" amrfinderplus.tsv | cut -f 7 | sort | uniq -c
```

Save unique OXA to a new file:

```bash
grep "OXA" amrfinderplus.tsv | cut -f 7 | sort | uniq -c > unique_OXA.txt
```

### Count lines with `wc`

```bash
wc amrfinderplus.tsv
wc -l amrfinderplus.tsv
wc -w amrfinderplus.tsv
```

---

## 📌 Summary

| Command | Purpose |
| --- | --- |
| `gzip` | Compress a file |
| `gunzip` | Uncompress a `.gz` file |
| `zcat` | Print contents of a `.gz` file |
| `zless` | View a `.gz` file page by page |
| `grep` | Search text |
| `grep -c` | Count matching lines |
| `cut` | Extract selected columns |
| `sort` | Sort lines |
| `uniq` | Remove consecutive duplicates |
| `wc` | Count lines, words, or characters |
