---
title: Navigation
parent: Linux
nav_order: 2
nav_exclude: false
permalink: /linux_navigation/
---

# Navigation on the Command Line

---

{: .objectives }
> By the end of this lesson, you should be able to:
>
> - Describe the file system tree and distinguish absolute from relative paths
> - Navigate the file system using `cd`, `pwd` and `ls`, using both absolute and relative paths


{: .note}
This lesson builds on [First Commands and Getting Help](04_01_linux.md).

---

## 1. Moving around the filesystem

Open a terminal and move into the workshop repository:

```bash
cd ~/2026-bioinf-ToT-EPIC-Namibia
pwd
```

{: .tip}
Type `cd ~/2026` and press Tab to autocomplete the path.

Now try the following:

```bash
cd data
pwd

cd ..
pwd

cd ~
pwd

cd ~/2026-bioinf-ToT-EPIC-Namibia/data
pwd

cd -
pwd
```

### 🗝️ Key ideas

- `~` = your home directory
- `.` = the current directory
- `..` = the parent directory
- `cd` = change directory, accepts paths as arguments
- `cd ~` or  `cd` = change to home directory
- `cd ..` = change to parent directory 
- `cd -` = change to the directory you were in previously

---

## 2. Absolute and relative paths

An absolute path starts from the filesystem root `/`.

Example: `/home/$USER/2026-bioinf-ToT-EPIC-Namibia/data`

A relative path starts from where you are right now.

Example workflow:

```bash
cd ~/2026-bioinf-ToT-EPIC-Namibia
ls data

cd data
ls .
ls ..
```

{: .discussion}
> Run these commands and explain why they work:

```bash
cd ~/2026-bioinf-ToT-EPIC-Namibia
ls data

cd data
ls ..

cd ..
cd ./data
```

---

## 3. Practice: navigating the file system

Work through the exercises at your own pace. You do not need to finish all of them. Start with the warm-up, then continue as far as you get.

After every `cd`, check where you are with `pwd`. Use Tab to complete paths.

{: .note}
The folder `data/raw_data/kp_ont` only exists if you have followed the setup instructions. 

### Warm-up

**Exercise 1: Where am I?**

Open a new terminal and run `pwd`. Which directory are you in? Where is it in the file system tree?

<details markdown="block">
<summary>Solution</summary>

```bash
pwd
```

A new terminal starts in your home directory, `/home/$USER`. This is the same as `~`.

</details>

**Exercise 2: Go to the workshop folder with an absolute path**

Move into the workshop folder using an absolute path. Check with `pwd`.

<details markdown="block">
<summary>Solution</summary>

```bash
cd /home/$USER/2026-bioinf-ToT-EPIC-Namibia
pwd
```

An absolute path always starts with `/`. It works no matter where you are.

</details>

**Exercise 3: Look without moving**

You are in the workshop folder. List what is in `data` without moving into it. Then check with `pwd` that you have not moved.

<details markdown="block">
<summary>Solution</summary>

```bash
ls data
pwd
```

`ls` takes a path as an argument. You can look into a directory without going there.

</details>

**Exercise 4: Two levels down in one step**

From the workshop folder, move into `tutorial_data` with one single `cd` command. How many files are in it?

<details markdown="block">
<summary>Solution</summary>

```bash
cd data/tutorial_data
ls
```

There are three files: `ERR16718636.fna.gz`, `amrfinderplus.tsv` and `bactopia_samplesheet.txt`.

</details>

### Core practice

**Exercise 5: Up and over**

You are in `data/tutorial_data`. Move into `docs` with one relative path.

<details markdown="block">
<summary>Solution</summary>

```bash
cd ../../docs
pwd
```

The first `..` takes you up to `data`, the second up to the workshop folder. From there you go into `docs`.

</details>

**Exercise 6: List a directory somewhere else**

You are in `docs`. Without leaving `docs`, list the contents of `prodigal_training_files`.

<details markdown="block">
<summary>Solution</summary>

```bash
ls ../data/prodigal_training_files
```

You should see one file: `Klebsiella_pneumoniae.trn`.

</details>

**Exercise 7: Visit the root**

Move to the root of the file system and list its contents. Can you find `home`? Then go back to your home directory with the shortest command you know.

<details markdown="block">
<summary>Solution</summary>

```bash
cd /
ls
cd
pwd
```

`cd` without an argument takes you home. `cd ~` does the same.

</details>

**Exercise 8: Predict, then check**

Write down what `pwd` will print **before** you run the commands:

```bash
cd ~/2026-bioinf-ToT-EPIC-Namibia/data/tutorial_data
cd ../..
pwd
```

Now run them. Was your prediction correct?

<details markdown="block">
<summary>Solution</summary>

`/home/$USER/2026-bioinf-ToT-EPIC-Namibia`

Two times `..` takes you up two levels: from `tutorial_data` to `data`, then to the workshop folder.

</details>

**Exercise 9: Jump back and forth**

Go to `data/tutorial_data` in the workshop folder. Then go to `/usr/bin`. Now run `cd -` twice and check `pwd` after each one. What does `cd -` do?

<details markdown="block">
<summary>Solution</summary>

```bash
cd ~/2026-bioinf-ToT-EPIC-Namibia/data/tutorial_data
cd /usr/bin
cd -
pwd
cd -
pwd
```

`cd -` takes you back to the directory you were in before. Running it again takes you back again, so you switch between two places.

</details>

**Exercise 10: Fix the error**

You are in `docs` and run:

```bash
cd data/tutorial_data
```

You get `No such file or directory`. Why? Fix it in two ways: once with a relative path, once with an absolute path.

<details markdown="block">
<summary>Solution</summary>

A relative path starts from where you are. There is no `data` inside `docs`, so the path does not exist from here.

```bash
# relative
cd ../data/tutorial_data

# absolute
cd /home/$USER/2026-bioinf-ToT-EPIC-Namibia/data/tutorial_data
```

</details>

### Challenge

**Exercise 11: Three ways to the same place**

You are in `docs`. Write three different `cd` commands that all take you to `data/tutorial_data`. Test each one (go back to `docs` in between).

<details markdown="block">
<summary>Solution</summary>

```bash
cd ../data/tutorial_data                                          # relative
cd ~/2026-bioinf-ToT-EPIC-Namibia/data/tutorial_data              # starting from ~
cd /home/$USER/2026-bioinf-ToT-EPIC-Namibia/data/tutorial_data   # absolute
```

Which one would still work if you were in a different directory?

</details>

**Exercise 12: Find the file size**

Use `ls --help` to find the options that show file sizes in a human-readable format. Then show the size of `ERR16718636.fna.gz` from your home directory, without moving.

<details markdown="block">
<summary>Solution</summary>

```bash
cd ~
ls -lh 2026-bioinf-ToT-EPIC-Namibia/data/tutorial_data/ERR16718636.fna.gz
```

`-l` shows the long format with the size, `-h` makes the size human-readable (for example `1.4M` instead of a long number).

</details>

**Exercise 13: Hidden entries**

In the workshop folder, compare `ls` with `ls -a`. What extra entries do you see? What are `.` and `..`?

<details markdown="block">
<summary>Solution</summary>

```bash
cd ~/2026-bioinf-ToT-EPIC-Namibia
ls
ls -a
```

`ls -a` also shows hidden entries, whose names start with a full stop, for example `.git` and `.gitignore`. `.` is the current directory and `..` is the parent directory.

</details>

**Exercise 14: The longest way up**

You are in `data/raw_data/kp_ont`. What is the shortest relative path to `docs`? Write it down first, then test it.

<details markdown="block">
<summary>Solution</summary>

```bash
cd ~/2026-bioinf-ToT-EPIC-Namibia/data/raw_data/kp_ont
cd ../../../docs
pwd
```

Three levels up (`kp_ont` → `raw_data` → `data` → workshop folder), then into `docs`.

</details>

---

## 📌 Summary

| Concept or command | Meaning |
| --- | --- |
| `pwd` | Show the current directory |
| `ls <PATH>` | List files and folders at a path |
| Absolute path | Starts at the filesystem root (`/`) |
| Relative path | Starts at the current directory (`.`) |
| `~` | Home directory |
| `.` | Current directory |
| `..` | Parent directory |
| `cd <PATH>` | Change directory |
| `cd ~` or `cd` | Go to your home directory |
| `cd ..` | Go to the parent directory |
| `cd -` | Return to the previous directory |
