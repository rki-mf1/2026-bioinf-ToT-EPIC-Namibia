---
title: Bash scripts
parent: Linux
nav_order: 5
nav_exclude: false
permalink: /linux_files/
---

# Viewing, Editing, Compressing, and Searching Files

---

## 🎯 Learning objectives

By the end of this lesson, you should be able to:

- write and run Bash scripts

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

## 1. Write your first Bash script

Create a directory for scripts:

```bash
cd ~/2026-bioinf-ToT-EPIC-Namibia/scratch
mkdir -p scripts
cd scripts
```

Create a small script called `create_project.sh`:

```bash
nano create_project.sh
```

Paste this content:

```bash
#!/bin/bash

TARGET_DIR=$1
PROJECT_NAME=$2

mkdir -p "$TARGET_DIR/$PROJECT_NAME"
mkdir -p "$TARGET_DIR/$PROJECT_NAME/data"
mkdir -p "$TARGET_DIR/$PROJECT_NAME/data/raw_data"
mkdir -p "$TARGET_DIR/$PROJECT_NAME/analyses"
mkdir -p "$TARGET_DIR/$PROJECT_NAME/scripts"
mkdir -p "$TARGET_DIR/$PROJECT_NAME/docs"
```

Save and exit.

### What this script introduces

- `#!/bin/bash` — the shebang; tells the system to use Bash
- `$1` — first argument
- `$2` — second argument
- `mkdir -p` — create directories, including parent directories if needed

---

## 2. Make the script executable and run it

Give the script execute permission:

```bash
ls -lh
chmod +x create_project.sh
ls -lh
```

Run it explicitly with Bash:

```bash
bash create_project.sh ~/2026-bioinf-ToT-EPIC-Namibia/scratch/scripts demo_project
```

Run it directly:

```bash
./create_project.sh ~/2026-bioinf-ToT-EPIC-Namibia/scratch/scripts demo_project_2

# or

./create_project.sh . demo_project_3
```

Check the results:

```bash
ls demo_project
```

{: .discussion}
> - What is the difference between `bash create_project.sh ...` and `./create_project.sh ...`?
> - What happens if the script is not executable?

---

## 📌 Summary

| Command | Purpose |
| --- | --- |
| `chmod +x` | Make a file executable |
| `bash script.sh` | Run a script with Bash |
| `./script.sh` | Run an executable script directly |
