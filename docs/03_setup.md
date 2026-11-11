---
title: Setup
nav_order: 3
nav_exclude: false
has_children: false
has_toc: false
permalink: /setup/
---

# Setup

There are two ways to take part in the workshop:

* **Using a workshop laptop**: Everything is already installed. Work through the [checklist](#using-a-workshop-laptop).
* **Using your own laptop**: Follow the [setup guide](#using-your-own-laptop) to install the necessary software and download the data. 
* **Using your own Windows laptop**: First [set up WSL](#windows-laptops-set-up-wsl-first), which gives you Ubuntu Linux inside Windows. Then follow the [setup guide](#using-your-own-laptop) inside the Ubuntu terminal.

{: .tip }
> To paste into the terminal, press: 
> `Ctrl` + `Shift` + `V` (Linux and WSL/Windows Terminal) or 
> `Cmd` + `V` (macOS).

---

## Wifi

Please connect to the Wifi.
The Wifi password will be communicated by your facilitators.

---

## Using a workshop laptop

Your laptop has already been set up for the workshop. You do not need to install anything. Before we start, please check that the required software and data are available. If a check fails, let a facilitator know.

After the workshop, you can use the [setup guide for your own laptop](#using-your-own-laptop) to repeat the workflow on your own machine.

### Checklist

- [ ] Wifi connected
- [ ] Workshop directory exists
- [ ] VSCodium opens
- [ ] Docker runs without `sudo`
- [ ] Git is installed
- [ ] Conda is installed, with `conda-forge` and `bioconda` channels
- [ ] Nextflow Conda environment exists
- [ ] Toy dataset is downloaded

### Run the checks

Open VSCodium and open a terminal (Terminal -> New Terminal in the top bar). Run the following checks.

**Workshop directory**

Clone the workshop directory:
```bash
cd && git clone https://github.com/rki-mf1/2026-bioinf-ToT-EPIC-Namibia.git
```

Verify that the workshop folder exists:
```bash
ls ~/2026-bioinf-ToT-EPIC-Namibia
```
If you see `data` and `docs`, you have successfully cloned the workshop directory.

**Toy dataset**

Follow the instruction [here](06_01_dataset.md) to download the toy datasets.
This might take a while, so please continue in a fresh terminal while the datasets are being downloaded.

**Docker**

```bash
docker run hello-world
```
You should see `Hello from Docker!`. If you get a `permission denied` error, your user is not in the `docker` group. Ask a facilitator.

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

**Data**
Verify that the data was successfully downloaded

```bash
ls ~/2026-bioinf-ToT-EPIC-Namibia/data/raw_data
```

You should see `kp` and `kp_ont`. 

---

## Windows laptops: set up WSL first

The workshop tools run on Linux. On a Windows laptop, you can use the **Windows Subsystem for Linux (WSL)**. WSL runs a full Ubuntu Linux system inside Windows, so you can follow the same steps as on a Linux laptop.

{: .important }
> You need **administrator rights** on your laptop and **Windows 10 (version 2004 or newer) or Windows 11**. If you are unsure, ask a facilitator before you start.

### Install WSL and Ubuntu

Open **PowerShell as administrator**: click Start, type `PowerShell`, right-click **Windows PowerShell** and choose **Run as administrator**.

Install WSL together with Ubuntu:
```powershell
wsl --install
```

**Restart your laptop** when the installation has finished.

After the restart, Ubuntu opens automatically (otherwise, open **Ubuntu** from the Start menu). Finishing the installation can take a few minutes. You are then asked to create a Linux username and password.

{: .note }
> When you type your password, nothing appears on the screen. This is normal on Linux. Type the password and press `Enter`. Remember this password: you need it whenever you use `sudo`.

### Check the installation

In PowerShell (no administrator rights needed), run:
```powershell
wsl --list --verbose
```
You should see `Ubuntu` with `VERSION` `2`. If it shows `1`, run:
```powershell
wsl --set-version Ubuntu 2
```

### Update Ubuntu

In the Ubuntu terminal, run:
```bash
sudo apt update && sudo apt upgrade -y
```

### Check that systemd is enabled

Docker needs `systemd` inside WSL. Check the WSL configuration:
```bash
cat /etc/wsl.conf
```
You should see:
```
[boot]
systemd=true
```
If these lines are missing, add them:
```bash
printf '[boot]\nsystemd=true\n' | sudo tee -a /etc/wsl.conf
```
Then restart WSL. In PowerShell, run `wsl --shutdown` and open Ubuntu again.

### Notes for the next steps

Now continue with the [setup guide for your own laptop](#using-your-own-laptop). Run all commands **in the Ubuntu terminal**, not in PowerShell. Keep these WSL details in mind:

* **Log out and log back in** (Docker step): close all Ubuntu windows, run `wsl --shutdown` in PowerShell, then open Ubuntu again.
* **VSCodium**: install it inside Ubuntu as described below and start it from the Ubuntu terminal with `codium`. The window opens on your Windows desktop. If no window appears, run `wsl --update` in PowerShell and try again.
* **Where to keep your files**: work in your Linux home directory (`~`). Your Windows files are visible under `/mnt/c/`, but working there is much slower.

---

## Using your own laptop

Please follow these steps in order to set up your working environment. The dataset download is started early because it takes the longest. On a Windows laptop, [set up WSL](#windows-laptops-set-up-wsl-first) first and run all steps in the Ubuntu terminal.

### Git

[Git](https://git-scm.com/) is a version control system. It keeps track of changes to files over time, so you can see what changed, when, and by whom, and go back to earlier versions if needed. Git is widely used to share code, and platforms such as [GitHub](https://github.com/) host Git repositories online.

Check if you have Git installed:
```bash
git --version
```

Install Git with `apt`:
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

Start the dataset download now. It takes a while, so it runs while you install the remaining software.

Follow the instruction [here](06_01_dataset.md) to download the toy datasets.

{: .important }
> Keep this terminal open until the download has finished. Open a **new terminal** for the next steps (on WSL: open Ubuntu again from the Start menu).

### Editor

[VSCodium](https://vscodium.com/) is a free and open-source code editor. It is built from the same source code as Microsoft's Visual Studio Code, but without Microsoft's branding and telemetry. You can use it to browse and edit files, view scripts and configuration files, and run commands in a built-in terminal, all in one window.

In this workshop, we use VSCodium to open the workshop repository, look at the pipeline files and inspect results. You can use another text editor if you prefer, but VSCodium makes it easier to follow along.

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

Open VSCodium and open the directory `2026-bioinf-ToT-EPIC-Namibia` (File -> Open Folder...). You can continue the setup in the VSCodium terminal.

To open a terminal in VSCodium, select Terminal -> New Terminal in the top bar.

### Docker

[Docker](https://www.docker.com/) is a tool for running software in *containers*. A container bundles a program together with everything it needs to run, such as libraries, dependencies and the right versions of each. This means a tool behaves the same way on every computer, regardless of what else is installed.

In this workshop, we use Docker together with Nextflow. Each step of the pipeline runs inside its own container, which Nextflow downloads and starts automatically. You don't need to install the individual bioinformatics tools yourself.

Check if you have docker installed on your machine:
```bash
docker run hello-world
```

Remove any conflicting old packages. It is fine if `apt` reports that none of them are installed.
```bash
for pkg in docker.io docker-doc docker-compose docker-compose-v2 podman-docker containerd runc; do
  sudo apt remove -y $pkg
done
```

Install the prerequisites and add Docker's official GPG key:
```bash
sudo apt update
sudo apt install -y ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc
```

Add the Docker repository:
```bash
sudo tee /etc/apt/sources.list.d/docker.sources <<EOF
Types: deb
URIs: https://download.docker.com/linux/ubuntu
Suites: $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}")
Components: stable
Signed-By: /etc/apt/keyrings/docker.asc
EOF
```

Install Docker Engine:
```bash
sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

Start Docker and enable it at boot:
```bash
sudo systemctl enable --now docker
```

Allow your user to run Docker without `sudo`. Nextflow calls Docker as your normal user, so this step is required.
```bash
sudo usermod -aG docker $USER
```

**Log out and log back in** for the group change to take effect.

Verify the installation:
```bash
docker run hello-world
```

If you see `Hello from Docker!`, Docker is working.

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

Finally, work through the [checklist for workshop laptops](#checklist) and [run the checks](#run-the-checks). Skip the cloning step, as you have already cloned the workshop directory. If every check passes, your laptop is ready for the workshop.
