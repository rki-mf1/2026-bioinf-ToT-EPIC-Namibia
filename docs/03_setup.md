---
title: Setup
nav_order: 3
nav_exclude: false
has_children: false
has_toc: false
permalink: /setup/
---

# Setup

There are three ways to take part in the workshop:

* **Using a workshop laptop**: Everything is already installed and the toy data is already downloaded. Double check by working through the [checklist](#using-a-workshop-laptop).
* **Using your own Linux laptop**: Follow the [setup guide](#using-your-own-laptop) to install the necessary software and download the data. You do not need WSL.
* **Using your own Windows laptop**: First [set up WSL](#windows-laptops-set-up-wsl-first), which gives you Ubuntu Linux inside Windows. Then follow the [setup guide](#using-your-own-laptop) in the terminal.

## Using a workshop laptop

Your laptop has already been set up for the workshop. You (most likely) do not need to install anything. Please check that the required software and data are available using the commands provided below. If a check fails, let a facilitator know. After the workshop, you can use the [setup guide for your own laptop](#using-your-own-laptop) to repeat the workflow on your own machine.

### Checklist

- [ ] Wifi connected
- [ ] VSCodium opens
- [ ] Workshop directory exists
- [ ] Git is installed
- [ ] Conda is installed, with `conda-forge` and `bioconda` channels
- [ ] Nextflow Conda environment exists
- [ ] Toy dataset is downloaded

### Wifi

Please connect to the Wifi. The password will be communicated by your facilitators.

### Run the checks

Open VSCodium and open a terminal (Terminal -> New Terminal in the top bar). Run the following checks by copy pasting the given command into your terminal.

{: .tip }
> To paste into the terminal, press `Ctrl` + `Shift` + `V`.

**Workshop directory**

Verify that the workshop folder exists:
```bash
ls ~/2026-bioinf-ToT-EPIC-Namibia
```
If you see `data` and `docs` among others, the workshop directory is in place.

**Toy dataset**

Check that the toy dataset is there:

```bash
ls ~/2026-bioinf-ToT-EPIC-Namibia/data/raw_data
```

You should see the folder `kp_ont`:

```
kp_ont
```

On some laptops you will see a second folder, `kp`, as well:

```
kp  kp_ont
```

Both outputs are fine. If `kp_ont` is missing, or you get the message `No such file or directory`, please inform a facilitator.

**Git**

```bash
git --version
```
You should see a version number.

**Conda**

```bash
conda --version
conda config --show channels
```
The first command should print a version number. The channel list should show `conda-forge` first and `bioconda` second.

**Nextflow**

```bash
conda env list
```
You should see an environment named `nextflow`.

---

## Windows laptops: set up WSL first

Ideally, this has already been done before the start of the workshop. The workshop tools run on Linux. On a Windows laptop, you can use the Windows Subsystem for Linux (WSL). WSL runs a full Ubuntu Linux system inside Windows.

{: .important }
> You need administrator rights on your laptop and Windows 10 (version 2004 or newer) or Windows 11. If you are unsure, ask a facilitator before you start.
> To check your Windows version, open Settings → System → About.

### Wifi

Please connect to the Wifi. The password will be communicated by your facilitators.

### Install WSL and Ubuntu

Follow this [guide](https://dev.to/brayandiazc/install-wsl-from-the-microsoft-store-111h) to install WSL and Ubuntu from the Microsoft Store.

When you open Ubuntu for the first time, you are asked to create a Linux username and password.

{: .note }
> When you type your password, nothing appears on the screen. This is normal on Linux. Type the password and press `Enter`. Remember this password: you need it whenever you use `sudo`.


Now continue with the [setup guide for your own laptop](#using-your-own-laptop).

---

## Using your own laptop

Please follow these steps in order to set up your working environment. Copy and paste the commands into the terminal. The dataset download is started as soon as possible because it takes the longest. On a Windows laptop, [set up WSL](#windows-laptops-set-up-wsl-first) first and run all steps in the Ubuntu terminal.

### Wifi

Please connect to the Wifi. The password will be communicated by your facilitators.

### Git

[Git](https://git-scm.com/) is a version control system. It keeps track of changes to files over time, so you can see what changed, when, and by whom, and go back to earlier versions if needed. Git is widely used to share code, and platforms such as [GitHub](https://github.com/) host Git repositories online.

Check if you have Git installed:
```bash
git --version
```

If you see a version number, continue with the next step. Otherwise, install Git with `apt`:
```bash
sudo apt update
sudo apt install -y git
```

Verify the installation:
```bash
git --version
```

### Workshop directory

Clone the workshop directory into your home directory:
```bash
cd && git clone https://github.com/rki-mf1/2026-bioinf-ToT-EPIC-Namibia.git
```

Verify that the workshop folder exists:
```bash
ls ~/2026-bioinf-ToT-EPIC-Namibia
```
You should see `data` and `docs`.

### Miniforge

[Miniforge](https://github.com/conda-forge/miniforge) provides `conda` and `mamba` through the community-maintained `conda-forge` channel. It is a fully open-source distribution and avoids reliance on Anaconda's default package repositories, whose use may be subject to commercial licensing terms.

Check if you already have conda or mamba:
```bash
conda --version
mamba --version
```

If you do not have it installed, you can install Miniforge.

Download the installer:
```bash
wget -O Miniforge3.sh \
  "https://github.com/conda-forge/miniforge/releases/latest/download/Miniforge3-$(uname)-$(uname -m).sh"
```

Run the installer:
```bash
bash Miniforge3.sh
```

Follow the prompts: press `Enter` to scroll through the licence, type `yes` to accept it, press `Enter` to keep the default install location, and answer `yes` when asked to initialize conda.

**Close and reopen the terminal** for the changes to take effect.

Remove the installer file:
```bash
rm Miniforge3.sh
```

Verify the installation:
```bash
conda --version
```

Add the `bioconda` channel after `conda-forge`. The `--append` option places a channel at the bottom of the list, so `conda-forge` stays the first priority.
```bash
conda config --append channels bioconda
```

Verify the channel order:
```bash
conda config --show channels
```

The output should list `conda-forge` first and `bioconda` second:
```
channels:
  - conda-forge
  - bioconda
```

### Toy dataset

Start the dataset download now. It takes a while, so it runs while you install the remaining software. Follow the instructions on the [Dataset page](06_01_dataset.md) to download only the ONT toy datasets.

{: .important }
> Keep this terminal open until the download has finished. Open a new terminal for the next steps.

### Editor

[VSCodium](https://vscodium.com/) is a free and open-source code editor. It is built from the same source code as Microsoft's Visual Studio Code, but without Microsoft's branding and telemetry. You can use it to browse and edit files, view scripts and configuration files, and run commands in a built-in terminal, all in one window.

In this workshop, we use VSCodium to open the workshop repository, look at files and inspect results. You can use another text editor if you prefer, but VSCodium makes it easier to follow along.

Choose one of the two options below.

#### Option 1: Install from the VSCodium website (recommended)

Follow the instructions for your operating system on the [VSCodium install page](https://vscodium.com/#install):

* Windows: download the installer from the [VSCodium releases](https://github.com/vscodium/vscodium/releases) (the file ending in `.exe`, e.g. `VSCodiumUserSetup-x64-<version>.exe`) and run it.
* Linux (Ubuntu): download the `.deb` file from the [VSCodium releases](https://github.com/vscodium/vscodium/releases) and install it by double-clicking it.

{: .note }
> Windows with WSL: if you installed VSCodium on Windows, its terminal opens PowerShell by default. Switch to Ubuntu: in the terminal panel, click the small arrow next to `+` and choose **Ubuntu (WSL)**. From now on, run all workshop commands in this Ubuntu terminal.

#### Option 2: Install with the command line

Run these commands in your (Ubuntu) terminal.

Add the GPG key of the VSCodium repository:
```bash
wget -qO - https://gitlab.com/paulcarroty/vscodium-deb-rpm-repo/raw/master/pub.gpg \
    | gpg --dearmor \
    | sudo dd of=/usr/share/keyrings/vscodium-archive-keyring.gpg
```

Add the repository:
```bash
echo -e 'Types: deb\nURIs: https://download.vscodium.com/debs\nSuites: vscodium\nComponents: main\nArchitectures: amd64 arm64\nSigned-by: /usr/share/keyrings/vscodium-archive-keyring.gpg' \
    | sudo tee /etc/apt/sources.list.d/vscodium.sources
```

On Ubuntu 23.10 or older, use this command instead:
```bash
echo 'deb [arch=amd64,arm64 signed-by=/usr/share/keyrings/vscodium-archive-keyring.gpg] https://download.vscodium.com/debs vscodium main' \
    | sudo tee /etc/apt/sources.list.d/vscodium.list
```

Install VSCodium:
```bash
sudo apt update
sudo apt install -y codium
```

Start VSCodium from the terminal:
```bash
codium
```

{: .note }
> Windows with WSL: the VSCodium window opens on your Windows desktop. If no window appears, ask a facilitator.

#### Open the workshop directory

Open VSCodium and open the directory `2026-bioinf-ToT-EPIC-Namibia` (File -> Open Folder...). You can continue the setup in the VSCodium terminal.

To open a terminal in VSCodium, select Terminal -> New Terminal in the top bar.

### Nextflow

[Nextflow](https://www.nextflow.io/) is a workflow manager. It runs multi-step bioinformatics pipelines and takes care of starting each step in its own container.

Create a Conda environment named `nextflow`:
```bash
conda create -y -n nextflow nextflow
```

Verify the installation:
```bash
conda activate nextflow
nextflow -version
conda deactivate
```
You should see a version number.

### Check your setup

Finally, work through the [checklist for workshop laptops](#checklist) and [run the checks](#run-the-checks). If every check passes, your laptop is ready for the workshop.
