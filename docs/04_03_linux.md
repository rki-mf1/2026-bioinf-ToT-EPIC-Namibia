---
title: Linux - File Management
parent: Linux
nav_order: 3
nav_exclude: false
permalink: /linux_file_management/
---

# Linux File Management

---

## 🎯 Learning objectives

By the end of this lesson, you should be able to:

- Create, copy, move, rename, and remove files and directories
- Write text to files with `echo`, `>` and `>>`, and edit files with `nano`
- View file contents with `cat`, `head`, `tail`, and `less`

{: .note}
This lesson builds on [Navigation](04_02_linux.md).

---

## 1. Create files and directories

{: .tip}
> Naming files and directories in the Linux command line.
> 
> Good file and directory names make your work easier, especially when using tab completion, scripts, and command-line tools.
> 
> - Do not use spaces or special characters such as `<`, `>`, `|`, `\`, `:`, `(`, `)`, `&`, `;`, `?`, or `*`.
> - Do not begin names with a dash `-` or a full stop `.`.
> - Use letters, numbers, dashes `-`, and underscores `_`.
> - Use a full stop `.` mainly for file extensions, such as `.pdf`, `.txt`, or `.fastq.gz`.
> - Linux is case-sensitive, so `my_first_file.txt`, `My_First_File.txt`, and `my_FIRST_file.txt` are three different files.

Create a safe practice area in the workshop repository and move into it:

```bash
cd ~/2026-bioinf-ToT-EPIC-Namibia
mkdir -p scratch
cd scratch
pwd
```

{: .note}
The option `-p` tells `mkdir` not to complain if the directory already exists.

Create a few files and directories:

```bash
touch notes.txt
touch copy_me.txt
mkdir drafts
mkdir results
ls -lh
```

Add text to a file:

```bash
echo "Hello Bioinformatics" > notes.txt
cat notes.txt
```

Append another line:

```bash
echo "Linux is powerful." >> notes.txt
cat notes.txt
```

{: .discussion}
> - What is the difference between `>` and `>>`?
> - Why is it useful to keep practice work in a separate directory?

Create a new file and open it in `nano`:

```bash
touch my_text_file.txt
nano my_text_file.txt
```

Inside `nano`, try the following:

- write two or three lines (e.g. tell us how much you like whales 🐋)
- save with `Ctrl + O`
- exit with `Ctrl + X`

Show the result:

```bash
cat my_text_file.txt
```

### Useful nano shortcuts

In the command overview at the bottom of the screen when in `nano`, `^` indicates Ctrl and `M-` indicates Alt. For example, `^O` means `Ctrl + O`.

Useful nano shortcuts:

- `Ctrl + O` (`^O`) — write/save the file
- `Ctrl + X` (`^X`) — exit nano
- `Ctrl + W` (`^W`) — search within the file
- `Ctrl + K` (`^K`) — cut the current line
- `Ctrl + U` (`^U`) — paste the cut text

---

## 2. Copy files and directories

Copy a file into another file:

```bash
cp copy_me.txt copy_me_backup.txt
ls
```

Copy a file into a directory:

```bash
cp notes.txt drafts/
ls drafts
```

Copy a directory **recursively**:

```bash
cp -r drafts drafts_copy
ls
```

{: .discussion}
> - When do you need `-r` with `cp`?
> - What is the difference between copying a file and copying a directory?

---

## 3. Move and rename files

Move a file into another directory:

```bash
mv copy_me_backup.txt results/
ls results
```

Rename a file:

```bash
mv copy_me.txt renamed_file.txt
ls
```

Move and rename at the same time:

```bash
mv renamed_file.txt drafts/final_notes.txt
ls drafts
```

---

## 4. Remove files and directories

Remove a file:

```bash
rm drafts/final_notes.txt
ls drafts
```

Remove a directory and everything inside it:

```bash
rm -r drafts_copy
ls
```

{: .caution }
> Be careful with `rm`, especially with `rm -r`. Deleted files do not go to a recycle bin.

---

## 5. View file contents

Use the file you already created:

```bash
cat notes.txt
```

Show only the first lines:

```bash
head notes.txt
```

Show only the last lines:

```bash
tail notes.txt
```

Open it page by page:

```bash
less notes.txt
```

### Useful `less` controls

- `q` — quit
- `/` followed by a pattern — search inside the file
- `n` — next match
- `p` — previous match
- `Space` — next page

{: .discussion}
> - When is `less` better than `cat`?
> - Which commands are more useful for large files?


---

## 6. Mini challenge

In `scratch` directory you created earlier, do the following:

1. create a directory called `project_demo` and move into it
2. create a file called `readme.txt` inside it
3. write one line into that file (e.g. "I love whales")
4. copy `readme.txt` file and name it `readme_copy.txt`
5. rename `readme_copy.txt` to `readme_backup.txt`
6. show the first lines of the original `readme.txt` file

One possible solution:

```bash
# 1.
cd ~/2026-bioinf-ToT-EPIC-Namibia/scratch
mkdir project_demo
cd project_demo

# 2. and 3.
echo "I love whales" > readme.txt

# 4.
cp readme.txt readme_copy.txt

# 5.
mv readme_copy.txt readme_backup.txt

# 6.
head readme.txt
```


---

---

## 📌 Summary

| Command | Purpose |
| --- | --- |
| `touch` | Create an empty file |
| `echo "text" > file` | Write text to a file (overwrites it) |
| `echo "text" >> file` | Append text to a file |
| `nano` | Edit a text file in the terminal |
| `mkdir` | Create a directory |
| `cp` | Copy files |
| `cp -r` | Copy directories recursively |
| `mv` | Move or rename files or directories |
| `rm` | Remove files |
| `rm -r` | Remove directories recursively |
| `cat` | Print a file |
| `less` | View a file page by page |
| `head` | Show first lines |
| `tail` | Show last lines |
