---
title: Setup
nav_order: 3
nav_exclude: false
has_children: false
has_toc: false
permalink: /setup/
---

# Recommended setup

Throughout this workshop, we will rely on GitHub and Conda. Before we begin, let’s set them up.

---

## Install Git

Git is used to download repositories and track changes to files.

You should work from a cloned copy of [this](https://github.com/rki-mf1/2026-bioinf-ToT-EPIC-Namibia) in your home directory (`~`).

To clone the repository, you need to have git installed. You can check whether Git is already available by opening a terminal and running:

```
git --version
```

If Git is not installed, install it on Ubuntu or WSL with:

```bash
sudo apt update
sudo apt install -y git
```

Check the installation:

```bash
git --version
```

---

## Clone the workshop GitHub repository

```bash
cd ~
git clone https://github.com/rki-mf1/2026-bioinf-ToT-EPIC-Namibia.git
cd ~/2026-bioinf-ToT-EPIC-Namibia
```

---

## Install Miniforge

[Miniforge](https://github.com/conda-forge/miniforge) provides `conda` and `mamba` through the community-maintained `conda-forge` channel. It is a fully open-source distribution and avoids reliance on Anaconda's default package repositories, whose use may be subject to commercial licensing terms.

Download and run the installer:

```bash
wget -O Miniforge3.sh \
  "https://github.com/conda-forge/miniforge/releases/latest/download/Miniforge3-$(uname)-$(uname -m).sh"

bash Miniforge3.sh
```

Follow the prompts, allow Conda to be initialised, and reopen the terminal.

Check the installation:

```bash
conda --version
mamba --version
```