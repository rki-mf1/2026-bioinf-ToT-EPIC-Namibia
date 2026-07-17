---
title: Dataset
parent: Data Managment
nav_order: 1
nav_exclude: false
permalink: /dataset/
---

# Downloading datasets

We will use the **NCBI SRA Toolkit** to download and convert the sequencing
data. The reads are downloaded with `prefetch`, converted to FASTQ with
`fasterq-dump`, and compressed with `pigz`.

```bash
# install SRA Toolkit and pigz
conda create -y -n sra-tools \
  -c conda-forge \
  -c bioconda \
  sra-tools pigz

# install NCBI Datasets for later tutorials
conda create -y -n ncbi-datasets-cli \
  conda-forge::ncbi-datasets-cli
```

---

## Illumina

This dataset contains paired-end Illumina whole-genome sequencing reads from eight *Klebsiella pneumoniae* isolates from wastewater ([Werner et al. 2026](https://www.frontiersin.org/journals/microbiology/articles/10.3389/fmicb.2026.1821458/full)). 

We will continue working in the same directory as before.

```bash
# start from workshop directory
cd ~/2026-bioinf-ToT-EPIC-Namibia

# activate conda environment
conda activate sra-tools

# create directory for raw data
mkdir -p data/raw_data/kp
cd data/raw_data/kp

# download reads from SRA
for acc in \
    ERR16718582 \
    ERR16718597 \
    ERR16718618 \
    ERR16718619 \
    ERR16718620 \
    ERR16718626 \
    ERR16718635 \
    ERR16718636
do
  echo "Downloading ${acc}"

  # download the SRA file
  prefetch "$acc" --output-directory .

  # convert SRA to FASTQ
  fasterq-dump "${acc}/${acc}.sra" \
    --split-files \
    --threads 4 \
    --outdir . \
    --temp .

  # compress FASTQ files
  pigz -p 4 "${acc}"*.fastq

  # remove downloaded SRA directory after successful conversion
  rm -r "$acc"
done

# go back to workshop dir
cd ~/2026-bioinf-ToT-EPIC-Namibia
```

---

## ONT

This dataset contains Oxford Nanopore whole-genome sequencing reads from
KPC-producing *Klebsiella pneumoniae* isolates described by
[Ozer et al. 2026](https://doi.org/10.1128/mra.01447-25).

{: .note }
Oxford Nanopore reads are single-end. Therefore, `fasterq-dump` is used
without the `--split-files` option.

We will continue working in the same directory as before.

```bash
# start from the workshop directory
cd ~/2026-bioinf-ToT-EPIC-Namibia

# activate the Conda environment
conda activate sra-tools

# stop the script if a command fails
set -euo pipefail

# create directories for raw data and temporary files
mkdir -p data/raw_data/kp_ont/tmp
cd data/raw_data/kp_ont

# download and rename the ONT reads
while read -r acc sample
do
  echo "Downloading ${acc} for ${sample}"

  # download the SRA file
  prefetch "$acc" --output-directory .

  # convert SRA to a single FASTQ file
  fasterq-dump "${acc}/${acc}.sra" \
    --threads 4 \
    --outdir . \
    --temp tmp

  # compress the FASTQ file
  pigz -p 4 "${acc}.fastq"

  # rename the file using the isolate name
  mv "${acc}.fastq.gz" "${sample}.fastq.gz"

  # remove the downloaded SRA directory
  rm -rf "$acc"

done <<'EOF'
SRR36316118 AR-1049-MV
SRR36316116 AR-1054-GC
SRR36316119 AR-1049-GC
SRR36316117 AR-1054
EOF

# remove the temporary directory
rm -rf tmp

# inspect the downloaded files
ls -lh

# return to the workshop directory
cd ~/2026-bioinf-ToT-EPIC-Namibia
```

The resulting files are:

```text
data/raw_data/kp_ont/
├── AR-1049-GC.fastq.gz
├── AR-1049-MV.fastq.gz
├── AR-1054.fastq.gz
└── AR-1054-GC.fastq.gz
```

{: .important }
The SRA files, uncompressed FASTQ files, and temporary conversion files may
coexist during downloading. Ensure that several gigabytes of free disk space
are available.
