---
title: Conda
parent: Software Management
nav_order: 1
nav_exclude: false
permalink: /conda/
---

![conda_logo](https://upload.wikimedia.org/wikipedia/commons/e/ea/Conda_logo.svg)

---

{: .objectives }
> By the end of this practical, you should be able to:
>
> - understand why software environments are important in bioinformatics
> - list available Conda environments
> - create a new Conda environment
> - activate and deactivate Conda environments
> - check which software and versions are available inside an environment
> - export a Conda environment to a YAML file for sharing
> - create a Conda environment from a YAML file and give it a custom name
> - understand why a tool may not be available before activating the correct environment

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
base                  *  /home/user/miniforge3
```

---

## 2. Create Conda environment with `fastp`

Now we can create the environment.

```bash
conda create -n fastp -c conda-forge -c bioconda fastp
```

Here, we use two important parameters:

- `-n fastp` gives the new environment the name **fastp**
- `-c conda-forge -c bioconda` specifies channel from which to install fastp

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
Once you activate an environment you will be able to use the software that is installed in that environment.

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

---

## 9. Export the environment so it can be shared

Export the environment to a YAML file so that you can share it with colleagues. Using this file they can recreate the environment and its package versions.

```bash
conda env export -n fastp -f fastp.yml --no-builds
```

{: .note }
> `--no-builds` avoids recording platform-specific build strings, which can make the file easier to use on another computer.
> Conda may still include platform-specific packages, so recreation on a different operating system is not always guaranteed to be identical.

---

## 10. Create an environment from a YAML file

To create an environment from an `environment.yml` file, run:

```bash
conda env create -f environment.yml
```

{: .note }
> You can specify `-n` when creating the environment from a YAML file to define the environment’s name.
> If you omit `-n`, Conda uses the name in the YAML file.
> If the YAML file does not specify a name, you should provide it with `-n`

Now, for the sake of practicing, let's create new environment using YAML file:

```bash
conda env create -f fastp.yml -n fastp_from_yml
```

---

## 11. Remove newly created environment

After checking that the environment created from the YAML file works correctly, you can delete it. 

```bash
conda activate fastp_from_yml

fastp --help

conda deactivate

conda env remove -n fastp_from_yml
```

---

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
| `conda create -n fastp -c conda-forge -c bioconda fastp` | Create a new environment with fastp |
| `conda activate fastp` | Activate the `fastp` environment |
| `conda list` | List packages installed in the active environment |
| `conda env export -n fastp -f fastp.yml --no-builds` | Export the `fastp` environment to a YAML file |
| `conda env create -f fastp.yml -n fastp_from_yml` | Create an environment from a YAML file with a specified name |
| `conda deactivate` | Deactivate the environment |
| `conda env remove -n fastp_from_yml` | Remove the environment |