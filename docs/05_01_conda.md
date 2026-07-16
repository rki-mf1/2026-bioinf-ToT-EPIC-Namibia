---
title: Conda
parent: Software Managment
nav_order: 1
nav_exclude: false
permalink: /conda/
---

![conda_logo](https://upload.wikimedia.org/wikipedia/commons/e/ea/Conda_logo.svg)

---

# Conda

## 🎯 Learning objectives

By the end of this practical, you should be able to:

- understand why software environments are important in bioinformatics
- list available Conda environments
- create a new Conda environment
- activate and deactivate Conda environments
- check which software is available inside an environment
- understand why a tool may not be available before activating the correct environment

---

## Overview

Bioinformatics tools often depend on very specific software versions. Installing all tools into one large system environment can quickly become messy, difficult to reproduce, and sometimes impossible to maintain.

This is where **Conda environments** are useful.

A Conda environment is like a separate software box. Each environment can contain a specific set of tools and versions. For example, one environment may contain `bcftools`, another may contain `fastp`, and another may contain tools for genome assembly.

In this practical, we will create and use a Conda environment for `fastp`.

---

## 1. List all available conda environments

First, let’s check which Conda environments are already available.

```bash
conda env list
```
You should see a list of environments. The **currently active environment** is marked with an asterisk (`*`).

Example output:

```bash
# conda environments:
#
base                  *  /home/user/miniconda3
```

---

## 2. Create Conda environment with `fastp`

Now we can create the environment.

```bash
conda create -n fastp -c bioconda fastp
```

Here, we use two important parameters:

- `-n fastp` gives the new environment the name **fastp**
- `-c bioconda` specifies channel from which to install fastp

---

## 3. Check that the environment was created

Now list the available Conda environments again.

```bash
conda env list
```
You should now see an environment called **fastp**.

🎉 Congratulations! You have created your first Conda environment in this workshop.

---

## 4. Try running fastp before activating the environment

The `conda activate` command lets you switch to a specific environment. 
You can use any of the environments that were listed in the output of `conda env list` command. 
Once you activate a environment you will be able to use the software that is installed in that environment.

```bash
# try to use fastp
fastp
```

Before activating the environment, let’s try running fastp.

```bash
fastp
```

You may see an error message similar to this:

```bash
Command 'fastp' not found
```
This is expected.

The software was installed inside the fastp Conda environment, but that environment is not active yet.

---

## 5. Activate the Conda environment

To use software installed inside a Conda environment, we first need to activate it.

```bash
conda activate fastp
```

Your terminal prompt may change and show the active environment name:

```bash
(fastp) user@computer:~$
```

Now try running fastp again.

```bash
fastp
```

This time, you should see the fastp help message instead of an error.

---

## 6. Check which version of fastp is installed

It is good practice to check software versions, especially when working on reproducible analyses.

```bash
fastp --version
```

---

## 7. List software installed in the active environment

You can inspect what is installed in the current Conda environment with:

```bash
conda list
```

---

## 8. Deactivate the environment

When you are finished using an environment, you can deactivate it.

```bash
conda deactivate
```

{: .discussion}
> Try to answer the following questions:
>
> - Which command lists all available Conda environments?
> - Which command activates the `fastp` environment?
> - Why did `fastp` not work before activating the environment?
> - Which command shows the installed version of `fastp`?

---

## 📌 Summary

In this practical, you used the following Conda commands:

| Command | Purpose |
|---|---|
| `conda env list` | List available Conda environments |
| `conda create -n fastp -c bioconda fastp` | Create a new environment |
| `conda activate fastp` | Activate the `fastp` environment |
| `conda list` | List installed packages in the active environment |
| `conda deactivate` | Leave the active environment |

---