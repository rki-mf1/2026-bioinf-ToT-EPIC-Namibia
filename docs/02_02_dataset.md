[⬅ Back to main page](../README.md)

# Downloading datasets

> [!NOTE]
> The FASTQ files were already downloaded on the eserver, so you can use them, they are stored here: `/shared/fastq_files`. 

In case you do not now have access to this directory on the server, you can download it. To do so, we first need to install a few tools.

We will use **NCBI SRA Toolkit**. The reads are downloaded with `prefetch`, converted to FASTQ with `fasterq-dump`, and compressed with `pigz`.

```bash
# install SRA Toolkit and pigz
conda create -y -n sra-tools -c conda-forge -c bioconda sra-tools pigz

# install ncbi-datasets
conda create -y -n ncbi-datasets-cli conda-forge::ncbi-datasets-cli
```

We will continue working in the same directory as before.

```bash
# start from workshop directory
cd ~/2026-Workshop-HSPA-Tunisia

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
cd ~/2026-Workshop-HSPA-Tunisia
```

---

[Next tutorial](./03_fastp.md)

[⬅ Back to main page](../README.md)