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
* **Using your own laptop**: Connect to the [Wifi](#wifi) and follow the [setup guide](#using-your-own-laptop) to install the necessary software and download the data. 

{: .tip }
> To paste into the terminal, press: 
> `Ctrl` + `Shift` + `V` (Linux) or 
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

### Data
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

## Using your own laptop

Please follow these steps to set up your working environment.

### Workshop directory

Clone the workshop directory:
```bash
cd && git clone https://github.com/rki-mf1/2026-bioinf-ToT-EPIC-Namibia.git
```

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

Open VSCodium, open the directory `2026-HSPA-Autumn-School` in VSCodium (Open Folder...) and continue the setup there.

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

Follow the prompts and answer `yes` when asked to initialize conda.

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
