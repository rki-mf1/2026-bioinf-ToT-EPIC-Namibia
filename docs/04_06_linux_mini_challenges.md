---
title: Linux - Mini Challenges
parent: Linux
nav_order: 6
nav_exclude: false
permalink: /linux_mini_challenges/
---

# Mini Challenges: Linux

These three challenges combine the commands from the Linux lessons:

- [First Commands and Getting Help](04_01_linux.md)
- [Navigation](04_02_linux.md)
- [File and Folder Management](04_03_linux.md)
- [Viewing, Compressing, and Searching Files](04_04_linux.md)

The challenges build on each other, so do them in order. Try to solve each step without looking at the solution first.

{: .note}
> All challenges assume the workshop folder is at `~/2026-bioinf-ToT-EPIC-Namibia`. Work in the `scratch` directory, so you never change the original files in `data`.

---

## 1. Mini challenge: Set up your project

You are starting the analysis of a *Klebsiella pneumoniae* sequencing run. Before you touch any data, set up a clean project folder.

1. open a new terminal and check where you are. What does `~` in your prompt stand for?
2. move into the `docs` directory of the workshop folder using an **absolute** path
3. from `docs`, move into `scratch` using a **relative** path (create `scratch` first if it does not exist)
4. build the following structure inside `scratch`:

    ```
    kp_project/
    ├── README.md
    ├── data/
    │   └── metadata/
    ├── results/
    └── logs/
    ```

5. write the line `Klebsiella pneumoniae project, Windhoek 2026` into `README.md`
6. add a second line `Author: <your name>` to `README.md` without overwriting the first line
7. open `README.md` in `nano`, add a third line `Status: setup done`, save and exit
8. show only the **last two** lines of `README.md`
9. go to your home directory, then jump straight back to `kp_project` with one short command
10. list everything in `kp_project` with details and human-readable sizes, **including hidden entries**

{: .tip}
> - You can give `mkdir` more than one argument at once.
> - Not sure about an option? Use `--help`, for example `tail --help` or `ls --help`.

<details markdown="block">
<summary>One possible solution</summary>

```bash
# 1.
pwd
# ~ stands for your home directory, /home/$USER

# 2.
cd /home/$USER/2026-bioinf-ToT-EPIC-Namibia/docs

# 3.
mkdir -p ../scratch
cd ../scratch

# 4.
mkdir kp_project
cd kp_project
mkdir data results logs data/metadata
touch README.md

# 5.
echo "Klebsiella pneumoniae project, Windhoek 2026" > README.md

# 6.
echo "Author: Franziska" >> README.md

# 7.
nano README.md
# add the line, then Ctrl + O, Enter, Ctrl + X

# 8.
tail -n 2 README.md

# 9.
cd
cd -

# 10.
ls -lha
```

In step 6 you need `>>`. With `>` the first line would be overwritten.

</details>

---

## 2. Mini challenge: Inspect the assembly

You received the compressed genome assembly `ERR16718636.fna.gz` and two tables. Copy them into your project and take a first look, without opening an editor.

1. copy all three files from `data/tutorial_data` into `kp_project/data` with **one** command
2. list the files in `kp_project/data` with human-readable sizes, **largest file first**
3. move `bactopia_samplesheet.txt` into `data/metadata` and rename it to `samplesheet.tsv` in one step
4. show the first 3 lines of the assembly **without** uncompressing it
5. count the contigs in the assembly (each contig starts with a header line beginning with `>`)
6. show the header lines of the **first five** contigs
7. browse the compressed assembly page by page and quit
8. uncompress the assembly but **keep** the compressed file, then compare both sizes
9. remove the uncompressed file again
10. write the number of contigs into `README.md` as a new line, e.g. `Contigs: 44`

{: .tip}
> - In step 1, `*` matches all files in a directory, e.g. `data/tutorial_data/*`.
> - In step 7, you search inside `zless` the same way as inside `less`.

<details markdown="block">
<summary>One possible solution</summary>

```bash
# Start
cd ~/2026-bioinf-ToT-EPIC-Namibia/scratch/kp_project

# 1.
cp ~/2026-bioinf-ToT-EPIC-Namibia/data/tutorial_data/* data/

# 2.
ls -lhS data

# 3.
mv data/bactopia_samplesheet.txt data/metadata/samplesheet.tsv

# 4.
zcat data/ERR16718636.fna.gz | head -n 3

# 5.
zgrep -c "^>" data/ERR16718636.fna.gz
# 44

# 6.
zgrep "^>" data/ERR16718636.fna.gz | head -n 5

# 7.
zless data/ERR16718636.fna.gz
# type /ERR16718636_00044, press Enter, then q

# 8.
gunzip -k data/ERR16718636.fna.gz
ls -lh data
# about 5.3M uncompressed vs. 1.6M compressed

# 9.
rm data/ERR16718636.fna

# 10.
echo "Contigs: 44" >> README.md
cat README.md
```

</details>

{: .task}
> Stretch task: the header lines contain the contig length, e.g. `len=549035`. Show only the lengths of the first five contigs. Hint: look at the `-d` option in `cut --help`. The fields in the header are separated by spaces.

<details markdown="block">
<summary>One possible solution</summary>

```bash
zgrep "^>" data/ERR16718636.fna.gz | cut -d " " -f 2 | head -n 5
```

</details>

---

## 3. Mini challenge: Write an AMR summary

Your colleague ran AMRFinderPlus on all samples. Answer the questions below from the command line and collect the results in `results/amr_summary.txt`.

1. show the header line of `data/metadata/samplesheet.tsv` and count how many samples it contains (the header does not count)
2. check that all samples belong to the same species
3. show the first line of `data/amrfinderplus.tsv`. Which column number holds the sample `Name`, and which holds the `Element symbol`?
4. how many hits are in the file in total, and how many for sample `ERR16718636`?
5. list the carbapenem resistance genes (lines containing `CARBAPENEM`) with their sample name
6. does **every** sample carry at least one carbapenem resistance gene? Answer with one pipeline.
7. save the unique carbapenem resistance genes, with how often they occur, to `results/amr_summary.txt`
8. add the number of hits per sample to the same file, without overwriting it. Sort it so the sample with the most hits is on the last line.
9. view `results/amr_summary.txt` and make a backup copy of the whole `results` directory called `results_backup`
10. clean up: remove `results_backup` and list the project to check your final result

{: .tip}
> - `sort` sorts text alphabetically by default. Check `sort --help` for the option that sorts by **numeric** value.
> - Tab-separated columns are hard to count by eye. `head -n 1 data/amrfinderplus.tsv | tr '\t' '\n' | cat -n` prints one numbered column name per line.

<details markdown="block">
<summary>One possible solution</summary>

```bash
# Start
cd ~/2026-bioinf-ToT-EPIC-Namibia/scratch/kp_project

# 1.
head -n 1 data/metadata/samplesheet.tsv
grep -c "ERR" data/metadata/samplesheet.tsv
# 8 samples

# 2.
cut -f 4 data/metadata/samplesheet.tsv | sort | uniq -c
# 8 Klebsiella pneumoniae (+ 1 "species" from the header)

# 3.
head -n 1 data/amrfinderplus.tsv
# Name is column 1, Element symbol is column 7

# 4.
wc -l data/amrfinderplus.tsv
# 333 lines -> 332 hits
grep -c "ERR16718636" data/amrfinderplus.tsv
# 28

# 5.
grep "CARBAPENEM" data/amrfinderplus.tsv | cut -f 1,7

# 6.
grep "CARBAPENEM" data/amrfinderplus.tsv | cut -f 1 | sort | uniq | wc -l
# 8 -> yes, all eight samples

# 7.
grep "CARBAPENEM" data/amrfinderplus.tsv | cut -f 7 | sort | uniq -c > results/amr_summary.txt

# 8.
cut -f 1 data/amrfinderplus.tsv | sort | uniq -c | sort -n >> results/amr_summary.txt
# ERR16718619 has the most hits (63)

# 9.
less results/amr_summary.txt
cp -r results results_backup

# 10.
rm -r results_backup
ls -lh
ls results
```

Did you try `wc -l` in step 1? It prints `8`, although the file has a header and eight samples. `wc -l` counts line breaks, and the last line of this file has none. Counting the sample IDs with `grep -c` avoids that trap.

In step 8, the header `Name` appears with count 1. Bonus: find a way to leave it out (try `grep --help` and look at `-v`).

</details>

{: .discussion}
> - In step 6, what happens if you leave out `sort` before `uniq`?
